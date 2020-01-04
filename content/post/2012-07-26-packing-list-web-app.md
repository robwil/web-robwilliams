---
author: Rob
categories:
- Projects
date: "2012-07-26T17:48:19Z"
guid: http://robwilliams.me/?p=267
id: 267
title: Packing List Web App
url: /2012/07/packing-list-web-app/
---
**Problem Description**  
When I go to the beach or Atlantic City or Florida or basically any trip, I noticed that I almost always need to pack the same things. I have been perfecting a checklist of things to pack over the past several years, and now I use it whenever I am packing to make sure I don’t forget anything useful. Right now it exists as a Google Tasks checklist. Before I start packing for a trip, I go through the checklist and make sure that everything I want to bring is unchecked, while everything I don’t need is checked (so I effectively ignore it). See the below screenshot for an example. Then, during the course of packing, I check the boxes next to each item I pack. When there are no unchecked boxes remaining, I know I’m good to go for the trip.

[<img class="alignnone size-medium wp-image-268" title="packing_list" src="http://robwilliams.me/wp-content/uploads/2012/07/packing_list-69x300.jpg" alt="Packing List - Google Tasks" width="69" height="300" />](http://robwilliams.me/wp-content/uploads/2012/07/packing_list.jpg)

The problem with this method is that on the way back from the trip I don’t know which items I packed and which ones I decided to not pack (because everything will be checked at the end, but some of those were never unchecked to begin with). Therefore, I am never sure if I remembered to re-pack everything on the way home.

**Designing a Solution**

I decided I wanted this to be more robust than just a copy of Google Tasks with the ability to mark things are ‘packed’ or ‘left at home’. The key feature I wanted is the ability to create a “List” for each category of items — e.g. Beach Stuff, Electronics, Toiletries, etc. — and then the ability to pull those lists’ contents into a “Trip” (I am quoting and capitalizing List and Trip because they will later be entities in my data model, so I want the meaning behind them to be clear). The idea is that if I am going on a Beach Vacation “Trip”, I’ll certainly want to include the contents of the Beach Stuff “List”, but wouldn’t want to include the contents of my Winter Clothing “List”. Similarly, for an Overnight “Trip”, I will want to include items from my Bedding “List”. This concept of breaking items into category-specific Lists will allow me to break my master packing list into several re-usable chunks that I can include or exclude at will for each type of Trip I go on.

As an aside, I original thought of making both Trips and Lists the same entity. This same entity could contain either items or links to another entity (which in turn would have items and links of its own). The problem with this approach is that you can introduce cycles by linking in a circle; a more important issue is that querying all the items in a list would require a recursive SQL query — something [not even possible in MySQL](http://stackoverflow.com/questions/3704130/recursive-mysql-query/3704175#3704175). I decided that allow a single level of linking worked for my use case, and this allowed me to keep Trips and Lists separate — Lists only contain items (like “toothbrush”, or “Kindle”) and Trips only contain links to Lists.

There was an extra level that I wanted. The whole purpose of this solution is to allow me to mark items as “Should Pack”, “Packed”, and “Repacked” (in contrast to my Google Tasks system which only allowed “Packed/Left at Home” with no distinction between them). This would be done each time I go on a trip. Did I want to have to reset the data for a given Trip each time I go on a new trip, or would I prefer to save a history of all my past trips? I decided that since I was building a whole app for this, I might as well add the feature of historical packing lists. This introduced an important requirement for a new entity type. The program would need to allow creating “Instances” of “Trips”. Let’s say today I go on a Beach Vacation “Trip” to Ocean City, NJ — this “Trip” may reference the “Beach Stuff” list and “Toiletries” list. I would want to create a copy of the Beach Vacation Trip and then within that copy is where I would work with the items and mark things as packed/repacked/etc. I could make an Instance work by referencing all the list items (referencing used here in a database foreign-key sense), but this would defeat the purpose of maintaining a history. As soon as someone modifies the item, deletes the item, or deletes the list containing it (deleting it through cascading), it would break the Instance. Therefore, I decided that an Instance should contain a copy of every List item referenced by the Trip that is being instanced. Think of it as a snapshot of a Trip, resolved to its underlying List items, at a given point in time.

This design led to the following DB schema (diagram made using [MySQL Workbench](http://www.mysql.com/products/workbench/ "MySQL Workbench")):

[<img class="alignnone size-medium wp-image-276" title="final_schema" src="http://robwilliams.me/wp-content/uploads/2012/07/final_schema-300x140.jpg" alt="" width="300" height="140" srcset="http://robwilliams.me/wp-content/uploads/2012/07/final_schema-300x140.jpg 300w, http://robwilliams.me/wp-content/uploads/2012/07/final_schema.jpg 876w" sizes="(max-width: 300px) 100vw, 300px" />](http://robwilliams.me/wp-content/uploads/2012/07/final_schema.jpg)

This design led to the following implementation requirements:

  * Mobile accessibility (e.g. iPhone)
  * CRUD for packing “Lists” and “List Items”
  * CRUD for packing “Trips” and “Trip Links”
  * CRUD for packing “Instances” — application-level logic will be needed to copy underlying List items into Instance
  * Interface for working with Instances to mark items as Should Pack, Packed, and Repacked

**Implementation**

I’ve noticed lately that I’ve fallen into what Maslow calls the “[nice CRUD generation utilities](http://www.grocerycrud.com/) for CodeIgniter, but I am glad that I did everything manually — it helped me learn CodeIgniter better.

There wasn’t much to the backend coding. It’s your usual CRUD coding work, most of it pretty tedious creation of forms and DB inserts. CodeIgniter made [ActiveRecord database class](http://codeigniter.com/user_guide/database/active_record.html) also made insertions dead easy; you can just write something like `$this->db->insert('list', $_POST)` and it “just works”, assuming the form that was POSTed contained all the non-nullable fields for the database table, with the “name” attribute of the `<input>` elements equal to the database columns — which it already is in almost any application.

The authentication work was done very simply. This is a single-user app, so a password was all that was needed. It’s just a password field which, if valid, stores a flag into the User Session. The main controller of the program redirects to the Login screen if that flag is not present in the session. Couldn’t be easier.

Most of the work was done on the frontend work for the Instance View page. This is where the user marks items as Should Pack, Packed, or Repacked. Since conceptually something cannot be packed unless it Should be Packed (i.e. “Should Pack”=true), and an item cannot be Repacked without having already been Packed (i.e. “Packed”=true), I represented this with a 3-column view. You have a column for Should Pack, containing all items in the instance. Then you have a column for Packed, containing all items marked under “Should Pack”. Finally, you have a column for Repacked that contains all items marked under “Packed”. Some jQuery fires AJAX calls to the backend controller to save the information in the DB, while also updating the interface with a nice effect. Something I found remarkable is that the only backend code needed to power the entire Instance View page’s AJAX stuff is this one line:

<pre lang="php">$this-&gt;db-&gt;update('instance_item', $_POST, array('item_id'=&gt;$_POST['item_id']));</pre>

This was my first CodeIgniter project, and I think it went quite well. The one glaring problem with my implementation is that I did not use Models at all; I put all the DB code directly into the Controller, which is obviously not the best practice, but I was in a rush. I will likely fix that, as well as turn this into a multi-user application (and potentially make it publicly available) in the near future, especially if I find it useful.

You can check out a video of the final product being used below (video captured with [CamStudio](http://camstudio.org/ "CamStudio Home Page")):



You can download the source code by clicking [here](http://robwilliams.me/weekly/pack.zip "Pack Source Code ZIP File").