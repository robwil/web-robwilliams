---
title: "Game Dev: WebSockets and WebAssembly"
date: 2021-01-10T10:40:25-04:00
draft: false
comments: false
categories:
- Projects
- Game Development
---

I haven't given an update for some time because I've been doing a bit of soul searching - that is, trying to find the soul of this project. I had been following the RPG book up until now, creating the framework for a 2D tile-based game. But as I read through the way the book was going to implement the combat, as a very simple JRPG turn-based system ('Attack', 'Spells', 'Items'), I quickly lost interest. With the limited amount of time I have to devote to this side project, the sheer amount of menus and GUI programming necessary within a Macroquad / GL context felt like a waste of time. Beyond this, I wanted to try a different style of combat, something involving explicit dice rolls similar to tabletop RPGs and board games like Arkham Horror or Roll for the Galaxy.

Long story short, I decided to abandon the `rpg-explore` repository and the 2D style graphics altogether, and instead shift to something that would allow quicker iteration of the actual combat system: using a web based interface. I considered whether to use a React/JS frontend to talk to my Rust backend, or to use Rust for the entire stack. I spent a brief moment experimenting with [Seed](https://seed-rs.org/) and was shocked by how productive it was. There was a some boilerplate necessary to package up the WASM file, but compared to the amount of boilerplate in a "create-react-app" template or similar, it was pretty small overhead. And thus I began to refactor my `dice-combat` macroquad/megaui game into a Seed frontend and Rust backend, using websocket as the communication protocol.

The game so far is not fully functional, but it allows drafting dice, rolling them, and then viewing possible actions. The selecting of actions and targets for those actions is partially implemented in the backend but not yet in the frontend. My immediate next steps will be to complete the end-to-end turn handling for human players, and then add a super basic AI agent that selects random valid actions/targets. At that point, I'll probably try to iterate on the actual gameplay -- which abilities are available, and how they are balanced.

I set up the deployment infrastructure similar to other side projects:
* Static (client-side) files are deployed to GCS, and served directly from there
* Server-side backend is built as a Docker container and deployed using Cloud Run. Cloud Run is my go-to for side projects since it's so cheap ($0 for idle time). It's obviously not a great fit for WebSocket servers due to the ephemeral nature of containers, but it works okay for a game session as long as people are taking turns within reasonable time frames.

**Latest build**

* The latest demo can be seen here: http://staging.robwil.io/dice-combat/initial/ (note that after drafting dice, it will just stay at "Waiting for server...")
* The latest code is available here: https://github.com/robwil/dice-combat

**![Basic dice drafting](/images/game_dev/2021-01-10-dice_combat.png)**