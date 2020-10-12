---
title: "Game Dev: Player Movement, Collisions, and Triggers"
date: 2020-10-12T10:40:25-04:00
draft: false
comments: false
categories:
- Projects
- Game Development
---

I've continued working through [the book](http://howtomakeanrpg.com/a/how-to-make-an-rpg-release.html), and the [Macroquad](https://github.com/not-fl3/macroquad) engine has continued working well for me.

As mentioned before, the book's Lua code is not always easy to translate to Rust. This gap has gotten even larger as I am departing from much of the book's code organization. I'm instead using ECS architecture.

## Player movement animation

The first big hurdle came during player movement. Actually moving the player around the grid was easy enough -- just some basic math which I encapsulated in the rendering system (I suspect this will get more complicated when I start to have multiple levels/maps). But the book introduced the concept of a "walk cycle", which includes 16 different sprites for the player -- 4 for each direction (left, up, right, down). Each time you press an arrow key to move, the game figures out the direction, changes the player sprite to the appropriate one, and then cycles through the 4 frames of the walking animation all while tweening the player from one tile to the next. The book does this using a combination of a State Machine and a Tween class. I will eventually probably need a more robust State Machine (and the Macroquad creator has an interesting implementation of one [here](https://gist.github.com/not-fl3/7ba109eb72f9ad28fa33258a233f9bb3)), but for now I was able to just encode the state as an enum within the ECS world's global resources. This is the technique used by the [Bracket Roguelike](https://github.com/robwil/bracket-roguelike) tutorial.

The actual animation and tweening I implemented in a new Player Movement system which combines the tweening and the animation frame selection, all while avoiding additional global state by calculating how far along in the animation I are using the existing `delta_x` / `delta_y`. Those variables start at `1` or `-1` when the player moves, and each frame I can check the delta time to figure out how much to move, and then update `delta_x` / `delta_y` accordingly, to track that the player does't need to go the full 1 tile anymore. For example, if the game is running at 60 frames per second, each frame will have a delta time of 1 / 60 = 0.01667, and so we'll decrement the appropriate `delta_*` variable by that much after actually moving the player that much.

## Collision detection

The second item was collision detection which turned out to be really easy. I just using an additional tile layer in the Tiled map editor to "draw" collision information into the map. In the future, I'll probably need to account for collisions with non-map entities, but doing so will be as easy as an ECS join with any entities that have a `BlocksMovement` component or similar.

**![Collision mapping in Tiled map editor](/images/game_dev/2020-10-12-collision-map.png)**

## Triggers and event system

The third item, and this was the most interesting problem to architect a solution for, is implementing a trigger system. I wrote up the resulting architecture in the game's readme, so I'll just copy that here.

A "trigger" in an RPG like this is some action that gets triggered based on the player entering, exiting, or "using" a particular map tile. There were two tricky aspects to handling this in an ECS system.

First of all, how do we keep track of the player entering, exiting, or using particular tiles? The most natural place for this is the Player Movement System, which is where we handle the animation and actual movement on screen of the player going from one tile to the next. On any valid movement, this is where we know a player is exiting or entering a tile. For the "using" case, that can be handled in the Input System by simply checking for the use key. The actual tracking of this can be handled by emitting an Event, which will be stored as a global Resource in the ECS world.

How to actually represent event state within the app? For simplicity, I am just using a global `EventQueue` containing two vecs: current events, and new events. At the end of each frame, the current events are cleared and replaced by the new events. This allows multiple systems to read the current events in a decoupled fashion, while also allowing multiple systems to emit new events. The decoupled nature could have downsides in the future if things get too complex because the relationships between producers and subscribers is not explicit, but it seems like the best way to handle things. This was inspired by the one implemented in the [rust-sokoban](https://github.com/robwil/rust-sokoban) tutorial.

The second issue to consider is how to represent the actual trigger points, e.g. at map position (5, 2) there is a door that should bring the player to the next map. It makes sense to represent these as Entities, using my existing `GridPosition` component. We will have some triggers that take place on map elements that come from the Tiled map, and therefore have no `SpriteDrawable` component. Other triggers might be drawn separately from the map, e.g. appearing after another trigger happened. That's fine, and is exactly what ECS empowers us to do. The actual trigger dimension can be captured with components like `TriggerActionOnEnter { action: Action }` or `TriggerActionOnUse { actions: Action }`. The systems responsible for such events would be 1) iterating through all the events from the appropriate event queue, 2) joining GridPosition with the appropriate trigger component, and iterating all those components, 3) if any of the incoming events' positions match the positions of the triggers, we execute the action. For now, I am doing all of that in a single system `ActionSystem`, but in the future this could be split out to many systems based on which events and actions they deal with. We could even completely decouple the event handling from the action execution if desired, by creating an `ActionQueue`.

One important note is that both `Event`s and `Action`s are modeled as enums. I didn't want `Action`s to have arbitrary code/lambdas attached to them because that would break the ECS paradigm. By keeping Actions as strictly data, we keep the logic in the Systems.

**Latest build**

* A demo of this functionality can be played here: https://github.com/robwil/rpg-explore/tree/collision-trigger
* The corresponding code is available here: https://github.com/robwil/rpg-explore/tree/collision-trigger