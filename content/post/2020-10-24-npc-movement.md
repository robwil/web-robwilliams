---
title: "Game Dev: NPC Movement"
date: 2020-10-24T10:40:25-04:00
draft: false
comments: false
categories:
- Projects
- Game Development
---

I've continued working through [the book](http://howtomakeanrpg.com/a/how-to-make-an-rpg-release.html), and at this point my implementation is getting further and further away from the way the book does things.

The main focus this week was on NPC movement. This required quite a lot of refactoring of code that was originally written for the player only. I realize that with ECS architecture, the more you do things with actual ECS the better. A lot of the tutorials (e.g. the Bracket Roguelike) treat the player and the overall game state as special snowflakes. Although this is necessary at times, I realize now that it should be avoided when it can be helped. My movement system as originally written was heavily specific to the player use case, but instead should have been more generic from the start. All of that being said, I have found that with Rust and ECS, refactors are painless because it's usually pretty obvious where something needs to change and my systems are pretty well isolated from each other.

After refactoring the movement system to be more generic, I set up some basic NPC "AI" (it's so basic it can hardly be considered AI). This was implemented using a marker component called `Strolling`, and a new Plan Stroll system. The idea was to have an NPC wait for some random amount of seconds, then pick a random direction to walk, and then repeat. The real design question was how to model the state machine for the NPC: the Wait state and the Movement state. I already had the concept of a movement state in my global game state used by the player, which my factoring turned into an Entity Move State component. I wound up implementing both the player's Awaiting Input State and the NPC's Waiting State as components as well.

This whole question of how to handle state machines within ECS architecture has gotten some attention by other bloggers. The author of the Flecs ECS library in C++ even modified their ECS to support state machine transitions as a first class citizen. Their [blog post](https://ajmmertens.medium.com/why-storing-state-machines-in-ecs-is-a-bad-idea-742de7a18e59) about this goes into several of the issues with using the States-as-Components approach that I am doing, most of which are performance related. However, I was mostly convinced by this [Unity example](https://github.com/Unity-Technologies/EntityComponentSystemSamples/tree/master/ECSSamples/Assets/Use%20Case%20Samples/1.%20State%20Machine%20AI) of a similar approach -- they make the point that as long as your state transitions are relatively rare, the "expensive" cost of reshuffling your ECS data structures is amortized. Generally speaking, performance is unlikely to be a problem in my game since it's a 2D JRPG, which don't have many entities or complex interactions or simulations going on.

One last note is that I tried using Specs parallelism, but it turns out to break WASM support (due to the lack of multi-threading for the WASM target), so that's another performance-related thing that will have to be punted until it becomes a problem.

**Latest build**

* The latest demo can be played [here](http://staging.robwil.io/rpg-explore/npc_movement/): there is one stationary NPC and one "strolling" NPC.
* The latest code is available here: https://github.com/robwil/rpg-explore/tree/npc-movement

**![Basic scene with two NPCs](/images/game_dev/2020-10-24-basic-scene.png)**