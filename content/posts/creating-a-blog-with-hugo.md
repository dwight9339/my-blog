+++
title = "Creating a Blog with Hugo and a General Guide to Static Site Generation"
date = 2024-04-10
draft = false
tags = ["Web Development", "Hugo"]
author = "David White"
+++
## Introduction
In today’s world of Single Page Apps, server-side rendering, and new Javascript frameworks appearing before you can even finish adding the last hot one to your resume, the idea of building a static site might seem adorably quaint. But the idea that static sites are obsolete is not just naive, but simply untrue. Dynamic sites offer the sexy allure of almost limitless flexibility and powered the transition from Web 1.0 to Web 2.0, but at what cost? When your site needs to query a database before it loads, well, that takes time. Nothing’s faster than having a page ready to go before you’ve even navigated to it. In addition to maximum rendering speed, a minimal attack surface, improved reliability, simple maintenance, and more make static sites look like the only logical choice for certain applications. Like this very blog, for instance! In this post I’ll introduce static site generators and delve a little more into static vs dynamic websites and the tradeoffs involved all while I guide you through how I created my personal blog site using Hugo, my personal favorite static site generator.

## What is a Static Site Generator?
“But wait,” you say “it just seems like a pain to have to type out all boilerplate HTML to make a static site. I can feel the carpal tunnel already…” You make a fair point. Manually writing out all the HTML, CSS, and structure for a website can be a tedious and time-consuming process, especially as sites grow in complexity. Enter static site generators (or SSGs, as the cool kids call them) to save you from the endless abyss of <div> tags and a lifetime of wrist braces.
Static site generators are tools that automate the process of generating complete, ready-to-deploy sites from simpler source files like Markdown documents, HTML templates, and configuration settings. At their core, these tools take your content and assets, apply layouts and styles, and output a set of static HTML, CSS, and JavaScript files that together make a complete static website.
The key benefits of using a static site generator include:
Faster build times and page loads - Since the pages are pre-generated, there's no need for server-side processing on each page load, leading to lightning fast performance.
Simplified hosting - With no server-side logic required, you can host a static site on simple, inexpensive storage like S3, Netlify, or Github Pages.
Version control friendly - The source files for your site (Markdown, templates, config) can be easily tracked in a Git repository.
Content-focused authoring - Writers and content creators can focus on the content itself in Markdown, rather than wrestling with HTML.
Scalability - Static sites can handle high traffic loads without needing to scale complex server infrastructure.
There are quite a few popular static site generators to choose from, including Jekyll, Gatsby, Next.js, and the one I'll be focusing on - Hugo.

## Why Hugo?
Hugo is a fast and flexible static site generator written in Go. It has a number of appealing features that made it the right choice for my personal blog:
Speed - Hugo is renowned for its blistering build times, often completing full site rebuilds in under a second. This makes the development workflow incredibly smooth.
Simplicity - The Hugo documentation is excellent, and the learning curve is quite gentle compared to some other generators. It follows the KISS (Keep It Simple, Stupid) principle.
Flexibility - Hugo has support for a wide range of content types, templates, and plugins, allowing for highly customized site designs.
Large & Active Community - Hugo has a thriving community of users and contributors, ensuring ongoing support and development.
No Runtime Dependencies - Unlike some generators that require a Node.js runtime, Hugo is a standalone binary, making deployment a breeze.

