---
title: "Migrated to Hugo"
date: 2020-01-04T17:32:38-05:00
categories:
- Projects
- Self-Referential
---
As part of a larger personal project to get off all unmanaged VPC nodes (moving to version-controlled infrastructure and/or No-Ops like Google Cloud Run where possible), I decided to migrate my blog off Wordpress and onto a static site generator [Hugo](https://gohugo.io/). I liked the simple [Bare theme by @org](https://github.com/orf/bare-hugo-theme) so I went with that.

The actual migration involved using the "Wordpress to Jekyll" exporter plugin and then using Hugo's built in import functionality for Jekyll-type Markdown. The only major issues I dealt with were with Unicode-encoded characters like — (aka em-dash) coming in like `&#8212`, with `<a href>` tags with titles not getting converted to Markdown links, and with images being a total mess due to my use of various Wordpress plugins. I fixed all of those more or less manually with VS Code and regex find/replace. Having Git version control of the content definitely helped there for when my regex was off.

The only remaining services running on my Digital Ocean VPC is a mail relay server for my custom domains. I'll look to migrating that to some Cloud based email forwarding in the near future.