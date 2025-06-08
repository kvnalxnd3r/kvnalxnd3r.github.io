---
title: Brute Force Attack Using RDP
date: 2025-05-29 12:33 +0700
categories: [Attack Simulation, Brute Force]
tags: [elasticsearch, siem, windows, rdp, kali, brute force, cyber attack, endpoint, logs]
---

## About This Post

This post made because I thought "Okay, I got my tasked by my workplace to learn how to make **Detection Rules** and **Active Directory**. Maybe I could search something on the internet that connect these two?" And to my surprise there's already someone that did what exactly I thought. Shout out to [MyDFIR](https://www.youtube.com/@MyDFIR/featured) for the video that I used as my main basis and reference throughout this post.

So the project that I will do will mostly following along the MyDFIR tutorial that consist of five parts that you could check [here](https://www.youtube.com/playlist?list=PLG6KGSNK4PuBWmX9NykU0wnWamjxdKhDJ). You could also check his playlist on your own to see what kind of project that suit your need. But for me I'll just scroll down on that playlist and start on his **Active Directory** project.

![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/mydfir-active-directory-playlist.png){: width="800"}
_MyDFIR Active Directory Playlist_

His project is also serve for my further learning about **Brute Force**, **RDP**, **Kali Linux and its tools** and many more new things for me. So with all of these background being said, I'll continue to what context you should expect from this post.

## Context Before We Start

This section is to explain to you guys what you should expect from this post and that is the objective, purpose, and some disclaimer. Okay firstly the objective of this post is to **simulating an attacker brute-forcing using RDP to the target machine (Windows). And with the attack being launched, we want to simulate SIEM Detection of it using Elastic while also documenting the detection rule creation, what challenges when making this rule, and some basic case handling**. 

Second, the purpose for this post is to **journalizing my learning so that maybe others could learn one or two things from this post. Even if you don't learn something, well at least I hope you could be entertain seeing me being a complete noob**. Lastly, disclaimer. **This post by no means is perfect, so take it with a grain of salt and always check whether what I yapped is a mistake or not**. 

With all of that out of the way, lets start the learning.

## What We Will Be Using

The machines and tools and that we will be using is consisting of:
- Attacker machine: **Kali Linux**
- Target machine: **Windows 10 Pro with Active Domain and RDP enabled**
- SIEM: **ELK Stack**
- Brute force: **Hydra**
- Remote desktop: **xfreedp**
- Log forwarder: **Winlog and Sysmon**

The ingredients is different from what MyDFIR using but the concept and the goals basically the same. So you could also adjusting that list to your need. 

## Preparing Lab Diagram

So after we know what is the tools and machine that we will be using, we need to prepare it first before we start with our project. Firstly we need to have a clear diagram of this whole project. Think of it like a map or instruction that will makes us understand how to navigate and operating this project to meet our need.

![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/active-directory-lab.png){: width="800"}
_Project Diagram_

This diagram, I make it using [**draw.io**](https://app.diagrams.net/#) and from it you could see that I plan to use 4 machines in total, Kali Linux(for attacker), Windows 10 Pro and Windows Server 2022 Pro(I'll tell you the reason on the next section), Ubuntu Server Linux(for ELK stack server). For the IP itself, I'm using DHCP from prefix of my already set NAT network, why DHCP? Because I mostly see people on production stage use DHCP(but half of the reason is that I'm lazy to think of network).

Why we need internet here? Aren't we running it locally? Well yes, it still running locally but the internet is used for downloading for updating dependencies, downloading and communication between machines. The diagram now done, next we preparing the machines.

## Story Time And Kill Chain Diagram

Let's add story for this project for the funni, so two weeks ago, a junior IT admin proposing a resignation after being denied a promotion. But day before his official resigning, the SIEM begins logging dozens of failed RDP attempts from an internal IP toward the legacy Windows 10 remote access machine still exposed via port 3389. After several hours, a login is successful — and lateral movement attempts begin. No ransomware deployed… yet. 

Was this a random brute-force? A targeted attack? An insider handing off credentials? The SOC must act fast — reviewing RDP logs, building custom detection rules, and hunting traces before the attacker escalates privileges or exfiltrate data.

To further our understanding of this attack scenario, here's the kill chain diagram based on [**LOCKHEED MARTIN**](https://www.lockheedmartin.com/en-us/capabilities/cyber/cyber-kill-chain.html) cyber kill chain.
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/kill-chain-diagram.png){: width="850"}
_Kill Chain Diagram_

As you can see, the final objective for this lab project is to make adversaries gaining access for our data, so we need to act accordingly to this. Let's start the lab progress shall we?.

## Installing The VMs
> Remember on the lab diagram section I told that you need to use Windows Pro edition for all of the Windows VMs? That is because the Pro version enabling us to use RDP but the other Windows version won't. Don't ask me why but that is just how Microsoft is.
{: .prompt-info }

For this section I could refer you to follow my two parts blog, [part 1](/posts/elk-installation-with-two-vm/) [part 2](/posts/integrating-fleet-server-and-preparing-windows-vm/) to preparing our SIEM server and one of the endpoint/target machine. As for Kali Linux installation, you could follow MyDFIR [part 2](https://www.youtube.com/watch?v=2cEj3bS5C0Q&list=PLG6KGSNK4PuBWmX9NykU0wnWamjxdKhDJ&index=15&pp=iAQB) video at minute **8:58**, lastly for the Windows server VM at minute **12:31**.

> Don't forget to add NAT networks and port forwarding rules for our VMs able to communicate each other before configuring Windows server.
{: .prompt-info }
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/nat-network-configuration.png){: width="350"}

![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/port-forwarding-rules.png){: width="350"}

Well, apologize if you felt that I'm so lazy for just referring to that video(which is true) but mostly I felt that there's no need to 'reinventing wheel'. And after the installation complete, next we preparing the Windows Server which is on [part 4](https://www.youtube.com/watch?v=1XeDht_B-bA&list=PLG6KGSNK4PuBWmX9NykU0wnWamjxdKhDJ&index=17) just follow along his video but for the static IP configuration part just skip it because we using DHCP other than that just follow along the video and we will good to go for our Windows server.

>Oh while we're at it don't forget to also add Elastic agent and Sysmon on Windows server VM too. The how to do it, you can see it on my [part 2](/posts/integrating-fleet-server-and-preparing-windows-vm/) post. This optional but I feel the need to do this to simulate real life scenario.
{: .prompt-info }

**We skipping part 3** of MyDFIR video because **he using Splunk** and we don't do that here but if you wish to use Wazuh or any other SIEM feel free to use it. The basic remains the same. And with all of it done, we can move to the fun part part, attacking the Endpoint.

## Adversaries Commencing The Attack

Now at this point we have all the VMs and scenario set in stone, what is left for us is to commence the attacking scenario. And the first phase of the attack is the **Recon** phase. 

### Recon
On the kill chain diagram it stated that the attacker start by scanning the network IP. How's the attacker the network IP of that said active directory? It's suspected there's an insider informant for the attacker and the fact that the adversaries already inside the internal network really strengthening **the suspicious insider among us**(I'm sorry for that reference, but I had to).

Because the attacker already infiltrating the network and got information of the network IP that being for the active directory, he/she then begin to scan that said IP. Here's what scanning the IPs would look like from the attacker perspective.
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/attacker-scanning-available-ip-network.png){: width="800"}
_Attacker Scanning IP Network_
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/attacker-got information-about-available-ips.png){: width="800"}
_Attacker Got List of Available IP_
As you can see, the tool that being used is [**netdiscover**](https://www.kali.org/tools/netdiscover/). This tools allowing the attacker to got clear vision of what is inside the network. The next step is for the attacker to scan what is the port that available on those IPs.

![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/scanning-active-port-of-the-ips.png){: width="800"}
_Attacker Scanning For Open Ports_

Now the attacker start scanning what port that available on that IP range using [**nmap**](https://www.kali.org/tools/nmap/) and would you look at that, he already got got so much information. The attacker knows the network got port **3389**, **139**, **445**, **135**, and **22**. A quick google search will tell you that:
- Port 3389: used for remote connection(RDP).
- Port 139: used for file sharing, printers, and network for Windows-based system over NetBIOS, basically for older version for Windows version or Unix version.
- Port 445: used for file sharing, printers, and network over TCP/IP and more of a modern version.
- Port 135: used for allowing clients and server communicate to remote access and management. Or in other word, implement RPC Mapper Service in Windows environment.
- Port 22: used for SSH, a secure remote login and command execution.

As you can see, so much information gained by the attacker, and by now the attacker knows what IP to attack and that is ``192.168.68.2`` and ``192.168.68.7`` because the RDP port is available on it. So now the attacker resume to the next phase.


### Weaponization
This is where this blog and [part 5](https://www.youtube.com/watch?v=orq-OPIdV9M&t=557s) video of MyDFIR differ, if MyDFIR using template word lists while I will be using my own word list because the scenario here the adversaries got information about what is the username and password but not know exactly what is the password. That is why the attacker prepping a list of possible password from the clue given.

So here's how the attacker will be preparing the word list.
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/make-passwords-list.png){: width="800"}
_Attacker Make Password Guess List_
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/inserting-passwords-guess.png){: width="800"}
_Attacker Inserting Possible Password Guess_

First, attacker will make password list consisting possible password guess and combination. Attacker knows this because receiving information that one of the account on the active directory domain is username ``mrizky`` and the password is something along ``mrizkyad`` abbreviation of **mrizky active directory**. The attacker also knows that the password is using capital letter and number combination that is why you see the password list having bunch of numbers and capital letter. As for the number of password possibilities, the attacker using 20+ possibilities of guess.

Now the attacker ready for the next phase of it's attack and that is brute forcing their way inside the target machine.

### Delivery And Exploitation
I just combine this two part because the progress is continuous of each other, what I mean by that is we will be using [**Hydra**](https://www.kali.org/tools/hydra/) to brute force and guessing the password from the list and **Hydra** will shows us what is the correct answer. In MyDFIR video he's using **Crowbar** but for me I failed when using it and find success when using **Hydra**. So lets start the brute force.

We know that there's two target here, ``192.168.68.2`` and ``192.168.68.7`` so we will be attacking those two. Starting with ``2``.

![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/executing-brute-force-on-68-2.png){: width="800"}
_Attacking 192.168.68.2_

Attack that launched to ``192.168.68.2`` using command ``sudo hydra -l mrizky -P password.txt -t 4 rdp://192.168.68.2``is failed, this caused by either the **firewall blocking the inbound traffic**, **RDP is not enabled**, or anything regarding security policy inside this machine. The command consisting of:
- ``-l`` flag, this flag telling hydra to use username that given to it specifically.
- ``-P`` flag, this flag telling hydra to use list in form of a file in this case its **password.txt**.
- ``-t`` flag, this flag telling hydra to tried four times for each attempt of brute force.
- ``rdp://IP_ADDRESS`` flag, this flag telling hydra to use RDP targeted to the IP being given.
More about available flag of Hydra can be read on [official page of it](https://www.kali.org/tools/hydra/).

 Next we'll attack ``192.168.68.7``.

![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/executing-brute-force-on-68-7.png){: width="800"}
_Attacking 192.168.68.7_

Now it's a success! Let me attempt to explain a bit what happen here in this succeed attempt. The line where **[3389][rdp]......** is where Hydra attacking the port that attached on that IP using one of the password that given in the list ``mrizky4d``. And it failed but the next line it says the connection is failed right? That is either because the **connection is bad** or **RDP rejecting another connection attempt, RDP is really sensitive about this by default**. 

And Because the Hydra on the background running through the list checking all of the possible password is why the next line we got success notification in colored highlight. The password is **mR1zKyaD**, this is why I combined delivery and exploitation because the it can be executed simultaneously. Next let's see the attacker using **xfreedp** to remotely entering the machine.

### Installation
All the important information is acquired, next that left is for attacker entering the machine and let's see how it is done.
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/attacker-establish-rdp-connection.png){: width="800"}
_RDP Connection Established_  

From the picture you can see the attacker using [**xfreerdp3**](https://www.kali.org/tools/freerdp3/) tool and the command is ``sudo xfreerdp3 /v:192.168.68.7 /u:mrizky /p:mR1zKyaD``.
- ``/v`` flag is signalling to xfreerdp3 to attempting connection to the given IP.
- ``/u`` flag is signalling to xfreerdp3 to use username being given.
- ``/p`` flag is signalling to xfreerdp3 to use password being given.

And from the look of it, the attacker now successfully connected and being given prompt to end other user that currently logged in as you can see from the picture below.
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/user-allowing-rdp-connection.png){: width="800"}
_RDP Connection Established_  

From the picture let's just assume that whoever currently using the machine thinking that maybe this is from IT department wanted to log in, so he/she just accepting the connection. When the current user accepting it, here's what it looked like on the attacker side.
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/accepted-connection.png){: width="800"}
_Attacker Connected To The Machine_ 

After the connection is established, the attacker finally inside the machine.
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/attacker-entering-the-machine.png){: width="800"}
_Attacker Inside of The Machine_

