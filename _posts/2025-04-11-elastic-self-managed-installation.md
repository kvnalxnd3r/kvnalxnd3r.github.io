---
title: Elastic Self-managed Installation
date: 2025-04-11 16:25 +0700
categories: [Installation, Tutorial]
tags: [elasticsearch, siem, blog, itsecurity]
author: kev
---

## What Makes Me Start

I just started my first work as SOC Analyst back in march 2025. When I first started, my manager asking me to install and play around with Elasticsearch. An application that I never heard before. My first thought is just "meh, it's just another apps, what could be so difficult about it". 



Oh boy was I so wrong. Elastic is a giant to learn in itself, so much of features, processes and many other that I should learn from the scratch. So in this post, I would like to begin with how I setting up my Elasticsearch. Hope you guys learn something from this post.

## What is Elasticsearch exactly?

So, what's about this 'Elasticsearch' being so difficult to me when I first encounter it, is that there's three in one shenanigans for this applications. Imagine your system is screaming — “Things are breaking!” — but all the signs are scattered across thousands of log files in weird folders. You don’t have time to read all of them. Elasticsearch is like a giant smart search engine that stores and analyzes those logs fast. It helps you make sense of what’s happening in your system.

So here's a diagram to visualize how Elasticsearch works


As you can see on the diagram, there's so many components accompanying Elasticsearch itself. And from my experience alone here's some of steps or things that I learn during my trial and errors.

**What I learned:**
- Always follow the *correct order of installation*: Elasticsearch → Kibana → Fleet.
- Keep your tokens and fingerprints handy, don't forget them.
- Watch logs closely during setup — they often explain what’s wrong.

## The Installation Procedures
So with that in mind, the steps that I follow to installing Elasticsearch and Kibana is this video below by **Wasay Tech Tips**.
{% include embed/youtube.html id='A0z3acG9ncE' %}

## What's Missing From The Installation
But, here's the catch. After I done following and installing Elasticsearch and Kibana based on that video, I still don't have log or any data showing up on my Elasticsearch. And turns out, I need to install the Agent for it to be working properly.

So the way Elasticsearch works is that the node or in this case my laptop to be able to sent log or data to Elasticsearch is by having agent installed on my device. So that is what I do, installing the agent and here's how I do it.
