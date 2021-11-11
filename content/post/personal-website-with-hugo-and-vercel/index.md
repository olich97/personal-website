---
author: Oleh Andrushko
title: Building a personal blog with Hugo and Vercel
date: "2021-11-09"
description: Guide to create and deploy your own personal website/blog with Hugo
categories: 
- Programming
- Hugo
  
tags: 
- hugo
- blog
- programming
- website
- vercel
  
image: your-personal-website-with-hugo.png

readingTime: 10

draft: false
---

It's been a while that I was wondering what could be a better way to improve my English communication and writing skills? <br>
What about starting a technical blog? <br>
I think in this way I could have fun, keep learning a lot of new stuff and improve my skills by actually practicing it, while building something of my own.<br>

So here I am writing my first blog post on how I actually build this blog 🙂 Let's goooooooooo 💪 <br>
> P.S: Sorry in advance for my rough English

In this post I'll try to explain how to set up a personal site or blog with [Hugo](https://gohugo.io/) for **free**, by creating a [continuous deployment workflow](https://en.wikipedia.org/wiki/Continuous_deployment) with [GitHub Actions](https://github.com/features/actions), deploying and hosting our site on [Vercel](https://vercel.com) platform.

## Why Hugo?

[Hugo](https://gohugo.io/) is a **cross-platform**, **open-source** static site generator written in [Go](https://golang.org/).

> A static site generator is a build tool that produces sites that have pages built beforehand and do not change often between user visits. 
Those pre-rendered pages are hosted where users can access them, a quite different approach unlike dynamic sites we're used to see where a page is built when a user requests it. <br>
Static sites involve a lot fewer moving parts and are easier to build, secure, and maintain. They are fast since pages are already pre-rendered and are served as is. Scaling static sites is easier compared to other sites with more complex infrastructures.

There are a lot of [static sites generators](https://jamstack.org/generators), but here I'll list some of advantages/features that I found quite useful and for which I chose Hugo.

### Fast & Flexible
Hugo is pretty flexible. 

I started this blog with simple open source theme ([there are a lot of them](https://themes.gohugo.io/)), then changed it completely over time. 
Sometimes I want to do things (e.g custom tracking, comments integration) in my website that are out of the scope of a simple blog, and Hugo allows to create those things.

Another reason I chose Hugo is because it is fast. 
First, it has Go at the core, which is known to be a fast compiled language. Also [Go's Goroutines](https://golangbot.com/goroutines/) are a perfect match for many of the tasks that Hugo performs on every build. Things are made to be as fast as possible. 

For example, it comes with [Live Reload](https://gohugo.io/getting-started/usage/#livereload) built in and every time changes are detected it takes milliseconds to see the result:
```shell
Change detected, rebuilding site.
2021-11-09 23:57:27.805 +0100
Source changed "path\\to\\post\\personal-website-with-hugo-and-vercel\\index.md": WRITE
Total in 17 ms
```

### Markdown
I think hugo is a right choice for a technical blog, especially if you are a developer and you’re willing to write in Markdown. Non-tech people might just refuse to use Markdown, and it’s perfectly understandable.

Also, you have to be prepared for a Git-centric workflow for publishing your posts (or you can use some cms solutions like [Strapi](https://strapi.io/integrations/hugo-cms) or [Forestry](https://forestry.io/)). Most people would disagree with me on using `git commit` and `git push` just for publishing a post, but I found it pretty cool.

For example, my process for writing a new post is:

1. Write a post using Markdown
2. Commit my changes to a GitHub develop branch
3. Some glue technology deploys the changes on the Vercel preview server
4. Take some days to review the post in preview environment, make some improvements and then I am satisfied create a pull request towards the main branch
5. After self-accepting the PR, the same glue technology deploys all new stuff on the main server visible to all the world

## Prerequisites

Here are some stuff that you're gonna need in order to make the magic happens:
- [Installation](https://gohugo.io/getting-started/installing/) of Hugo version `0.80.0` or higher
- A free subscription of [Vercel](https://vercel.com/signup) account

## Step 1: Installation

Here I need to explain how to install stuff listed in Prerequisites

## Step 2: Create your website

Here I need to explain how to create a hugo website, use a theme, configure the website.
Push and create a website repository

[Theme to use](https://themes.gohugo.io/themes/hugo-paper/): https://github.com/nanxiaobei/hugo-paper

## Step 3: Deploy your website

Prepare vercel account, create and commit git workflow, publish the website.
Explain how vercel works and how personalize domains.

## Step 4: Enjoy

Some demo stuff

## Conclusion