At this stage, the victim is at attacker mercy, because from this point on attacker successfully infiltrating the system and can do whatever possible.

### C2 With Actions On Objectives
Again, I combining this 2 phases because to my understanding this two phases as I practice it, it correlate with each other. Why? Because when attacker dropping the backdoor it also mean that accessing data is available and any other feature inside the machine. Let us see from the attacker perspective.

![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/making-shell-script-with-bat-format.png){: width="800"}
_Attacker Dropping Backdoor Script_
The attacker making **``bat``** file in the ``C:\Users\<victim>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup`` with script ``powershell -windowstyle hidden -command "Start-Sleep 5; Start-Process calc"``. The script basically will automatically open calculator when user **``mrizky``** login to the machine.
> This script will only running when user mrizky logged in to the machine. If you use other user it will not running.
{: .prompt-tip }

![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/script-executed-when-logged-in.png){: width="800"}
_The Script Executed When Login_
You guys see the script got executed right? This will opening calculator application and I try to simulate the attacker attempting another login using ``xfreerdp`` and the script successfully executed. This is just one simple example of an attacker planting some script. The script I shows in this project is just a harmless one that opening calculator(this is to match my skill that is just a noob in cyber security field) but imagine if the attacker is more skilled? How destructive could it be eh? 

### Final Words About The Attack
With it the attack is done now. Quite fantastic and mind blowing I'd say to be able done this project because with this project I could picturing how is the attacker mindset actually got in action. But how about from the defender side?(Blue Team) I'll show you guys how's from the blue team side would look like.

