---
title: "Choosing a static site generator"
date: 2020-04-24T22:45:49+01:00
draft: false
categories:
- development
tags:
- hugo
- "static site"
- SSG
- JAMstack
featured_image: "/v1588111829/gotf-fireflies_zg6mhl.png"
---

## What is a Static Site Generator?

Static Site Generators have exploded in popularity in the last few years but the concept is not a new one. The very first websites were simple static HTML files after all. The difference with modern static site generators is they allow us to leverage powerful technologies to have dynamic functionality with the speed and security of static sites.

The Smashing Magazine article [Modern Static Website Generators are the Next Big Thing (2015)](https://www.smashingmagazine.com/2015/11/modern-static-website-generators-next-big-thing/) outlines three key features common to all Static Site Generators:

### Templating

Breaking a site down into reusable layouts through templates. This is similar to the kind of php templating Wordpress has provided for years, now done on the frontend. There is a focus in general on modular, reusable components in front end development, and templates are a logical choice.

### Markdown

Rather than writing content in HTML, markup languages like markdown allow us to store it as plain text with human-readable shorthands for formatting. It's ultimately the latest evolution of the separation of content and design.

### Metadata

This is the additional information relative to our content. The data about data. In Wordpress terms, things like the author and date of a post are meta data. But we don't just create blog posts any more. Many UX and Developer sites now include projects and case studies, for example. Things like client, role, duration, and technology can all be relevant metadata.

This is what Front Matter is for. It allows us to outline meta data inside our documents and templates. It's typically written in [YAML](https://yaml.org/), making it a nice human-readable format.

## Finding my way to Hugo (eventually)

Hugo wasn't actually my first choice but it's a good one! I actually found my way to Hugo in a rather round about way:

I'd heard a lot of really good things about Eleventy, another Static Site Generator, but at the time I wasn't really aware of what SSGs were. Then I discovered Andy Bells' [Hylia starter kit](https://hylia.website/) for Eleventy and so began my journey.

In a couple of clicks, I was able to create a blog using Hylia with NetlifyCMS for content creation (think of the backend admin area in Wordpress). What I really like about Hylia is it comes with a [styleguide](https://hylia.website/styleguide/) and design tokens that can be adjusted in the CMS to tweak these options.

It was great, but I didn't really like the NetlifyCMS it was bundled with. It just felt a bit basic and I had minor irritations with the interface. Hex codes had to be entered manually in the design tokens. Not so bad if you already know your colour scheme, but I like the freedom of being able to explore a colour wheel. It also has similar post status and publish options as Wordpress, but I found the layout made it less than obvious how these worked (I kept pressing Publish, not realising I had to set the status too)

They are *very* minor grievances and, to be honest, I didn't really give it much time at all so wouldn't be too hasty to rule it out in future.

Beginning to understand though that the SSG and static CMS could be loosely coupled, I started looking for an alternative. It was then that I discovered Forestry and, even better, a port of Hylia for it.

Again, in a couple of clicks I had an Eleventy powered site with Forestry as the static CMS. I found the Forestry interface much easier to understand and started exploring.

Then I got confused again!

I couldn't work out how to create a static page like an About page, and how to include this in the main navigation. I found the Eleventy docs a little impenetrable since there seemed to be an assumption that I was already familiar with all the Static Site terminology (I wasn't). While I figured out how to create the content in Forestry, I couldn't figure out how to wire this up in the right way to be built by Eleventy.

It felt like, in jumping straight into Hylia, i'd skipped over the basics of working with Eleventy. I didn't have the right grasp of how everything was working together.

In hindsight, I realise also that my mental model from Wordpress was tripping me up when it came to the distinction between posts and pages. As I now understand it, there isn't one. There are only collections of content and these can have any type you determine. I think this is covered in the Eleventy documentation, but I didn't actually understand this until I discovered Hugo.

## So why Hugo?

It was around this time that I realised that, while Eleventy works with Forestry, other Static Site Generators like Hugo and Jekyll are more fully supported (menus in Forestry are only supported in Hugo and Jekyll, for example). I decided that, until i'm more familiar with all the moving parts, sticking with a tried and tested approach was best.

I also found the Hugo documentation much more beginner-friendly. It walks you through from start to finish, building up the knowledge required, whereas I felt a bit lost with Eleventy. I don't think this is necessarily a discredit to Eleventy, but something just 'clicked' easier with the way concepts were described in Hugo.

Admittedly, part of the attraction of Eleventy for me was that templates can be written in (amongst many options) JavaScript, whereas in Hugo they are written in Go. I wanted to avoid having to learn another language if I could help it, but I think it's a minor trade off since I don't need to create that many templates. Either way, my content itself will be written in Markdown which i'm already familiar with.

Although not a concern for my small site, Hugo is also proven to be blazingly fast even with thousands of blog posts. It's nice to know that it operates well at scale.

In the next post i'll explain how I got up and running with Hugo.