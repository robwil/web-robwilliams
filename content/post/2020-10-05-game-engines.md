---
title: "Game Dev: Game Engine Decisions"
date: 2020-10-05T10:40:25-04:00
draft: false
comments: false
categories:
- Projects
- Game Development
---

After spending way too much time exploring tabletop RPGs (like [Dungeon World](https://dungeon-world.com/), [Blades in the Dark](https://www.evilhat.com/home/blades-in-the-dark/), and [FATE Core](https://fate-srd.com/fate-core/basics)) for inspiration regarding combat mechanics, story, and setting, I finally decided to just start building a basic JRPG style game and then circle back to mechanics later. Following game development advice from many seasoned veterans, it’s really important to get something simple working before getting bogged down in complex mechanics or innovative ideation.

The book I finally settled on as my primary learning mechanism is [How to make an RPG](http://howtomakeanrpg.com/a/how-to-make-an-rpg-release.html). After reading a bit of the free articles on the author’s website, it felt like a good fit for what I was looking for. The game being built is relatively simple in scope, while still covering a lot of ground and setting me up to branch into more complex games in the future. However, all of the code in this book is written in LuaJIT, which gets included into the [Dinodeck C++ engine](http://dinodeck.com/) written by the author strictly for this book.

Since one of my [goals for this project]({{< ref "/post/2020-09-28-game-dev-intro" >}}) is to use Rust, this was problematic. I would have to handle the code side of things on my own, only using the book's code as a reference. Since the book's code and engine are both open-source, there luckily won't be any implementation details hidden (e.g. as might be the case if I was following a Unity or Unreal engine tutorial). That said, I also really don't want to spend all my time porting the Dinodeck engine to Rust. My hope is that an existing Rust engine would be able to cover most of the basics for me, and I could build on top of that any missing pieces.

I have been subscribed to and reading the [Rust Game Dev newsletter](https://rust-gamedev.github.io/posts/newsletter-014/), and the [Amethyst engine](https://amethyst.rs/) has popped up a few times. Since it is built on top of the specs ECS library that I already used for `rust-sobokan` and the Bracket Roguelike tutorials, it felt like a good option. As I read more about it, it seemed like a perfect fit for working on an RPG in the style of Dan Schuller’s book, since Amethyst offers game state machines, tweens, and other similar terminology and concepts.

I decided to follow [the Amethyst book](https://book.amethyst.rs/stable/), and wound up [starting to build out Pong](https://github.com/robwil/amethyst-pong). I got the paddles on the screen and the initial movement felt really smooth. The problems hit once I started to get the ball moving on the screen. Everything got really slow, and that’s when I came upon this thread that names poor Metal performance in Mac: https://github.com/amethyst/amethyst/issues/1699. This was really disappointing and made me second guess the use of the Amethyst engine. My overall goal also includes deploying to web, and it turns out the [Amethyst wasm support](https://community.amethyst.rs/t/wasm-effort/1336) is very experimental and under active development. This led me to start looking into other engines. Even if I don’t wind up using Amethyst, the engine has some really nice design decisions (e.g. the bundling of related ECS systems) that I intend to copy for my own game if it gets sufficiently complex.

Here are other options I am exploring:

- [Macroquad](https://github.com/not-fl3/macroquad) built on top of [Miniquad](https://github.com/not-fl3/miniquad) which is used by the [Zemeroth game](https://github.com/ozkriff/zemeroth) which has beautiful WASM results
    - Bonus points is there’s proof of concept for integrating with Map Tile Editor, which is used by the Schuller book: https://gist.github.com/not-fl3/d0b5999cb95c7fac63e973da7a32a2f5
- The [Bevy Engine](https://bevyengine.org/) has been making waves recently, especially for it's ECS library
    - However, WASM support is still not quite there.
- Godot engine, which has pretty good support for Rust bindings
    - https://godotengine.org/
    - https://medium.com/@recallsingularity/gorgeous-godot-games-in-rust-1867c56045e6
    - https://blog.usejournal.com/how-i-use-rust-and-godot-to-explore-space-806bb810e950
    - https://paytonrules.com/post/games-in-rust-with-godot-part-one/
    - Godot, in addition to tons of other resources, does support the Map Tile Editor: https://godotengine.org/asset-library/asset/158
    - The big downside I see here is that Godot is huge and massive thing to learn, and it’s not really clear what benefit writing Rust plugins for it will have over just GDScript.
    - Additionally, Godot does not necessarily support or work with ECS architecture, see: https://www.reddit.com/r/godot/comments/9geg7r/are_there_any_examples_of_using_ecs_entity/. While I’m not particularly wedded to ECS, I do want to try it out.
    - If I do want to go down this route, this looks like a solid place to start: https://www.heartgamedev.com/1-bit-godot-course-fall-sale

So far, I spent a bit of time on macroquad. I like the simplicity of the library and the fact that it makes compiling to WASM a breeze. I used macroquad to go through the first 10 or so steps of the RPG book before hitting what felt like a brick wall. That’s when the book transitions to use Tiled Map Editor. At first I couldn’t figure out how to get this working with macroquad, even with the gist mentioned above, but I eventually got it. The key trick was exporting to JSON with uncompressed CSV layer data, which it seems is what the macroquad-tiled library expects. Also non-obvious was how to import the macroquad-tiled subcrate, which has some [unexpected syntax](https://github.com/rust-lang/cargo/issues/1466#issuecomment-348460973).

But finally, I do have a basic Tiled map rendering, and even implementing camera panning with arrow keys. You can see a working demo [deployed here](http://staging.robwil.io/rpg-explore/camera-pan/).