## Getting Started with Hugo
We’ll first need to install Hugo. I’m using Ubuntu Linux running in WSL on my Windows host machine. Check out the installation guides to find instructions for your OS. Once Hugo’s up and running, we’ll create our project directory.
hugo new site personal-blog
This command creates a scaffold for a new Hugo project. The project directory includes a `hugo.toml` file, where we’ll add all of our site configuration settings, and various folders for different site assets. Next we’ll initialize a git repo for the project and install a theme as a git submodule. I used the binario theme. You can browse other themes featured by Hugo [here](https://themes.gohugo.io/).
```
cd personal-blog
git init
git submodule add https://github.com/vimux/binario themes/binario
```
We can then run `hugo server` to build and serve our site. If we head over to `http://localhost:1313`, we see this
![A barren-looking web page](/images/posts/creating-a-blog-with-hugo/blank-site.PNG)

It doesn’t look like much yet but it’s something! Next we’ll need to add some content to display on our site.
```
hugo new content posts/creating-a-blog-with-hugo.md
```
This creates a new markdown document with an initialized page setting header.
![A brand new markdown file](/images/posts/creating-a-blog-with-hugo/blank-markdown.PNG)

Settings parameters within the header are used by Hugo and within themes to set anything from the title of the post to its publication date, categories, tags, and even specific layout templates. For instance, in the binario theme, you might set parameters to customize how your post appears, including its featured image, author name, and description. Underneath the header you can add the actual page content as markdown.
![A markdown document with more info filled in](/images/posts/creating-a-blog-with-hugo/page-with-markdown.PNG)
 
And now we can head back over to the browser and navigate to `/posts/creating-a-blog-with-hugo` to see our rendered page in all its glory.
![The web page with content now filled in](/images/posts/creating-a-blog-with-hugo/rendered-page.PNG)

This about covers the most basic usage of Hugo. Once you have Hugo installed and know how to install and configure a theme and create page content markdown you have everything you need to create a full static web site in next to no time at all! But, of course, this only scratches the surface. There's a whole host of topics about Hugo and its powerful features and developing or modifying themes that I haven't even touched (and, honestly, haven't even looked that deeply into myself) to create even more impressive sites. That's the beauty and power of flexible systems like Hugo, it's simple when you want or need it to be while still hiding a whole host of added capabilities when you find that you want to improve your products further!

## When is a Static Site No Longer Static?
As I was writing this blog post, I repeatedly found myself asking where exactly the line between a static page and a dynamic page actually lies. In recent years, the JAMstack (JavaScript, APIs, and Markup) model has gained popularity. This model involves building static pages that also include JavaScript which can load some data in the page dynamically after the page itself loads. It might seem counter-intuitive or even contradictory to say that a JAMstack page which loads data dynamically from a server is actually a static page. But, as we'll see, the dichotomy isn't so cut and dry.

Let's look at an example. Suppose you were building an e-commerce site and each product had its own static page stored on the server. However, suppose that while the most important information about the product, its description, price, and photos, were all present in the page's static HTML and the server's assets folder, it also included JavaScript to query the backend and load the current inventory for the product and adjust the displayed price based on any sales that might be going on. If you're like me, you might say that dynamically loading *any* data would technically exclude these product pages from being considered "truly" static. And while this opinion may *technically* be correct (ahem), the fact that unique static pages for each of the store's products exists on the server and in CDNs everywhere might make one reconsider this hardline point of view. From a fully dynamic development perspective, it makes much more sense to simply write one product page that queries the back end and fills in the product info dynamically rather than having a separate page for each product. Then, if we throw server side page rendering, where a page might not necessarily exist on the server for each product but is fully built on the back end and pushed up to a CDN when queried, into the mix as well, then our previously held concrete views on what makes a page *static* or *dynamic* quickly begin to crumble. 

Ultimately, the distinction between a static page with dynamic features and a fully dynamic page is more of a spectrum than a binary classification. As you add more dynamic elements and rely more on server-side processing, you move further towards the dynamic end of the spectrum and vice-versa as you remove them. In practice, many modern websites and applications fall somewhere in the middle of this spectrum, using a combination of static generation and dynamic loading to balance performance, scalability, and interactivity. The key is to understand the tradeoffs involved and choose the approach that best fits the specific requirements and constraints of your project.

## Conclusion
We discussed the benefits of static sites, discussed the trade-offs between static and dynamic pages, and looked into how the two approaches have become hybridized in modern web development. We also dipped our toes into developing sites using Hugo and the powerful features and simple, convenient workflow it provides. I hope you found this exploration as informative and entertaining as I did and I hope you check out my next post where I plan to deploy my own self-hosted version of the open-source comment engine [Remark42](https://remark42.com) and add it to my blog while also exploring the world of self-hosting services more generally. See ya!