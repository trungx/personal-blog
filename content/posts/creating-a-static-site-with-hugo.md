---
title: "Creating a Static Site with Hugo"
date: 2020-04-25T12:41:31+01:00
draft: true
---

Last post I explained how I settled on Hugo as the Static Site Generator for my blog as part of my move to the "JAMstack". This post is how I got things up and running.

I had a working site after installing Hugo and adding a theme. The rest is how I customised it to my needs:

[Installing Hugo and Creating a new Project](#installing-hugo-and-creating-a-new-project)  
[Adding a Theme](#adding-a-theme)  
[Overriding Theme Styles](#overriding-theme-styles)  
[Creating Content](#creating-content)  
[Adding Different Content Types](#adding-different-content-types)  
[Creating Archetypes](#creating-archetypes)  
[Creating Templates](#creating-templates)  
[Updating the Navigation Menu](#updating-the-navigation-menu)  
[Viewing the site Locally](#viewing-the-site-locally)  
[Deploying the site to Netlify](#deploying-the-site-to-netlify)

## Installing Hugo and Creating a new Project

This was super simple!

I followed the [Quick Start article](https://gohugo.io/getting-started/quick-start/), installing the Hugo binary for macOS using homebrew from my Terminal:

```
brew install hugo
```

Then running the following where 'personal-blog' is the name of the folder that was created:

```
hugo new site personal-blog
```

The resulting files and folders were as follows:

* archetypes
* config.toml
* content
* data
* layouts
* resources
* static
* themes

I found a really good explanation of the key files and folders in [Hugo's Directory Structure Explained](https://www.jakewiesler.com/blog/hugo-directory-structure/) by Jake Wiesler, but essentially:

* Site settings are defined in config.toml
* Content lives in the content folder
* Templates are defined in layouts
* Static assets like CSS, JS, fonts and images live in the static folder
* Default parameters for content like 'title', 'date' etc are defined in Archetypes
* Themes live in the themes folder

[Back to Top](#top)

## Adding a theme

Hugo doesn't come with a theme so next the guide advises installing one. This was as simple as cloning a theme from GitHub (I used Ananke as suggested) into a repositiory in the themes folder using the following commands:

```
cd personal-blog
git init
git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
```

Here I am:

* Changing into the personal-blog folder that was created
* Creating a new git repository (in this case, making the existing project a git repository)
* Cloning the Ananke theme repository into a git repository *within* my main repository, allowing me to later pull any updates to the theme (see [git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules))

I then added a line to the config.toml file to point to this theme. This can be manually edited with a text editor or you can enter the following in the Terminal:

```
echo 'theme = "ananke"' >> config.toml
```

The config.toml file now includes a 'theme' parameter:

```toml
title = "My new Hugo website"
baseURL = "https://example.com"
languageCode = "en-us"
theme = "ananke"
```

> ## Just a heads up
> It's worth pointing out that if you follow the next set of Hugo quickstart instructions to create your first post and start a local web server to view your site, it won't look as expected. For one thing the header image is missing.
>
> Although the guide doesn't mention it, it's necessary to follow some of the instructions from the [Ananke Theme page](https://github.com/budparr/gohugo-theme-ananke) to configure the theme. You need to set things like the path where the header image is found, for example.
>
> An example is included in the Ananke exampleSite folder. You can simply copy some or all of the settings from the exampleSite config.toml file into your own site's config.toml file.
>
> One thing to watch out for though is that theme name is incorrectly shown as "gohugo-theme-ananke" in the example site config.toml. It needs to match the name of the theme folder which is actually "ananke", so if you copy the exampleSite config.toml, you need to change this line.
>
> You can also remove `themesDir = "../.."` line since this was used by the exampleSite to point to the theme folder, two levels above where the exampleSite config.toml is located, which is not the case for our site.

[Back to Top](#top)

### Overriding Theme Styles

Including a CSS file in the static folder and providing a link to the stylesheet in the header layout file allows the CSS defined in the theme to be overwritten. However, themes like Ananke make this easier by providing a `custom_css` parameter in config.toml that you can instead set to your custom CSS file.

For example, I created a file called custom.css in a static/css subfolder:

```
cd static
mkdir css
touch custom.css
```
and linked to this in config.toml

```toml
[params]
custom_css = ["css/custom.css"]
```

I haven't yet made many changes as i'm still figuring out the CSS of Ananke but they will go in this file.

[Back to Top](#top)

## Creating Content

New content (blog posts, pages etc) can be created from the Terminal using the following:

```
hugo new posts/my-first-post.md
```

This asks hugo to create a Markdown file called 'my-first-post' in a posts subfolder (it will be created if it doesn't already exist). These files are created in the content folder by default.

You can alternatively, create a markdown file in any text editor and manually place it in a 'posts' subfolder in the Hugo content folder. The benefit of using the terminal is Hugo includes a timestamped date in the "Front Matter" (more on that below). The file created by Hugo has this content, for example:

```YAML
---
title: "My First Post"
date: 2019-03-26T08:47:11+01:00
draft: true
---
```

The content between the triple dashes --- at the top of the file is the [YAML](https://yaml.org/) Front Matter. This is where you set variables like the post title and date. Hugo comes with some that are predefined like title, type and layout, but you can also create your own.

Below the second set of triple dashes is where you write your Markdown, the markup language used to format your documents. The syntax is very simple to learn - see [Markdown Guide](https://www.markdownguide.org/) for a reference. I already use it at work with our wiki platform Confluence and our issue tracker YouTrack. It's such a pleasure to work with. If you've ever used the delightful notetaking application Bear on macOS/iOS, you've already used Markdown.

The Ananke theme uses some of the Front Matter variables when displaying posts such as the title and date.

## Adding Different Content Types

As I mentioned, it wasn't until I started exploring Hugo that I realised that there was no separation between pages and posts. As far as the Static Site Generator is concerned, they are both just pieces of content.

Hugo is clever enough to determine the content's type from the type set in the Front Matter of the Markdown file or, if not specified, from the first directory in the file's path. My blog posts for example, automatically inherit a type of 'posts' from the containing directory ('posts') since I didn't add a type to the Front Matter.

However, my About page is at the top of the content folder so there isn't an appropriate directory to inherit from. I can instead specify the type in the Front Matter of the content file. This takes precedence over the directory so the page could be anywhere in the content folder and the type would still apply. In her article [Migrating from Jekyll+Github Pages to Hugo+Netlify](https://www.sarasoueidan.com/blog/jekyll-ghpages-to-hugo-netlify/), for example, Sara Soueidan used 'static' as the type for fixed pages like the About page.

That said, in Hugo, content is type 'page' by default so i've left this alone for now. Instead, I specified a review type for my Project Ghibli articles:

```YAML
---
title: "Gauche the Cellist"
date: 2020-04-25T22:05:54+01:00
draft: true
type: review
---
```

The keen eyed may spot the use of +++ rather than --- in Sara's article:

```TOML
+++
type = "blog"
description = "..."
title = "..."
date = ...
+++
```

The difference is simply that +++ is TOML format while --- is YAML format. Hugo supports both (and also JSON) and I went with YAML since this is what posts used by default. The configuration file uses TOML though (config.toml) so I may end up making this a yaml file later too because I prefer the colon format over equal signs.

### A word on Type vs Section

I thought I had this straight early on but since the type can be inferred from the Section, I found myself confused.

[From Wordpress to Hugo - A Mindset Transition](https://regisphilibert.com/blog/2019/01/from-wordpress-to-hugo-a-mindset-transition/) by Régis Philibert does a great job of clearing some things up:

#### Type
> In a framework like WordPress, every entry is a post of different types. A post is a post of type post, a page is a post of type page and a recipe is a post of custom post type recipe (or whatever you chose to name it).
>
> In Hugo, every entry or content file is a regular page of a different type. And because there is no built-in type, every type is your own custom type.
>
> -- Régis Philibert

#### Section

A Section is defined from the organisation of content and not set by the user. By default a post will have the same section and type:

> "By default, everything created within a section will use the content type that matches the root section name. For example, Hugo will assume that `posts/post-1.md` has a `posts` content type."
>
> -- [Official Hugo Docuemntation](https://gohugo.io/content-management/sections/)

The type can be overwritten by the Front Matter. This may be on a content by content basis or automatically applied using Archetypes (more on that next)

[Back to Top](#top)

## Creating Archetypes

While it's fine to add parameters to the Front Matter of files during editing, Archetypes allow you to save time by predefining the parameters to add to certain types of files when they are created using `hugo new`. These parameters can then be used in the content templates (layouts).

For example, I made a ghibli-review Archetype (ghibli-review.md) with parameters that would be added to my Project Ghibli posts:

```YAML
---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: true
type: review

# Project Ghibli metadata
featured_image: # e.g. images/featured.png
release: # e.g. 23rd January 1982
directors: # e.g. ["Isao Takahata"]
producers: # e.g. ["Kôichi Murata"]
screenwriters: # e.g. ["Isao Takahata"]
alt-titles: # e.g. ["Gōshu the Cellist"]
---
```

As far as I understand it, including the content type is unnecessary since this will already be based on the Archetype filename, however I included it as I wanted to override this.

Now, any content files created using the ```hugo new ghibli-review``` command such as:

```
hugo new ghibli-review/gauche-the-cellist.md`
```

will be created in the content/ghibli-review directory (it will be created if it does not exist) and will have these parameters:

```YAML
---
title: "Gauche the Cellist"
date: 2020-04-25T22:05:54+01:00
draft: true
type: review

# Project Ghibli metadata
featured_image: # e.g. images/featured.png
release: # e.g. 23rd January 1982
directors: # e.g. ["Isao Takahata", "Hayao Miyazaki"]
producers: # e.g. ["Kôichi Murata"]
screenwriters: # e.g. ["Isao Takahata"]
alt-titles: # e.g. ["Gōshu the Cellist"]
---
```

The `hugo new` command uses the content section to find the most suitable archetype template in your project. If the project does not contain any Archetype files, it will also look in the theme.

While it's a little timesaver having these parameters automatically added to the top of my Markdown files, i'm still manually editing their values at the moment. However, I believe I can create input fields in the Forestry UI and pull these through to the Front Matter to do this.

[Back to Top](#top)

## Creating Templates

In Hugo you can define templates for the way that types of content will be displayed. In Hugo, every piece of content is a Page and there are three key types: [home](https://gohugo.io/templates/homepage/) for the homepage, [single](https://gohugo.io/templates/single-page-templates/) for single pages like a blog post or the about page, and [list](https://gohugo.io/templates/lists/) for list pages like category and tag pages. See [Content Organisation](https://www.mikedane.com/static-site-generators/hugo/content-organization/) by Mike Dane.

Templates are defined in the layouts folder. To create the template for my Project Ghibli posts, I copied the `single.html` file from the Ananke theme into a review folder in layouts. This folder was named review to match the type defined in the ghibli-review and film-review Archetypes.

I then adjusted this to display my custom parameters for directors etc. The Go syntax was fairly easy to understand: Front Matter parameters can be accessed via the `.Params` variable, while `.Directors` is the custom parameter I created. I made these fields arrays so that I can provide multiple names and use `range` to iterate over their values. Here i'm looping through the Directors array, with the `.` referring to the current context (in this case, the current item in the array):

```HTML
<ul>
  {{ range .Params.Directors }}
  <li>{{ . }}</li>
  {{ end }}
</ul>
```

Here is the above list in the context of a `single.html` template:

```HTML
{{ define "header" }}{{ partial "page-header.html" . }}{{ end }}
{{ define "main" }}
  <div class="flex-l mt2 mw8 center">
    <article class="center cf pv5 ph3 ph4-ns mw7">
      <header>
        <p class="f6 b helvetica tracked">
          {{ humanize .Section | upper }}
        </p>
        <h1 class="f1">
          {{ .Title }}
        </h1>
      </header>
      <div class="nested-copy-line-height lh-copy f4 nested-links nested-img mid-gray">
        <ul>
          {{ range .Params.Directors }}
          <li>{{ . }}</li>
          {{ end }}
        </ul>
        {{ .Content }}
      </div>
    </article>
  </div>
{{ end }}
```
At the start of the file, the header content is pulled in from another file stored in layouts/partials. Using this 'partial' allows you to build templates up from several parts.

The line ```{{ humanize .Section | upper }}``` cleans up the value of .Section - the name of the content folder - removing hyphens and spaces and turning it to uppercase. It's automatically pluralised by Hugo so, for example, becomes Reviews in the built site.

I plan to have a few review types (ghibli, films, games etc) so created a few Archetypes. I could have made seperate Layouts too, however I figured out a way to conditionally render different parameters meaning I could just use a single Layout for all my reviews. I may find a better approach in future.

Since I can override Type in the Front Matter, I made two Archetypes `ghibli-review` and `film-review` that create content files with Type `review` and then use a single review Layout for both.

Since all i'm providing in the template is a list, I can pull in and display only the needed values from the Front Matter.

From [Hugo, the scope, the content and the dot](https://regisphilibert.com/blog/2018/02/hugo-the-scope-the-context-and-the-dot/), I figured out that I could switch context and render out each parameter in turn. The use of `with` means that a parameter will only be used if it has content. In this way, I can simply leave these fields blank and they won't be rendered:

```HTML
<ul>
{{ with .Params }}
    {{ with .Release }}
      <li>Original Release: {{ . }}</li>
    {{ end }}
    {{ with .Directors }}
      <li>Director(s): {{ delimit . ", "}}</li>
    {{ end }}
    {{ with .Genre }}
      <li>Genre(s): {{ delimit . ", " }}</li>
    {{ end }}
    {{ with .Producers }}
      <li>Producer(s): {{ delimit . ", "}}</li>
    {{ end }}
    {{ with .Screenwriters }}
      <li>Screenwriter(s): {{ delimit . ", "}}</li>
    {{ end }}
    {{ with .Alternative_Titles }}
      <li>Alternative Title(s): {{ delimit . ", "}}</li>
    {{ end }}
{{ end }}
</ul>
```

[Back to Top](#top)

### Multiple sections on the homepage

After creating my Project Ghibli posts, I was slightly surprised to find that my homepage only showed my 'ghibli-review' Section content rather than a mix of posts from all Sections I had created.

It turns out that in themes it's common and recommended to use the following:

```HTML
{{ $section := where .Site.RegularPages "Section" "in" $mainSections }}
```

This stores a list of all the user created pages (.Site.RegularPages), however it defaults to the Section with the most pages. Since I had created more Project Ghibli posts than regular posts at this point, that is what was being returned.

Since Ananke uses this approach, I needed to [override $mainSections](https://gohugo.io/functions/where/#mainsections) with the Sections I wanted to include. So in my config.toml file I added this:

 ```TOML
 [params]
 mainSections = ["posts", "ghibli-review"]
 ```

[Back to Top](#top)

## Creating the Navigation Menu

Hugo comes with a built in [menu system](https://gohugo.io/templates/menu-templates/#site-config-menus). The easiest way to create a simple navigation is to add ```SectionPagesMenu = "main"``` to config.yaml. This will set the menu named "main" to include all Sections.  These can then be looped through in a template using `{{ range .Site.Menus.main }}` for display. The Ananke theme already does it like this, for example:

```HTML
{{ if .Site.Menus.main }}
  <ul class="pl0 mr3">
    {{ range .Site.Menus.main }}
    <li class="list f5 f4-ns fw4 dib pr3">
      <a class="hover-white no-underline white-90" href="{{ .URL }}" title="{{ .Name }} page">
        {{ .Name }}
      </a>
    </li>
    {{ end }}
  </ul>
{{ end }}
```

Adding `SectionPagesMenu = "main"` to config.toml is all you need to do to use this with Ananke.

I didn't necessarily want all sections in my navigation though and I wanted to be able to rename some of them. Instead of using `SectionPagesMenu`, individual menu items can be defined in config.toml or in the content's front matter.

```TOML
[menu]
[[menu.main]]
  identifier = "posts"
  name = "Blog"
  title = "All Posts"
  url = "/posts/"
  weight = 1

[[menu.main]]
  identifier = "review"
  name = "Project Ghibli"
  title = "Project Ghibli"
  url = "/review/"
  weight = 2
```

The documentation doesn't explicitly say it, but in TOML format you define `[[menu.main]]` items for each of the menu items.

The identifier and url must match the name and location of the Section. The name is the name displayed, though i'm not sure yet what purpose title serves. The weight controls the order, with lower numbers coming first (they can be negatives too).

My About page is not defined in config.toml since I had already included this in the menu by adding `menu: "main"` to its Front Matter. I didn't include a weight so this automatically comes after the other two menu items:

```YAML
---
title: "About"
description: "About Daniel Mclaughlan — Accessibility and Usability Consultant"
menu: "main"
---
```

[Back to Top](#top)

## Viewing the site locally

The last thing to do to see my site in action, locally at least, is to start a local webserver. Hugo makes this simple:

```
hugo server -D
```

The -D flag tells Hugo to include content marked as a draft. By default, Hugo will not build drafts, but we can override that behaviour during development with this flag. Here is a [list of hugo server options](https://gohugo.io/commands/hugo_server/#options).

Once the process is complete, you will get a summary of what was created and the URL of where to view the site at http://localhost:1313/:



The wonderful thing about this is now any changes made to your files will cause Hugo to instantly rebuild the site so you will see changes almost immediately. This is the Static Site Generator in action. By default, Hugo only rebuilds what has changed (Fast Render Mode) but if you prefer this can be disabled using the `--disableFastRender` flag to rebuild the whole site each time.

[Back to Top](#top)

## GitHub and Deploying the site to Netlify

When I created the project, using `git init` in my project directory set it up as a git repository. To deploy the site to Netlify, I first had to commit my local repository to GitHub.

Following the advice of forum posts, I created a [.gitignore](https://www.atlassian.com/git/tutorials/saving-changes/gitignore) file in the root directory of my project to have Git avoid tracking the /public and /resources folder that would be generated when the site is built (since Netlify will take care of this):

```
# Hugo default output directory
/public
/resources

## OS Files
# Windows
Thumbs.db
ehthumbs.db
Desktop.ini
$RECYCLE.BIN/

# OSX
.DS_Store
```

I then used created a remote repository on GitHub and pushed the changes in my local repository to it. See [Adding an existing project to GitHub using the command line](https://help.github.com/en/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line)

Next, I logged into Netlify and clicked New Site from Git button before authorising Netlify to connect to my GitHub account. I chose the repository 'personal-blog' and branch master. Netlify already included the hugo build command and public directory for publishing. See [Hosting on Netlify](https://gohugo.io/hosting-and-deployment/hosting-on-netlify/) for more information.

Now my site's files are stored on Github and any time commits are made - such as when I add a new blog post - Netlify will rebuild the site.

(Note that posts that are draft: true won't show up in production)

[Back to Top](#top)

## Useful Links


https://forestry.io/blog/up-and-running-with-hugo/

https://www.11ty.dev/
https://gohugo.io/
https://www.staticgen.com/

[Back to Top](#top)