## Defender Defending The System

From the defender side we will see how them preventing, securing, monitoring, and maintaining the system from the attacker. From my understanding I would like to show you guys how can defender preventing and defending from some of the key phase from the attacker so that we could securing the the system. Without further ado, let's get into it.

As I'm using Elastic as SIEM, we will take a look how Elastic work, and better way to showcasing it by showing the dashboard that using queries to will help us taking information about event, logs, and other process on our endpoint machines. Here's some of the queries that will help us achieving that.
- ``event.category : "network" and destination.port : * and network.transport : "tcp"`` for querying TCP activities. This query would help us scanning the activities of network that using TCP protocol. Why TCP? Because from [**nmap**](https://nmap.org/book/toc.html) documentation they usually using TCP for their traffic, so I my guess is that we will focusing on the TCP. I also basing this query that I make from [**this rule**](https://www.elastic.co/docs/reference/security/prebuilt-rules/rules/network/discovery_potential_port_scan_detected) from Elastic.
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/queries-for-network-and-port-scanning.png){: width="800"}
_TCP Connection Activity_

- ``event.code : 4625 AND winlog.event_data.LogonType : ("3" or "10")`` for querying failed remote connection attempt. I make this query because from the [**Windows official documentation**](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4625) about log on type ``4625``, it is a failed log and the type for remote connection is ``3`` and ``10``. So I just using the type with operator OR so that it could catch both or one of it.
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/queries-for-brute-force-attempt.png){: width="800"}
_Failed Remote Login Attempt_

