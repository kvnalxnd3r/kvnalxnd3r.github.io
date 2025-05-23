---
title: Detecting Windows Logon Events 4624-4625 With Elasticsearch As a SIEM
date: 2025-04-12 13:40 +0700
categories: [Tutorial, SIEM, ELK Stack]
tags: [elasticsearch, siem, windows, security events, detection]
author: Kevin Alexander
---

## Detecting My First Event

First time I'm diving into cyber security and being tasked to explore Elasticsearch and it's functionality, that is where I'm really in a dark as to how and where to begin with this journey ahead. But then I watch some videos on youtube and in it Elasticsearch can be used for events detection. At that time, there's a light bulb lights up on my mind because I thought to myself "How about I simulate a basic events using this local Elasticsearch that I've just installed and see what is it looks like in the Kibana? And along the way I could also learn other cyber security concept. Two birds with one stone" I said to myself.

![Desktop View](/assets/img/posts/2025-04-12-detecting-windows-logon-vents-4624-4625-with-elasticsearch-as-a-lightweight-self-managed-siem/peter-griffin-light-bulb-idea.jpg){: width="700"}
_My Thought Process Looks like This Fr Fr_

So with this post I'll explain and walk through about what is my setup for this scenario, the configuration for the setup, what kind of queires for logs that I used, what I learn from this little experiment, and some bits about what I should know more. So if you feels like just like me, new to Elasticsearch as SIEM or maybe just cyber security in general especially blue team, I hope this experience of mine helps you.

## What Is Event ID 4624 & 4625? And Why Need To Detect Them?

So for starter, all of the processes and events that is happening inside the computer are being recorded and stored in a form of logs that being specified by the system's audit policy. And that is called [**Windows Security Logs**](https://en.wikipedia.org/wiki/Windows_Security_Log), where it's offer an immense amount of insight into what's happening on a computer/machine. Our so called 'main highlight' for today is event ID 4624 & 4625.

