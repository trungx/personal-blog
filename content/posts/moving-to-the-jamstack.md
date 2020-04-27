---
title: "Moving to the Jamstack"
date: 2020-04-24T17:23:23+01:00
draft: false
---

I mentioned last post that i'm taking this renewed interest in blogging as an opportunity to migrate to a different platform and learn some new skills. I've lost no love for Wordpress but, in aiming to have a clearer focus on personal projects, and with my growing interest in learning modern web technologies, I feel this blog is a good place to start.

Rather than make the change behind the scenes though, I want to show my learning live. Linked from this post, I will add later posts discussing the move: the technologies involved, choices made and everything I understand or don't. The aim is to build a habit of reflecting on my projects and to create a tangible record of my progress.

## Enter the JAMstack

> "A modern web development architecture based on client-side JavaScript, reusable APIs, and prebuilt Markup"
>
> — Mathias Biilmann (CEO & Co-founder of Netlify).

The phrase "JAMstack" is one i've seen crop up ever more frequently this past year. It's not a new thing as it was actually coined around 2015, but it's certainly increased in popularity within the last couple of years.

It's the term for a development philosophy and a collection of technologies that move away from the traditional architecture of database driven websites and server hosting.

The idea is that rather than pages being pulled together and presented from a server or database every time someone visits the site, they are instead "static", in other words prebuilt, and distributed across Content Delivery Networks (CDNs) ready to be served.

The JAM in "JAMstack" comes from the JavaScript, APIs and Markup technologies that make up this approach:

* **JavaScript** provides dynamic functionality entirely on the client side
* **APIs** are used to abstract away any server-side functionality and provide specialised services
* **Markup** is prebuilt when the site is built, typically using templates, rather than being built by the server when the site is visited.

The suggested benefits are increased security and speed together with lower costs and less complexity. The loosely coupled architecture offers more flexibility and adaptability since service providers can (in theory) be swapped out relatively easily compared to a tightly integrated building and hosting solution.

It's an attractive idea, not least because it does away with the need for traditional hosting and learning server side technologies. There's also something about the simplicity of writing in a plain text format like Markdown that I find very appealing.

## What does that mean for the blog?

As I find my feet with this different approach, these are the technologies i'm currently using to make this blog possible:

* GitHub - Version control to host the source
* Hugo - A Static Site Generator to build the site
* Forestry - A Static CMS to make editing content easier
* Netlify - A Cloud computing service for automated deployment

My choice of products and their implementation are discussed in the following posts.

[Choosing a Static Site Generator]
[Creating a Static Site with Hugo]
[GitHub and Netlify]
[Exporting content from Wordpress]
[Implementing comments]

## Useful Links

https://jamstack.wtf/
https://jamstack.org/
https://snipcart.com/blog/jamstack