- ``event.code : 4624 AND winlog.event_data.LogonType: ("3" or "10)`` for querying successful remote connection. From the [**documentation**](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4624) other than different ID number and this time its successful login, it's basically just the same with failed log on attempt query.
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/queries-for-external-remote-connection.png){: width="800"}
_Successful Remote Connection_

- ``file.extension : ("bat" OR "ps1" OR "lnk")`` for querying script powershell execution. For this query, I just go simple and straight specifying the type of the file. I mean it's not everyday someone would executing ``bat`` file or ``ps1``(PowerShell) file right? So we could just heavily suspecting it to be suspicious activities.
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/queries-for-shell-script-execution.png){: width="800"}
_Executed PowerShell Script_

With all of those query that I could think of, this is some of the steps that I come up with as **Junior SOC Analyst** would do, is to first as you can see testing whether the query is getting hit and from all of the picture above it is proven we got hit. So next thing to do for me is to make **Rules** for those queries.

We go to **Security** tab inside the **hamburger menu**, next click on **Alerts** menu and then go to **Rules** page. We then would arrive in the dashboard of **Rules** where we can create the rules that we wanted.

![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/rules-dashboard.png){: width="800"}
_Rules Dashboard Page_

From that dashboard you can see that I already make one of the rules, but don't worry I'll show you how to do it and first click on **Create new rule**, we will then presented the options and setting for us to make the rules. I will show you how to make first rules, start with the prevention of **Reconnaissance** phase of the attacker and that is for detecting network and port scanning attempt based on the information that we got. Just follow along the picture that I show you guys below.

