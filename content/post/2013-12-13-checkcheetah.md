---
author: Rob
categories:
- Projects
date: "2013-12-13T12:02:52Z"
guid: http://robwilliams.me/?p=324
id: 324
title: 'CheckCheetah: A Rails Web App for Ordering and Paying at a Bar'
url: /2013/12/checkcheetah/
---
In a [previous entry](http://robwilliams.me/2013/09/update/ "Update"), I mentioned that much of my personal free time had been devoted to a closed-source programming effort for a startup I’m involved with. After we spent several months trying to sell the completed MVP and concept to prospective users, we didn’t have much luck. This idea of business and marketing and sales is new to us, so we didn’t expect to succeed right away. Even if we are never able to sell this, I think we learned several valuable lessons along the way. I’d like to share some of the technical insights learned while building the app.

### App Overview

CheckCheetah is a mobile web app which allows a customer at a bar or restaurant to see the menu, place an order, and then pay for the items & tip using their Amazon.com accounts. We knew the market was rather saturated, but we felt that by utilizing the Amazon payment method, we could offer a much quicker ordering process than competitors. The reason is because almost everyone already has their credit card details saved in Amazon, not to mention they trust Amazon with such financial details. On the business administrative end, the app allows the business to customize their menus in real time and even setup specials that would change the price of an item at a given time of day or day of the week.

### Technology Overview

Since this is a web app, we needed the ability to deliver a full-stack web solution. Rails was chosen because it has been used successfully at scale by [Bootstrap](http://getbootstrap.com/2.3.2/) was chosen to make responsive design easier (the site was designed to scale from large desktop down to iPhone 320px width responsively). We don’t have a ton of Javascript interaction, but where we did it was hand-coded in jQuery because at the time I had no experience with anything else.

Obviously, the site requires the capability to receive payments. We chose to use [Amazon Flexible Payment Services (FPS) API](https://aws.amazon.com/fps/) for this. If the state of the documentation is any indication, I feel like FPS is not one of the most well-supported or used services offered by Amazon, but it was the only payment system we found which offered the types of complex payment handling we wanted to do (more on this in Lessons Learned). It had the bonus side-effect of using the payment information most people already have stored in Amazon, while also freeing us from the need to be PCI compliant or worry about securing credit card numbers.

The other notable technology needed was a way to “push” updates to the user’s order screen when their drink or food was prepared. The pushing also worked in the opposite direction: new orders from customers would appear on the business side. At the time, Heroku didn’t support WebSockets, so we had to resort to a hack of sorts, using the Ruby server version of [Faye](http://faye.jcoglan.com/), along with a Javascript client, which in lieu of WebSocket support used a polling method.

### Lessons Learned

**PaaS Pros/Cons**

We decided to use Heroku for the obvious reasons: we did all this work in our free time with full-time jobs, so we didn’t have precious hours to configure AWS instances and worry about how to scale them if we had hundreds of users on a Friday night. The benefit of Heroku is that it works magically with Rails. For this particular application without much asset pipeline complications, deployments were quick and easy. Never once did a deployment fail, and we never had downtime issues. Being able to develop everything for free with 1 dyno is fairly hard to beat. The tooling around Heroku’s PostgreSQL is also amazing and we were confident it would scale with us, with things like [Follower Databases](https://devcenter.heroku.com/articles/heroku-postgres-follower-databases).

The downside is that when your PaaS provider doesn’t support WebSockets, you don’t have many options. As mentioned above, we chose to go with a Faye solution. We created a separate Heroku app running a Ruby (not Rails) based Faye server,and we used the Javascript client from our existing Rails app. This allowed “pushing” order status information to devices, but under the hood it was glorified polling. Our use case was ultra simple, establishing basically one pub/sub channel per order (for customer-facing updates), and one channel for each business (for business-facing updates). Getting Faye working on Heroku led to a number of challenges. The first breakthrough of actually getting it running was thanks to some code found in a [this Github repo](https://github.com/Hareramrai/fayeserver)).

However, I then ran into subtle intermittent problems where the updates wouldn’t get pushed. It turns out the issue is that the underlying private_pub gem (by legendary [commit history](https://github.com/robwil/private_pub/commits/master) of my Github fork of private_pub.

**Amazon FPS API**

For this app, we had some unique payment needs. The goal was to allow a customer to pay a business directly, but for us as the SaaS provider to take a percentage cut. This essentially meant that financial transactions involved three parties, instead of the usual two. This quickly excluded many of the common APIs from places like Square or Paypal. We also had extra requirements. For example, we didn’t want someone to have to go through the Amazon sign in and pick their credit card every time, if they were a repeat customer. This required the ability to “remember” past financial details. It turns out that Amazon FPS (Flexible Payment Services) allows both of these unique cases. By using the [“Marketplace” preset](https://amazonpayments.s3.amazonaws.com/FPS_ASP_Guides/FPS_Marketplace_Quick_Start.pdf), we could accomplish our three party system. By creating multi-use recipient tokens, we could save a customer –> business “session” for later use, by asking the user to authorize $500 (an arbitrary number we picked) ahead of time. It turns out that even for both of these somewhat complex use cases, FPS had a “preset” for us. I was highly impressed by the flexibility (it really lives up to its name), and realize that we only scratched the surface: FPS allows writing custom payment instructions in an XML-based DSL for even more advanced transactions.

FPS uses statuses for everything, and the state transitions were somewhat cumbersome. The API was fully asynchronous, so placing an order would return immediately with a status of “Pending”. This caused some challenges as we would have to poll for success, but Amazon has a solution to this. They allow you to setup an API endpoint where they will POST a request to you when the order reaches a terminal state. This was extremely useful, since we didn’t have to worry about managing the interim states, simply letting Amazon feed us a “success” or “error” at some time in the future. Designing our internal statuses around this led to quite a few business requirements discussions, but from a technical point of view it was fairly simple and avoided a lot of the complications most payment APIs might lead to.

We never reached the need to scale or use FPS heavily in production, so I can’t vouch for that side of it. However, I can say that from my experience the API was easy to use (there are some Ruby gems to make it easier, but even doing it manually is a fairly straight-forward API call with HMAC signing) and has an impressive array of features.

**PhoneGap Adventures**

We didn’t heavily invest in “native” mobile apps because the site never really took off, but we did do an exploratory Android app via PhoneGap. It literally was as easy as making a single HTML page with “window.location.href = http://our\_production\_url”, since our site was already designed to scale to device. This way of quickly wrapping a responsive app into a “native” app is very nice. However, we took it one step further and experimented with using Pusher to actually send real native push notifications to the Android device. All we needed to add was a quick Javascript hook into Pusher from the PhoneGap app, and then some very minor server-side code (to associate the device ID with the user account, and to send the actual push notifications). It was very impressive how easy it was to bolt real push notifications onto a responsive mobile web app, inside a native app container. I would definitely consider this option for future projects where true native functionality is not needed.

### Conclusion

You can check out a working version on our staging server [here](https://limitless-garden-3809.herokuapp.com). Note that the Faye server is currently not working on staging so the “push” updates won’t be working. The system gracefully supports manual refreshes in this case. It is also tied to the sandbox for Amazon FPS, so feel free to place an order using your Amazon account – it will come pre-baked with a fake credit card number and billing address.

Below are some screenshots of the system in action from customer viewpoint (on a phone) and business viewpoint (on desktop).

<div id='gallery-1' class='gallery galleryid-324 gallery-columns-3 gallery-size-thumbnail'>
  <dl class='gallery-item'>
    <dt class='gallery-icon portrait'>
      <a href='http://robwilliams.me/wp-content/uploads/2013/12/Screen-Shot-2014-02-01-at-15.01.03.png'>![image](http://robwilliams.me/wp-content/uploads/2013/12/Screen-Shot-2014-02-01-at-15.01.03.png)
    </dt>
  </dl>
  
  <dl class='gallery-item'>
    <dt class='gallery-icon portrait'>
      <a href='http://robwilliams.me/wp-content/uploads/2013/12/Screen-Shot-2014-02-01-at-15.01.15.png'>![image](http://robwilliams.me/wp-content/uploads/2013/12/Screen-Shot-2014-02-01-at-15.01.15.png)
    </dt>
  </dl>
  
  <dl class='gallery-item'>
    <dt class='gallery-icon portrait'>
      <a href='http://robwilliams.me/wp-content/uploads/2013/12/Screen-Shot-2014-02-01-at-15.01.34.png'>![image](http://robwilliams.me/wp-content/uploads/2013/12/Screen-Shot-2014-02-01-at-15.01.34.png)
    </dt>
  </dl>
  
  <br style="clear: both" />
  
  <dl class='gallery-item'>
    <dt class='gallery-icon portrait'>
      <a href='http://robwilliams.me/wp-content/uploads/2013/12/Screen-Shot-2014-02-01-at-15.02.07.png'>![image](http://robwilliams.me/wp-content/uploads/2013/12/Screen-Shot-2014-02-01-at-15.02.07.png)
    </dt>
  </dl>
  
  <dl class='gallery-item'>
    <dt class='gallery-icon landscape'>
      <a href='http://robwilliams.me/wp-content/uploads/2013/12/Screen-Shot-2014-02-01-at-15.36.57.png'>![image](http://robwilliams.me/wp-content/uploads/2013/12/Screen-Shot-2014-02-01-at-15.36.57.png)
    </dt>
  </dl>
  
  <br style='clear: both' />
</div>