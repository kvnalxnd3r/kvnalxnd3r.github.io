---
title: Elastic Self-managed Installation
date: 2025-04-11 16:25 +0700
categories: [Tutorial, SIEM, ELK Stack]
tags: [elasticsearch, siem, blog, itsecurity]
---

## What Makes Me Start

I just started my first work as SOC Analyst back in march 2025. When I first started, my manager asking me to install and play around with Elasticsearch. An application that I never heard before. My first thought is just "meh, it's just another apps, what could be so difficult about it". 

![Desktop View](/assets/img/posts/2025-04-11-elastic-self-managed-installation/how-do-we-tell-him-mr-krabs.jpg){: width="500"}
_Basically What My Experience Looks like_

Oh boy was I so wrong. Elastic is a giant to learn in itself, so much of features, processes and many other that I should learn from the scratch. So in this post, I would like to begin with how I setting up my Elasticsearch. Hope you guys learn something from this post.

## What is Elasticsearch exactly?

So, what's about this 'Elasticsearch' being so difficult to me when I first encounter it, is that there's three in one shenanigans for this applications. Imagine your system is screaming — “Things are breaking!” — but all the signs are scattered across thousands of log files in weird folders. You don’t have time to read all of them. Elasticsearch is like a giant smart search engine that stores and analyzes those logs fast. It helps you make sense of what’s happening in your system.

So here's a diagram to visualize how Elasticsearch works
![Desktop View](/assets/img/posts/2025-04-11-elastic-self-managed-installation/elastic-diagram.jpg){: width="500"}
_Diagram of How Elasticsearch Works_

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

1. First you need to install the agent from the official Elasticsearch website [here](https://www.elastic.co/downloads/elastic-agent).
2. After downloading it, extract the file and head inside the file of the downloaded agent file and then type cmd on the address bar and press enter. This will make cmd open on that file path.
![Desktop View](/assets/img/posts/2025-04-11-elastic-self-managed-installation/start-cmd-in-agent-folder.png){: width="800"}

3. The cmd shows up, don't enter installation command yet. Head to your Elasticsearch localhost shown in the picture below.
![Desktop View](/assets/img/posts/2025-04-11-elastic-self-managed-installation/elastic-fleet.png){: width="800"}
4. In the Fleet menu, click on "Add Fleet Server".
![Desktop View](/assets/img/posts/2025-04-11-elastic-self-managed-installation/add-elastic-fleet-server.png){: width="800"}
5. Next, the add server menu shows up and this is where we could select where we wanted to put our fleet server. In my case, I put on the localhost address with port 8220(you could get the localhost address from the configuration yml file) and click continue.
![Desktop View](/assets/img/posts/2025-04-11-elastic-self-managed-installation/start-fleet-server.png){: width="800"}
6. The fleet server location has been set, next step is to copy the installation command line to the previously started cmd. But keep in mind that for me, this command line from my localhost Elasticsearch is missing some key command line that I will show on the next step.
![Desktop View](/assets/img/posts/2025-04-11-elastic-self-managed-installation/fleet-server-install-command.png){: width="800"}
7. And for the final step of agent installation is to add this command to the cmd.
```bash
.\elastic-agent.exe install ^
  --fleet-server-es=https://192.168.56.1:9200 ^
  --fleet-server-service-token=AAEAA... ^
  --fleet-server-policy=6be1b061-868b..... ^
  --fleet-server-es-ca-trusted-fingerprint=f54c4b...... ^
  --fleet-server-port=8220
```
For the server service token, server policy, and ca trusted fingerprint it should be generated automatically if you on fresh installment. For my case, this is because I have previously installing agent and uninstall it again. So you should be getting it automatically if it's firt time installment.
Next, paste it on the cmd like picture below.
![Desktop View](/assets/img/posts/2025-04-11-elastic-self-managed-installation/additional-installation-command.png){: width="800"} 

And with that, the Elasticsearch installation should be completed now.

## Setting Up Integration
But we're not done yet, we still need those integration for our Elasticsearch to work properly. So next up is, setting the integration.

1. From the navigation side bar menu, we can find the Integration tab. Click that and we will be routed to integration menu.
![Desktop View](/assets/img/posts/2025-04-11-elastic-self-managed-installation/integration-menu.png){: width="800"} 
2. Once we in the integration menu, we could then search for the relevant integration that satisfy our needs. For this tutorial, I need Kibana for visualizing my log so that I could understand all of the data that being collected. We could use the search bar for finding Kibana. And once Kibana found, click on it.
![Desktop View](/assets/img/posts/2025-04-11-elastic-self-managed-installation/searching-integration.png){: width="800"} 
3. Now that we're in Kibana installation menu, click "Add Kibana" button. This way we can begin the installation process.
![Desktop View](/assets/img/posts/2025-04-11-elastic-self-managed-installation/kibana-installation.png){: width="800"} 
4. Usually, all the previous steps above should be set by default. The one that we want to set however is "Where to add this integration" option. This option asking us in what agent policies that we want to put out integration. For me because my fleet server is on agent policies 8, I have to put my Kibana on that so it can work with the fleet server.
![Desktop View](/assets/img/posts/2025-04-11-elastic-self-managed-installation/kibana-policies.png){: width="800"} 
5. After that, it will give prompt like this. Just click "Save and deploy changes".
![Desktop View](/assets/img/posts/2025-04-11-elastic-self-managed-installation/deploy-agent-and-integration.png){: width="800"}
6. And wait for the installation to completed and *voilà* our integration installed successfully.
![Desktop View](/assets/img/posts/2025-04-11-elastic-self-managed-installation/complete-integration.png){: width="800"}

## But What Agent Policies?
Now you might be wondering, "What is this policies exactly? Are they like rules? Or what?". Now when I first installed Elastic I also have the same question and turns out, Agent Policies are like a set of instructions that tells each Elastic Agent what to do, what to collect, and where to send it. It’s like handing your agent a to-do list:

- What logs or metrics to monitor (e.g., Windows Event Logs, system metrics)
- Which integrations are enabled (like Winlogbeat, Filebeat, System, etc.)
- Where the data goes (usually to Elasticsearch)
- Whether the agent can auto-update or be centrally managed

Each device running an Elastic Agent is tied to one of these policies. So if I have 5 computers with Elastic Agents and they’re all doing the same job (like collecting Windows logs), I can put them under the same policy. That way, if I change something in that policy — boom 💥 — it applies to all of them at once. Super convenient.

Elastic even lets you have multiple policies so you can customize agents differently for servers, workstations, cloud VMs, etc.

## The Dashboard
And with all of the installation process, here's what Elasticsearch dashboard look like if the installation succeeded.
![Desktop View](/assets/img/posts/2025-04-11-elastic-self-managed-installation/elastic-dashboard.png){: width="800"}

## Final Thoughts
This blog isn’t meant to be perfect. It’s real. If you’re learning Elastic and security monitoring with just one machine and zero prior experience, I hope this helps. My goals for writing this blog is to help others and future me in-case need to start from zero again on the elastic installation, so this blog could make as an knowledge based to others and me too.

Thank you so much for reading this and have a nice day where ever you are.