- First it will asking you guys to inserting the query, just insert it like this picture.
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/rules-queries-for-network-and-port-scanning.png){: width="800"}
_Rules Query Inserted_

- Next, we will filling in information about what this rule would help us detecting attack type based on [**MITRE ATT&CK**](https://attack.mitre.org/) framework. For this I'm referring this rule with [**Discovery**](https://attack.mitre.org/tactics/TA0007) technique attack and also [**Reconnaissance**](https://attack.mitre.org/tactics/TA0043) technique attack because when I read the definition, the description matches with how the attack being executed and operated.  To make this rule more specific I also specifying the sub-technique attack of it to [**Network Service Scanning/Network Service Discovery**](https://attack.mitre.org/techniques/T1046), [**Active Scanning**](https://attack.mitre.org/techniques/T1595/) - [**Scanning IP Blocks**](https://attack.mitre.org/techniques/T1595/001/).

For the severity level I set it to low because sometimes this rule detecting [**false positive**](https://csrc.nist.gov/glossary/term/false_positive) and to be honest, I'll learn more about queries in the near future but for my current skill this is what I could think of.
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/information-about-network-and-port-scanning.png){: width="800"}
_Specifying Rule Information_

- We already got the information, now what we wanted is to set the schedule of how often this rule will be applied and running. For this I highly suggest just got with whatever ideal for you and for me it will be running every ``20m`` with additional check between ``1h``.
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/rules-schedule-for-network-and-port-scanning.png){: width="800"}
_Defining Rule Schedule_

- Final step is to specify what kind of alert application or method that we want to set this rule for help notifying team about our finding using this rule. For me, because I'm in lab environment I'll just go with no actions. But in real life scenario you could choose from **Microsoft Teams**, **Slack**, etc.
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/rules-action-for-network-and-port-scanning.png){: width="800"}
_Rule Action Notification_

And that's it, the rule has been created, next thing left is to make rules for the rest queries. I'll just showcasing the important part of it because it's basically same procedures as our first rule.

- For detecting brute force attempt rule.
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/rules-queries-for-brute-attempt.png){: width="800"}
_Rule Queries Brute Force Attempt_
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/information-about-brute-force-attempt-rule.png){: width="800"}
_Rule Information Brute Force Attempt_

- For detecting external remote access rule.
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/rules-queries-for-external-remote-connection.png){: width="800"}
_Rule Queries External Remote Connection_
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/information-about-external-remote-connection.png){: width="800"}
_Rules Information External Remote Connection_