But before we delve more in event ID 4624 & 4625, let me explain what exactly does 'ID' means here. So in term of [windows event logging](https://learn.microsoft.com/en-us/windows/win32/eventlog/event-identifiers), 'ID' is referring to unique identifier that uniquely identifies particular events that help define it's description so that viewers can present these description to the user and thus helping user solving their own problems.

And with that said definition we could finally explain and understand better what are those two events exactly.

- ✅ Event ID 4624 – Successful Logon
  - Generated when user logs on successfully to a computer/machine.
  - This is very Useful for understanding who logged in, from where, and using what method.
  
- ❌ Event ID 4625 – Failed Logon
  - Generated when a logon attempt fails by user.
  - Extremely useful for detecting brute force attacks, incorrect password attempts, or even internal misuse.

And with that information and adding documentation from [Microsoft's official documentation](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4624) and the [event ID reference](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4625), we could at least expect the information would be provided with fields like: 

- TargetUserName: Who tried to log in

- IpAddress: From where

- LogonType: What method (RDP, Interactive, Network, etc.)

- FailureReason: For 4625, why the login failed

- ProcessName: What app triggered the login

As we can see, the information really essential for us to help spot some patterns, suspicious behaviour or unsual acitivity in general.

## The Necessary Tools

For the stack that I used in this little project to gather, store, and visualize the logs being gathered are as follows:

- Winlogbeat (Agent installed on Windows host)

- Elasticsearch (Storage & querying engine)

- Kibana (Dashboard for data exploration)

- Windows Installed Device(Generating real login events)

This stack is a part of the Elastic Stack (ELK), which is widely used for log management and lightweight SIEM purposes.

## Step-by-Step Setup

- ✍️ Step 1: Install Winlogbeat on the Windows Host

Download and install Winlogbeat from the official [Elastic downloads page](https://www.elastic.co/downloads/beats/winlogbeat).

Edit the configuration file (winlogbeat.yml):
```yml
winlogbeat.event_logs:
  - name: Security
    event_id: 4624, 4625
```

Specify the destination for Elasticsearch:
```yml
output.elasticsearch:
  hosts: ["http://localhost:9200"]
```

Enable the Winlogbeat service:
```bash
.\winlogbeat.exe install-service
Start-Service winlogbeat
```
Now that the winlog is set, we then can proceed to next step. Connecting the winlog to Elasticsearch and Kibana.

- 🔗 Step 2: Connect Elasticsearch and Kibana

  - Ensure Elasticsearch is up and running: http://localhost:9200

  - Open Kibana: http://localhost:5601

  - Navigate to Stack Management > Index Patterns and create a pattern like **winlogbeat-***
  
## Start Querying Those Windows Logon Events in Kibana

With all of the setup being done and Kibana is ready to use, head to **Discover** menu on the Kibana like in the picture below.
![Desktop View](/assets/img/posts/2025-04-12-detecting-windows-logon-vents-4624-4625-with-elasticsearch-as-a-lightweight-self-managed-siem/kibana-discover-tab.png){: width="800"}
_Kibana Discover Working Properly Showing Logs & Events_

After inside the Kibana Discover tab, we could see all of what's happening inside our machine/computer. This will give us a clear information to pin point the threats, adverseries, and any other suspicious activies.

The next steps to see the wanted events is to input the queries. We can input the queries like shown in the picture below alongside the necessary queries.
![Desktop View](/assets/img/posts/2025-04-12-detecting-windows-logon-vents-4624-4625-with-elasticsearch-as-a-lightweight-self-managed-siem/event-4624.png){: width="800"}
_Successfully Filtering Event ID 4624_

The language being used in the picture above is called KQL(Kibana Query Language) but before we continue with the event. Let me briefly expalain what is KQL in my understanding.

- 🧠 So What is KQL (Kibana Query Language)?
It's a set of search syntax in Kibana to filter and explore log data within Kibana that is designed to be intuitive and user-friendly for analysts to be able build queries without writing complex raw Elasticsearch DSL.

They way it being structured is by having field-based searches, logical operators, and even wildcards to filter events quickly. Here's the composition of it explained in table form:

| **Component**      | **Example**                                                | **Description**                                                           |
| ------------------ | ---------------------------------------------------------- | ------------------------------------------------------------------------- |
| Field match        | `event.code: "4624"`                                       | Match entries where `event.code` is exactly `4624`.                       |
| AND / OR operators | `event.code: "4625" AND winlog.event_data.LogonType: "10"` | Combines multiple conditions. `AND` = both must match. `OR` = either one. |
| Grouping values    | `event.code: ("4624" or "4625")`                           | Matches either value in a field.                                          |
| Wildcard search    | `TargetUserName: "admin*"`                                 | Matches values that start with `admin` (e.g., `administrator`, `admin1`). |
| Phrase match       | `ProcessName: "C:\\Windows\\System32\\svchost.exe"`        | Searches for exact phrases, often used with file paths.                   |
| Negation           | `NOT TargetUserName: "SYSTEM"`                             | Excludes logs where username is `SYSTEM`.                                 |
| Time range         | `@timestamp >= "now-1d/d"`                                 | Shows only logs from the last day. Supports relative time filtering.      |

## How To Simulate Event ID 4625

With the knowledge of how KQL structured and compose we can now then simulate how to make the event appear. Because event 4624 is a success logon, for this section I try to emulate event 4625. This is because currently I'm using one machine/computer only. 

There's various way to emulate it, here are some of it:
1. Using Runas with Wrong Password
   - Open CMD or PowerShell
   - Run the following command on it
    `runas /user:fakeuser cmd`
![Desktop View](/assets/img/posts/2025-04-12-detecting-windows-logon-vents-4624-4625-with-elasticsearch-as-a-lightweight-self-managed-siem/runas-command.png){: width="800"}
   - When prompted, enter any password (wrong is fine).

2. Create a Dummy Local Account, Then Log in Wrong
   If it need to be more realism:
   - Create a dummy user using PowerShell(run as administrators):
   `net user admin P@ssword123 /add`
![Desktop View](/assets/img/posts/2025-04-12-detecting-windows-logon-vents-4624-4625-with-elasticsearch-as-a-lightweight-self-managed-siem/net-user-command.png){: width="900"}

   - Now try logging in with this account using the wrong password:
     - Lock screen (`Win + L`) → switch user → admin
   - Try logging in with wrong credentials:
     - Use something like admin123 as the username and any wrong password.
   - Delete the account after testing:
     - `net user admin /delete ` 

## What Is It Looks Like Inside Discover in Kibana?
And with that being done, we then can proceed to monitor the activity using some of the queries. Here's some explame of it:

1. All successful logins
```kql
   event.code: "4624"
```
![Desktop View](/assets/img/posts/2025-04-12-detecting-windows-logon-vents-4624-4625-with-elasticsearch-as-a-lightweight-self-managed-siem/event-4624.png){: width="800"}
2. Failed logins via local
```kql
   event.code: "4625" AND winlog.event_data.LogonType: "2"
```
![Desktop View](/assets/img/posts/2025-04-12-detecting-windows-logon-vents-4624-4625-with-elasticsearch-as-a-lightweight-self-managed-siem/event-4625-local.png){: width="800"}
3. Filter out noise from system accounts
```kql
   NOT TargetUserName: "SYSTEM" AND event.code: "4625"
```
![Desktop View](/assets/img/posts/2025-04-12-detecting-windows-logon-vents-4624-4625-with-elasticsearch-as-a-lightweight-self-managed-siem/filter-noise-4625.png){: width="800"}
4. Failed login attempts where the username starts with "admin"
```kql
   user.name : "admin" AND event.code: "4625"
```
![Desktop View](/assets/img/posts/2025-04-12-detecting-windows-logon-vents-4624-4625-with-elasticsearch-as-a-lightweight-self-managed-siem/failed-username-admin.png){: width="800"}
5. Failed logins from the last hour
```kql
   @timestamp >= "now-1h/h" AND event.code: "4625"
```
![Desktop View](/assets/img/posts/2025-04-12-detecting-windows-logon-vents-4624-4625-with-elasticsearch-as-a-lightweight-self-managed-siem/failed-login-one-hour-ago.png){: width="800"}

And that's some queries that I tried to shows the regarding events on this little experiment of mine.

## Insights & Lessons Learned

Here’s what I learned while building this setup:

- Elasticsearch is flexible: You can query just about anything if you understand the schema.

- Fields matter: Understanding LogonType, TargetUserName, and ProcessName can surface important patterns.

- Kibana makes learning fun: Seeing the data visually helped me internalize the concepts faster.

- Winlogbeat is efficient: It shipped the logs reliably with minimal config and resource usage.

- Also: filtering out logon attempts from Anonymous users or from NT AUTHORITY\SYSTEM helped clean up the data.

## What I Still Don’t Know (Yet)

This little experiment raised more questions that I’d like to explore:

- How can I set up alerts for excessive 4625s in a short time? (e.g., brute force detection)

- What is the best way to enrich logs with user info or location?

- Can I use Elastic Security (SIEM app in Kibana) to build detection rules easily?

- What about other logon-related event IDs like 4768 (Kerberos ticket request) or 4771 (pre-auth failure)?

## Conclusion: My First Step into Detection Engineering

This post helped me move from theoretical understanding to practical skill-building with Elasticsearch and Windows logs. I now better understand:

- How login events work on Windows

- How to query and filter them using KQL

- How to use Elasticsearch and Kibana as lightweight SIEM components

- Next, I want to explore Elastic Security, and maybe even try shipping Linux logs or network traffic into the stack. The possibilities are wide open.

If you're on the same journey or have tips for me, I’d love to hear them. What logs or events do you think I should dive into next?

Thanks for reading! If you found this helpful, check out my [first post] where I talk about the installation pains and how I made sense of the Elastic ecosystem as a beginner.