---
title: Creating Elastic Login Dashboard 
date: 2025-06-16 14:40 +0700
categories: [Tutorial, SIEM, ELK Stack]
tags: [elasticsearch, siem, windows, dashboard, login dashboard, detection]
---

## Before We Start

Welcome to another post of me yapping about cyber security. This time, I want to yap about how to build your own dashboard in Elastic. This post is done with objective of allowing us to understand how to build dashboard that suit your need or my need while also learning the progress of making it along the way. 
![Desktop View](assets/img/posts/2025-06-16-creating-elastic-login-dashboard/bild-meme.png){: width="650"}
_Bild 👍_

With that being said, the type of dashboard that we will be building is login/logon dashboard that designed to track and count how many attempt of login that happened to our machines that have Elastic agent installed inside it. This will helps us better understand what or who could make this attempt possible, the intention of this attempt, source of this attempt and many more important information that could help us understanding and recognizing threats.

This post is inspired by [**Tech with Jono**](https://www.youtube.com/@TechwithJono/featured) video of titled [**IMPROVE Your Cybersecurity Portfolio like this...**](https://www.youtube.com/watch?v=JWQY-0WsFSE&t=303s&pp=0gcJCdQJAYcqIYzv) and in that video there's mentioning about making dashboard as portfolio and I thought, "yeah, that is also important and it rhymes with my [brute force post](/posts/brute-force-attack-using-rdp)." and here we're creating the post about it.

## What Is Dashboard?

A little bit of background explanation to understand what is 'dashboard'. From Elastic [official documentation](https://www.elastic.co/docs/explore-analyze/dashboards) we know that **dashboard is best way to visualize and sharing insight from your Elasticsearch data**. So with that visualization we could make charts, tables, metrics, and also maps in order to help enhancing the visual and helping us to understand better.

![Desktop View](assets/img/posts/2025-06-16-creating-elastic-login-dashboard/dashboard-menu-page.png){: width="800"}
_Default Page For Dashboard Menu_

Picture above showing us what dashboard main page looks like and as you can see there's many existing templates that you could choose for the visualization. But for now we don't really need this but in the near future you're being asked to make a custom dashboard that not available from the templates. 

And for that what would do? Well, we make our own dashboard and that is what this post about. I know from the picture above, I already made one of login/logon dashboard but I will make another one that will **track failed logon attempt**. Without much further yapping, let's get into it.

## Creating New Dashboard

On the main **Dashboard page** click on **Create dashboard** button and this will direct us to **Editing page** where here we could use what type data that we want to visualize, what query it will be using(yes, they use query too for retrieving data or information that we want), and many more options that could help us with dashboard making process.

![Desktop View](assets/img/posts/2025-06-16-creating-elastic-login-dashboard/editing-new-dashboard-page.png){: width="800"}
_New Dashboard Creation Page_

Above as you can see, it's what I called customization page where we could choose what type of visualization that we want. Because I'm also still learning and didn't know much about dashboard feature and it's full capabilities, I'll just utilize the existing template available from Elastic library by clicking **Add from library** button and it will show **Add from library** page on the right side, like shown below.

![Desktop View](assets/img/posts/2025-06-16-creating-elastic-login-dashboard/dashboard-template-page.png){: width="800"}
_Template Lists of Visualization_

From that library menu on the right we now then could search for visualization that we wanted, for me I just search using key word **logon** on the search bar and it will shows all templates that related to it. For this dashboard, because we want it to track failed logon attempt, choose **failed logon tsvb, logon failed table, failed logon heatmap, logon failed account, logon failed source ip** as the visualization and after you choosing it, close the page and let's see what the templates looks like.

![Desktop View](assets/img/posts/2025-06-16-creating-elastic-login-dashboard/failed-logon-visualization-template.png){: width="800"}
_Failed Logon Attempt_

As you can see, the template's working and showing data of failed logon attempt, also notice that I use **Last 7 days** as time range for it. This is because I want to catch as many as possible of failed logon attempts, and looks like I got so many hits with it. That's perfect but for me there's still one missing thing and that is charts to help us understanding the statistics more better.

Because I cannot find any charts or graphics that suit my need, I'll just create one myself. To do that, just click on **Create visualization** button on the left side of your page and you will be shown **Edit visualization** page like this.

![Desktop View](assets/img/posts/2025-06-16-creating-elastic-login-dashboard/edit-visualization-page-horizontal-axis.png){: width="800"}
_Creating New Visualization Chart And Customizing Horizontal Axis_

There's a lot in here but what we want to focus first is on the middle part of the page, the drop down menu to be exact. If you click on it, you will be given options of what kind of charts or graphics that you want and for me I choose **Bar vertical stacked** to help me visualizing my data.

After you choose the type of charts head to the right side of the page and this time we will determined what data will be shown on the chart. I start with the horizontal part of my bar chart and this part, I choose **@timestamp** and it will shows us timestamp data of the events that happening. The interval that I will be using is **3 hours interval** and for the the name that will be displayed is **@timestamp failed logon**.

Horizontal is done, now let's get the vertical done too. Oh and also just **close the horizontal axis menu** and **choose vertical axis** to start customizing it like picture below.

![Desktop View](assets/img/posts/2025-06-16-creating-elastic-login-dashboard/edit-visualization-page-vertical-axis.png){: width="800"}
_Creating New Visualization Chart And Customizing Vertical Axis_

We start by choosing **Count** as the function and the field will be **Records**. Now the chart would look like what shown in the middle. And there's one final touch before we done, that is inserting queries. The queries will be ``data_stream.dataset : windows.security OR data_stream.dataset : system.security OR event.code : "4625"``.

![Desktop View](assets/img/posts/2025-06-16-creating-elastic-login-dashboard/failed-logon-attempt-queries-chart.png){: width="800"}
_Queries For Failed Logon Attempt Chart_

And with those queries, our chart will be more accurate in showing data and gaining hits within the system. Thus, makes us better understand the situation. Now all you need to do now is click on **Save and return** button, then you will be directed back to the main customization page to naming the panel that we just previously created.

![Desktop View](assets/img/posts/2025-06-16-creating-elastic-login-dashboard/naming-newly-created-dashboard-panel.png){: width="800"}
_New Dashboard Panel Name_

You can name it whatever you want, as for me I'll just name it **Logon Failed - Logon Failed Timeline [Windows System Security]**, and after that just click **Save**.

Now for me, the panels on this dashboard is enough but what's left is to edit the positioning for the layout of this dashboard. For that, I'll just start by **dragging** the panel to where I want it. The dashboard would look like this after I done editing the layout.

![Desktop View](assets/img/posts/2025-06-16-creating-elastic-login-dashboard/dashboard-finalization-1.png){: width="800"}

![Desktop View](assets/img/posts/2025-06-16-creating-elastic-login-dashboard/dashboard-finalization-2.png){: width="800"}

![Desktop View](assets/img/posts/2025-06-16-creating-elastic-login-dashboard/dashboard-finalization-3.png){: width="800"}
_Failed Attempt Dashboard Finalization Edit_

Now we're truly done with our dashboard, just save it by clicking **Save** button on the upper right corner of the page and then a window will pop up for us to entering the name of our dashboard and the description to describing it.

![Desktop View](assets/img/posts/2025-06-16-creating-elastic-login-dashboard/saving-newly-created-dashboard.png){: width="800"}
_Saving Dashboard_

I just naming it **Home Lab - Failed Logon Windows** and put some description on it. You can just put some name and description on it but be sure to make it meaningful. Now let's check whether our newly created dashboard is on the list or not. Let's return back to the dashboard main page and see.

![Desktop View](assets/img/posts/2025-06-16-creating-elastic-login-dashboard/new-dashboard-on-the-list.png){: width="800"}
_New Dashboard On The List_

## Checking On Success Logon Attempt Dashboard

Now, let's check on the successful logon attempt dashboard to see some difference between the failed one. Just click on it on the list and you will inside the edit page of it.

![Desktop View](assets/img/posts/2025-06-16-creating-elastic-login-dashboard/viewing-success-logon-dashboard.png){: width="800"}
_Successful Attempt Dashboard Panel_

We're inside it and for us to gaining full access for this page is just by clicking the **Edit** button on the right corner of the page. After you done switching to **Edit** mode, let's observing this dashboard panel.

![Desktop View](assets/img/posts/2025-06-16-creating-elastic-login-dashboard/viewing-panel-on-edit-mode-1.png){: width="800"}

![Desktop View](assets/img/posts/2025-06-16-creating-elastic-login-dashboard/viewing-panel-on-edit-mode-2.png){: width="800"}

As you can see there's a couple different from our failed attempt dashboard and that is in this successful dashboard there's two charts and the details table. The other chart is to track logon event and administrator logon(but I just disabling it because for me I just want full view specifically for the successful logon attempt), as for the details it track the time, username, source IP, and the ID of the winlog.

Also, let's take a closer look of the bar chart and see what is inside of it.

![Desktop View](assets/img/posts/2025-06-16-creating-elastic-login-dashboard/bar-chart-of-successful-attempt.png){: width="800"}
_Successful Attempt Bar Chart_

There's not much difference from the failed attempt other than the query of ``event.code`` is specified to take event ``4624`` and some parameters like ``winlog.provider_name : Microsoft-Windows-Security-Auditing`` and ``NOT Logon Failed``. Now let's just head back to the main page.

![Desktop View](assets/img/posts/2025-06-16-creating-elastic-login-dashboard/back-to-dashboard-main-page.png){: width="800"}
_Back to Main Page_

And that's it, we finally have two dashboard ready to use for tracking logon events from our machine. Just click on **Switch to view mode** button if you want to exit properly.

## Final Words

Thank you so much for reading this and hopefully you guys gained some knowledge from it and help you using Elastic. Really appreciate your time and see you on the next post.



## References

| References                                                                                                                                  |
| :------------------------------------------------------------------------------------------------------------------------------------------ |
| Tech with Jono. [IMPROVE Your Cybersecurity Portfolio like this...](https://www.youtube.com/watch?v=JWQY-0WsFSE&t=303s&pp=0gcJCdQJAYcqIYzv) |
| Elastic Dashboard Documentation. [Dashboard](https://www.elastic.co/docs/explore-analyze/dashboards)                                        |