- For shell script execution rule.
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/rules-queries-for-shell-script-execution.png){: width="800"}
_Rule Queries Shell Script Execution_
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/information-about-shell-script-execution.png){: width="800"}
_Rule Information Shell Script Execution_

And with that, we got ourselves custom rule that matches our condition and need. Let's see if this rule working as intended or not. To see the result, just click on one of the rule that we make previously in rule dashboard page and Scroll down a bit to see if there's any result we get. I'll start with brute force attempt rule because for me this where the attacker start being aggressive.

![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/brute-force-attempt-rule-result.png){: width="800"}
_Brute Force Attempt Rule Result_

To see the result just click on **View details** icon and we will be presented with this menu on the right side of the screen.

![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/brute-force-rule-result-detail.png){: width="800"}
_Taking Action For Rule Result_

Just click on the **Take action** button and select **Add to new case**. Next is shown on picture below.

![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/new-case-name-and-desrciption.png){: width="800"}
_Filling New Case Information_
![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/new-case-created.png){: width="800"}
_Making New Case_

After we click add new case, we will then have to inserting the case name and description to better explain what is wrong with this case. Next we will see whether the case has been created or not. To do that, head to **Cases** menu page by clicking the tab on the left and we will see this pages.

![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/cases-dashboard-page.png){: width="800"}
_Cases List_

Our new cases successfully added now we can follow its development as other team will be notified by clicking on it(External Remote Connection).

![Desktop View](assets/img/posts/2025-05-29-brute-force-attack-using-rdp/external-remote-connection-case.png){: width="800"}
_Case Management_

Because I'm just L1 SOC this is as far as I can do for my current skill and knowledge and with that we just have to wait other team confirming our finding.

## Final Words

Thank you so much for following this blog and I hope you guys could learn one or two things from it but if it's not then at very least I hope I could be entertaining enough with this. Once again, thank you so much and also thank you MyDFIR for inspiring me doing this project.

## References

| References                                                                                                                                                                                         |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| MyDFIR Playlist. [MyDFIR Active Directory Playlist](https://www.youtube.com/playlist?list=PLG6KGSNK4PuBWmX9NykU0wnWamjxdKhDJ)                                                                      |
| LOCKHEED MARTIN Cyber Kill Chain. [Cyber Kill Chain](https://www.lockheedmartin.com/en-us/capabilities/cyber/cyber-kill-chain.html)                                                                |
| Kali Netdiscover. [Netdiscover](https://www.kali.org/tools/netdiscover/)                                                                                                                           |
| Kali Nmap. [Nmap](https://www.kali.org/tools/nmap/)                                                                                                                                                |
| Kali Hydra. [Hydra](https://www.kali.org/tools/hydra/)                                                                                                                                             |
| Kali Xfreerdp3. [Xfreerdp3](https://www.kali.org/tools/freerdp3/)                                                                                                                                  |
| Elastic Port Scan Rule. [Potential Port Scan Detected](https://www.elastic.co/docs/reference/security/prebuilt-rules/rules/network/discovery_potential_port_scan_detected)                         |
| Windows Failed Log on Event. [4625(F): An account failed to log on.](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4625) |
| MITRE ATT&CK Discovery Technique. [Discovery.](https://attack.mitre.org/tactics/TA0007/)                                                                                                           |
| MITRE ATT&CK Reconnaissance Technique. [Reconnaissance.](https://attack.mitre.org/tactics/TA0043/)                                                                                                 |
| MITRE ATT&CK Network Service Discovery Technique. [Network Service Discovery.](https://attack.mitre.org/techniques/T1046/)                                                                         |
| MITRE ATT&CK Active Scanning Technique. [Active Scanning.](https://attack.mitre.org/techniques/T1595/)                                                                                             |
| MITRE ATT&CK Scanning IP Blocks Technique. [ Active Scanning: Scanning IP Blocks .](https://attack.mitre.org/techniques/T1595/001/)                                                                |
| False Positive Definition. [False Positive.](https://csrc.nist.gov/glossary/term/false_positive)                                                                                                   |

