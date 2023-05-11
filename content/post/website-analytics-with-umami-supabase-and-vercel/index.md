---
author: Oleh Andrushko
title: "Build your own privacy-focused website analytics platform with Umami, Supabase, and Vercel"
date: "2022-04-15"
description: A complete guide to setup a self-hosted, privacy-focused alternative to Google Analytics platform for free. Umami is a simple, fast, privacy-focused alternative to Google Analytics. Supabase is an open source Firebase alternative database. Vercel is the platform for develop, preview and ship frontend applications. All mixed toghether is nice and simple solution for tracking your personal websites usage.

categories: 
- Programming
  
tags: 
- analytics
- programming
- website
- vercel
- supabase
  
image: header.png

readingTime: 15

draft: false
---
Are you tired of using website analytics platforms that compromise your users' privacy and deal with consent banners on your website? Do you want to build your own analytics platform that respects your users' privacy and gives you full control over your data? If so, then this guide is for you.

Traditional website analytics platforms often compromise user privacy by tracking user data without their consent or knowledge and storing it on servers the user has no control over. While there are privacy-respecting platforms available like [Plausible](https://plausible.io/), sometimes all you need is a basic user traffic analytics system that is easy to set up and use.

In this article, I will explore how to build your own privacy-focused website analytics platform using open-source tools like Umami, Supabase, and Vercel. With this guide, you can learn how to create a platform that respects user privacy, gives you full control over your data, and is accessible to everyone.

## Why Umami?
[Umami](https://umami.is/) is a great choice for a privacy-focused website analytics platform because it is open-source and easy to set up. It provides all the basic features you need to track user traffic and customize your analytics. 

One of the key benefits of using [Umami](https://umami.is/) is that it is lightweight and fast, which means it won't slow down your website. Additionally, [Umami](https://umami.is/) is designed to be privacy-focused, which means it doesn't collect any personally identifiable information (PII) from your users. This is important because it helps you comply with data privacy regulations like GDPR and CCPA.

[Umami](https://umami.is/) provides a clean and intuitive user interface, making it easy to navigate and understand your analytics data. You can view your website traffic by day, week, or month, and see which pages are the most popular. You can also see where your traffic is coming from, including countries, cities, and referral sources. [Umami](https://umami.is/) also provides real-time analytics, which means you can see your traffic data updated in real-time as users visit your site.

> [Umami Dashboard Live Demo](https://analytics.umami.is/share/LGazGOecbDtaIwDr/umami.is)

## Prerequisites
- [Git](https://git-scm.com/) version control system installed
- A free subscription of [Vercel](https://vercel.com/signup) account
- A [GitHub](https://github.com/signup) account
- A [Supabase](https://supabase.com/) account
- [yarn](https://yarnpkg.com/) package manager: install with `npm install -g yarn`

## Step 1: Setup Supabase
[Supabase](https://supabase.com/) is an open-source backend-as-a-service platform that makes it easy to build serverless applications. We will use Supabase to store our analytics data. 

1. Login to Supabase and create a new project in a target region:
   ![Supabase - New Project](supa_db_new.png) 
2. Go to **Project Settings > Database** page of newly created project.
3. Copy the connection string from Connection Polling Section:
   ![Supabase - Connection String](supa_conn.png) 
4. Add `?pgbouncer=true` to the connection string you just copied and save it for later: 
```
postgres://postgres:[YOUR-PASSWORD]@db.blablabla.supabase.co:5432/postgres?pgbouncer=true
```

## Step 2: Setup Umami
1. Go to [Umami github repository](https://github.com/umami-software/umami)
2. [Fork](https://docs.github.com/en/get-started/quickstart/fork-a-repo) the repository
3. Clone the forked repository locally:
```shell
git clone https://github.com/olich97/umami.git
```
4. Go to cloned folder and create a `.env` file in the root of the repository with the content (connection string to the supabase database created at Step 1):
```
DATABASE_URL=postgres://postgres:[YOUR-PASSWORD]@db.blablabla.supabase.co:5432/postgres?pgbouncer=true
```
5. Install dependencies:
```shell
yarn install
```
6. Build the project:
```shell
yarn build
```
7. Start the project:
```
yarn start
```
8. Follow the [first Login](https://umami.is/docs/login) guide and change your default **username** and **password**

## Step 3: Deploy Umami on Vercel

1. Login to your [Vercel account](https://vercel.com/)
2. Add new project
3. Choose, cloned at Step 2, umami repository:
   ![Vercel - Import](git_import.png) 
4. In the configuration page, under **Envinronment Variables**, specify your `DATABASE_URL` variable: 
   ![Vercel - Project Settings](vercel_settings.png) 
5. Press **Deploy**
   ![Vercel - Deployment](success_deploy.png) 

> Vercel will assign a random domain to deployed website. Follow [the guide](https://vercel.com/docs/concepts/projects/domains/add-a-domain) to setup a custom domain.

## Step 4: Track your website

1. Login to newly deployed umami frontend
2. Create a new website: **Settings** -> **Websites** -> **Add Website**.
   ![Umami - Add Website](create_website.png)
3. Get the tracking code: **Settings** -> **Websites** -> **Your Website** -> **Edit** -> **Tracking Code**.
4. Add the script to the website you want to track.
5. Enjoy!

> For more about umami's capabilities read [the documentation](https://umami.is/docs/getting-started).

## Conclusion
By using Umami, Supabase, and Vercel, you can create a simple platform that respects user privacy and gives you complete control over your data. Open-source tools allow you to create an analytics platform that is accessible to everyone, regardless of budget or technical expertise. So, go ahead and give it a try!