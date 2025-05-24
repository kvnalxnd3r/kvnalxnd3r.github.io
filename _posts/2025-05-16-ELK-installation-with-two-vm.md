---
title: ELK Installation With Two VMs For Home Lab
date: 2025-05-20 13:21 +0700
categories: [Tutorial, SIEM, ELK Stack]
tags: [elasticsearch, siem, windows, linux, virtualbox]
---

## Before We Start

This section is a explaination for what to expect in this blog and also serve as a shoutouts to all of the peoples that making knowledge in form of tutorials. With that being said, this is some of the people that helps me on this installation:
- [crin](https://www.youtube.com/@basedtutorials/videos), thank you for making discord server where newbie like me and others could gather and share knowledge.
- nubbieeee, thank you so much for reaching out to me on discord. Really appreciate your guide, with it  I'm able to present my progress.
-  [Will Nissler](https://www.linkedin.com/in/william-nissler-770583188/) from [LevelEffect](https://www.leveleffect.com/), thank you for making the tutorial that I could use as a main guideline alongside with nubb tutorial. Your tutorial really have a lot of information.
-  And other tutorial that I will list on the reference that helps me fix certain error and circumstances that I experience along my installation and setup progress.

## What To Expect From This Guide

This guide is served as a supplementary and a note for present and future me or maybe others that will or want to start installing Elastic for home lab where it could be used as tools of experimenting and hands on learning of cyber security. By no means this guide is perfect and if you happen to encounter any error, use the material that I listed on the refence section or like the old saying **Just google it**. Because at the end of the day, cyber security is heavily relied on the so called **professional googling**. And don't forget to ask ChatGPT, it really helps to direct you to more accurate answer.

I'm planning to make this guide into two parts or maybe more depending on my current assignment given to me. But for the time being it will be just two part. For this part one, I will focusing on installation and setting up the ELK stack and all of the VMs.

## Some Stuff That You Will Learn

There are some stuff here that I sure you will learn one or two when following this guide, some of it are:
- Setting up and configuring VM with VirtualBox.
- A bit of networking stuff such as port forwarding, networking.
- ELK stack installation and understand how they work.

And maybe many more that I couldn't mention above. Hope this guide become useful for me in the future or for you guys too.

## Objective of This Guide

Okay this will be the last I'll yap other than the actual guide. So here's some of the objective for part one of this guide series:
- Setting up all of the VMs (i.e. Linux and Windows) to suit the requirement.
- Setting up VMs connection configuration. 
- Installing and configure Elasticsearch on Ubuntu server VM.
- Installing and configure Kibana on Ubuntu server VM.
- Installing and configure Filebeat on Windows VM.
- Successfully login to ELK stack after the installation.

## Let's Begin

Now in this section, we will begin the installation progress by first understanding what is ELK stack and then understand what this lab looks like on a diagram to further enhance our understanding of this lab. First thing first, what exactly is ELK stack? I'll add my 2 cents here too since the [official documentation](https://www.elastic.co/elastic-stack) already done such great detail in explaining it. Basically ELK is an acronym, Elasticsearch Logstash Kibana and each of that technology is serve their on purposes. 

Elasticsearch is to search, analyze, and process all of the data that being collected. You could call it the engine of ELK stack and Logstash is the instrument that ingest all of that data from multiple sources to Elasticsearch and when the data arrived in Elasticsearch, it is then can be visualize by Kibana for making user more easy to understand all of those data in form of charts and graphs.

### Diagram

And with that explanation in mind, we then could utilize ELK stack as a SIEM to monitor our system. For the system of this lab, I will explain it with the diagram below to better understand the context of what it looks like:

![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/siem-diagram.png){: width="500"}
_Here's a simple diagram to illustrate our lab_

I admit, it's not fancy but for me it could convey what our lab will look like and this is as far as my understanding of the lab. But if you want a more detailed overview of the diagram, head to [William Nissler guide](https://www.leveleffect.com/blog/how-to-set-up-your-own-home-lab-with-elk) he got to more detailed about it. So from that diagram, we want to install ELK stack on the ubuntu server with the listed ports and that port will be attached to the DHCP IP.

And the ELK stack will receiving log mainly from the Windows VM through sysmon, filebeat, windows event log, and powershell script. And also the Windows VM having the IP received from the DHCP process that can be used to make connection between two VMs. Lastly, the Windows analysis machine is our own PC that act as a monitoring device that can access the ELK stack with the localhost ip. But why ***localhost*** you say? To put it simply, we can't just access the VMs directly because the nature of VM itself.

VM is run on their own separate kernel despite it's still in our machine and that also mean they having their own network too, that is why in the diagram I put router to illustrate this process as router is the device that handling layer 3 i.e. network(IP, etc). And to access that network we have to set the NAT and some ports too if we have to use some protocol. We will go through this more in depth in the next section. But for now, just know that we can't just access the VMs directly.

## Linux Ubuntu Server VM Installation

In this section we will begin first with installing Ubuntu Server for our ELK stack. The Ubuntu Server version that I and some of the guides that I used is 18.04 LTS, the [LevelEffect guide](https://www.leveleffect.com/blog/how-to-set-up-your-own-home-lab-with-elk) didn't mention any reason as to why choosing this version but seeing the time of the blog being published was in 2022, my guess is version 18.04 is more stable than any version during that time. 

And if we done choosing the version, here's some requirement for the Ubuntu VM:

| Hardware Requirement |
| :------------------- | :------------------ |
| OS                   | Ubuntu Server 18.04 |
| RAM                  | 4 GB                |
| CPU                  | 2 Cores             |
| Storage              | 30 GB               |

I go with the requirement above because it suits my need and my hardware capability but if guys want to tweak some of the requirement then go ahead. With that being said, the steps for installing Ubuntu server is as follows:
1. Download it from the [official Ubuntu server website](https://releases.ubuntu.com/18.04.6/ubuntu-18.04.6-live-server-amd64.iso).
2. After done downloading it, we then open VirtualBox and click **Machine** menu and select __New...__.
![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/new-machine.png){: width="800"}
_Selecting Machine To Create New VM_
3. It will then having **Create Virtual Machine** pop up, with it we could configure and set the properties of the VM. Just put the machine hardware just like our hardware requirement previously. One thing to note here is that, I checked the **Skip Unattended Installation** because if we want to use unattended installation that mean VirtualBox will handles the installation based on the provided setting and in this case we want to configure it manually.
![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/create-virtual-machine-menu.png){: width="700"}
_Configuring VM Hardware_
4. Next, the VM will appear on the VirtualBox menu manager, select that and click on **Start** icon to start the VM installation.
5. The VM starting and next thing is to select language, select whatever language that suits you but in this case I choose english for obvious reason.
6. The language set is done, now we will greeted by Ubuntu Server version notification. For our case, just select **Continue without updating**
![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/ubuntu-version-update-notification.png){: width="800"}
_Skipping Version Update Selection_
7. Here, there will be IP that we got from DHCP, note it because this is important for basically every next configuration.
![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/receiving-dhcp-ip-from-nat-network.png){: width="800"}
_Receiving IP DHCP_
8. If being asked to choose proxy on the next step, just skip it because we don't need it.
9. Also just go with the default mirror link. Basically this is just Ubuntu asking for sources to download its dependencies and packages.
10. The next menu is storage partition. For me personally I'll just go with the default but feel free if you guys want to get creative with it.
11. Okay, next is configuring username and password for the Ubuntu. Remember also to make password that easy to remember but also not too easy to guess(it will be used a lot in the next steps). For this section, I'll configure it like shown below but then again, feel free just go with something you guys want.
![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/ubuntu-username-and-password.png){: width="800"}
_Creating Account For Ubuntu_
12. The account has made, we then continue to enabling SSH for this Ubuntu.
![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/enabling-ssh.png){: width="800"}
_Selecting Enable SSH_
13. All of that has been done then we wait for the installation to finish. It will somewhat look like this or similar.
![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/first-look-of-linux-vbox.png){: width="800"}
_Ubuntu Server Successfully Installed_

Now with the installation complete I'll add one more important thing to do before we proceed further. **I want you guys make snapshot for each section of this guide**. Yep, this will serve as a failsafe incase something went wrong, making it "point of return" for us(yes pun intended). Here's how to do it:
- Click on **Machine** menu again and select **Take Snapshot...**
![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/making-first-snapshot.png){: width="800"}
_Selecting Take Snapshot_
- After that there will be menu pop that want us to name the snapshot. The name will function as a description for what exactly is this snapshot that we make. So make sure the name and description is informative about what machine situation when the snapshot being made. 
![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/finish-apt-update.png){: width="800"}
_Making Snapshot Name and Description_

And with that our Ubuntu Server is done we could continue to next step, that is updating our Ubuntu with this commands. But we need to login to our Ubuntu. After done login, execute these steps:

#### Disclaimer for sudo Command
From here on we will using **A LOT** really a lot ``sudo`` and it will often asking to enter our root password(same with your user password) that is why I said to use easy to remember password previously. Basically we want to minimize access to our root as little as possible because security concern(shoutout to nubb, thanks for the insight). Okay, on with the guide.

1. Enter command ```sudo apt update && sudo apt upgrade -y```. These commands will update and upgrading our existing packages.
2. Next command ```sudo apt dist-upgrade -y```. Same with previous but the difference is, this command will handle changes of the packages if necessary.
3. ```sudo apt install zip unzip -y``` is the next command, this command installing zip and unzip application for us to be able extracting zip format file and compressing file into it.
4. Lastly there's ```sudo apt install jq -y``` that used for JSON parsing and transformation. It's just a fancy words that means our Ubuntu able to interact with JSON format file.

With all of that being done, our Ubuntu is ready. Oh, and don't forget to make snapshot here too just to be extra careful if you guys want.

## Accessing Machine Through SSH

### Checking Ubuntu SSH
This section will focusing on SSH thingies, starting from checking it's status, setting up NAT rules, and many more. I recommend using SSH to simulate remote connection within work environment and to avoid hassle of interacting directly with the VirtualBox(yes, it's such a pain in the ass). But first thing first, login to the Ubuntu VM and execute this steps:

1. First, we have to check our Ubuntu SSH status. There's some possibilities here, either our SSH is working or not. Let's make sure of it with this command, ```sudo systemctl status ssh```. On my case, the SSH is **Active and running** like shown below.
![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/checking-SSH-status.png){: width="800"}
_Making Snapshot Name and Description_

This is because we **enabling SSH** previously during our installation but in case if some of you guys seeing **failed or dead status** proceed to next steps. 
2. Execute this command ```sudo apt-get remove --purge openssh-server && sudo apt-get update && sudo apt-get install openssh-server```. That commands(I use plural here because there's two command that being fused together with `&&` operator) basically the commands will first remove and deleting all of the SSH files, service, etc and then installing it again.
3. When it is done, you guys could run ```sudo systemctl status ssh``` and you should see a dead status but don't worry, execute ```sudo systemctl enable ssh``` to enable the service and then ```systemctl start ssh``` to start it again. And now next check it's status again, it should be running now. Don't forget to make snapshot 📷 .

### Network And SSH Configuration

Even with all of that progress previously, we still couldn't even access the VM directly by their IP, nuh uh that would be too convenient. Welcome to IT world, where if it's not pain in the ass you should suspect somethings wrong. So how do we SSH the VM then? well let's get into it:

1. First head to our VirtualBox Manager menu and select the network option shown below.
![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/selecting-network-menu-on-virtualbox-manager-menu.png){: width="800"}
_Selecting Network Menu_
2. After that there will be network menu, in it select **NAT Network** tab and click on **Create** icon to create new NAT Networks option.
![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/setting-ssh-forwarding-rules.png){: width="800"}
_Configuring NAT Network_
From the image above, we basically configuring our NAT Network to named "ElasticNetwork" to the network IP within prefix of 192.168.68.0/24 with DHCP server enabled. Below it, we could see there's **Port Forwaring** tab where you could add by clicking on the green plus icon and set it's name, the protocol which is TCP for SSH, host IP being left blank because we want it to be able receiving any IP other from the prefix range, the host port, guest IP with our own Ubuntu IP previously, and lastly the guest port being 22.

3. Next is to set our network mode for our Ubuntu VM, again we head to ``Machine`` > ``Settings`` on our VM windows and after that select **Network** like shown below.
![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/selecting-network-mode.png){: width="800"}
_Setting Ubuntu Network Configuration_
Change the **Attached to** with **NAT Network**, name change it with the network that we make, and lastly I set the **Promiscous Mode** to **Allow All**. That make sure it could capture all traffic because well, this is a SIEM lab.

![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/ssh-to-vm.png){: width="800"}
_Successfully SSH To Ubuntu VM_

Now all of that done, finally we are able to SSH to the VM and maybe some of you wondering why the IP is ``127.0.0.1`` and not ``192.168.68.5``? Well according to the guides that I read, this is possible because the port forwarding rules that we set before that allowing all IP to access SSH to Ubuntu VM through ``port 2222`` which then being redirected to ``port 22``. And ``127.0.0.1`` itself is another way of saying **localhost**. I've tried to access it with IP ``192.168.68.5`` but there's no connection, I still dunno why is that happen even though I've tried to add rules for it`¯\_(ツ)_/¯`. But oh well, maybe I'm just that _noob_. Oh and don't forget to make snapshot 📷.

## Setting Up Elasticsearch

Okay, now the nitty gritty part of it, buckle up and prepare to get your hands dirty because there will be a lot to configure. Oh and we will continue this part on SSH connection.

### Downloading and Installing Elasticsearch

But before we start downloading Elasticsearch, execute this command ``sudo apt install apt-transport-https -y`` to enabling us using APT transport for the HTTPS protocol which is a more secure version of HTTP and for the GPG key of the Elasticsearch. Again, some security stuffs and best practice.

Okay, below is screenshot and steps for Elasticsearch installation:

![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/downloading-elasticsearch.png){: width="800"}
_Successfully Downloading Elasticsearch_

1. First we download GPG key for the Elasticsearch with command ``wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -``
2. Next, to see if the repo is exist for our Ubuntu ``echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list``.
3. Now, with all of that done we could update and install Elasticsearch ``sudo apt update -y && sudo apt install elasticsearch -y``

Done and with that we could see the status of Elasticsearch service just like SSH previously with command ``sudo systemctl status elasticsearch.service``.
![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/elastic-systemctl-status.png){: width="800"}
_Checking Elasticsearch Service Status_
As you can see, the status is dead and that is because still haven't enabling the service yet, that will be done on the next section.

### Configuring Elasticsearch And Checking It's Connection

Before we enabling and running the service, let's configure Elasticsearch first to suit our need like changing it's address, protocols, ports, etc. 

We could do that by entering command ``sudo nano /etc/elasticsearch/elasticsearch.yml``. Let me explain a bit that command, basically we accessing elasticsearch.yml with super user privilege, then choosing our text editor for mine it's **nano** but you could choose other text editor, and lastly we specifying the filepath of elasticsearch.yml to be able to open it.

After that done, you should see the text editor open up and you could start changing the properties such as shown below:
![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/changing-elasticsearchyml.png){: width="800"}

![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/changing-elasticsearchyml-network-and-discovery-type.png){: width="800"}
_Changing Elasticsearch Properties_
What we're changing here is:
1. ``cluster.name`` to our own wanted name.
2. ``network.host`` to our own IP.
3. ``http.port`` is just commented out because this will be default value of 9200.
4. ``discovery.type`` is the last. This is for Elasticsearch to know whether we run a single node or multiple. More of it can be read [here](https://www.elastic.co/docs/reference/elasticsearch/configuration-reference/discovery-cluster-formation-settings)

With this we then could do these following steps:

![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/testing-elasticsearch-connection.png){: width="800"}
_Enabling Elasticsearch Service And Testing It's Connection_

1. We first enable the service with ``sudo systemctl enable elasticsearch``.
2. Then we start it ``sudo systemctl start elasticsearch``.
3. After that, checking its status ``sudo systemctl status elasticsearch``.
4. Finally we check the connection with ``curl --get http://YOUR_OWN_IP:9200``. Change ``YOUR_OWN_IP`` with the IP you got from DHCP previously. 

## Setting Up Kibana

Now, we do the same with Kibana, take a look this screenshot below.

![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/install-and-configure-kibanayml.png){: width="800"}
_Successfully Installing Kibana_

Enter command ``sudo apt install kibana -y`` and wait for it to finish the process. After that we edit the Kibana YML configuration, enter command ``sudo nano /etc/kibana/kibana.yml`` disclaimer again, **nano** is my text editor choice.

![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/set-kibanayml-properties.png){: width="800"}
_Changing Kibana YML Properties_

In here, we change:
1. ``server.port``, we just commented it out because we will be using it's default properties.
2. ``server.host``, change it again to your own IP.
3. ``server.name``, change it to your desire.

![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/change-kibanayml-authentication-cred.png){: width="800"}
_Kibana Credentials_

As for Kibana credentials, we just leave it for now. We will back to it in the later steps. And with that, let's continue next step that is enabling Kibana service.

![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/enabling-and-starting-kibana-services.png){: width="800"}
_Enabling Kibana Service_

1. We always start with enabling service first, ``sudo systemctl enable kibana``.
2. Then start it's service ``sudo systemctl start kibana``.
3. And finally checking it's status with ``sudo systemctl status kibana``.

And that's it, we finally installed Elasticsearch and Kibana. You could also checking Elasticsearch and Kibana status both at the same time with ``sudo systemctl status elasticsearch && sudo systemctl status kibana``. And as always don't forget to take snapshot 📷.

## Important Note

This section is for those who are done for the day of configuring this lab and want to take a rest or any other task. If you want to stop, you have to shutdown all of the component properly, here's how you do it.

![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/correct-way-of-stopping-services.png){: width="800"}
_Shutting Down System Properly_

The command used is in order, so first it's ``sudo systemctl stop kibana`` and the second is ``sudo systemctl stop elasticsearch``. This way we could safely shut down our system without worrying the risk to break anything. And another note, ``kibana.service`` == ``kibana`` && ``elasticsearch.service`` == ``elastic``.

And if you guys want to continue again:
1. sudo systemctl start elasticsearch
2. sudo systemctl start kibana

## Setting Up Filebeat

Same thing with previous installation steps, enter ``sudo apt install filebeat -y``, wait for the process to be done and edit the YML file ``sudo nano /etc/filebeat/filebeat.yml``.

Inside ``filebeat.yml`` file we could adjust and change variety of input received by Elasticsearch from ``journald``, ``winlog``,  Windows event logs, and many more. Winlog helps us captures more log data beyond just logged events. The guides that I follow mostly using Filebeat to collect it's log, so let's just focus on that too.

What you have to edit in the ``filebeat.yml`` is as follows

![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/change-outputelastic-properties.png){: width="800"}
_Changing Filebeat Hosts_
We change that to our own IP that attached to Elasticsearch port 9200. And the next thing to is shown in screenshot below.

![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/disabling-logstash-and-testing-filebeat-connection.png){: width="800"}
_Disabling Logstash And Checking The Connection To Elastic_
According to the guides that I follows, this is to disable Logstash and to ensure Filebeat correctly connected to Elasticsearch. We use ``sudo filebeat setup --index-management -E output.logstash.enabled=false 'output.elasticsearch.hosts=["YOUR_OWN_IP:9200"]'``. 

Next we enable the service ``sudo systemctl enable filebeat.service`` and we start it ``sudo systemctl start filebeat.service``. Lastly we use ``curl --get http://YOUR_OWN_IP:9200/_cat/indices?v``, that command simply making an API call of the URL and printing it out in verbose but also in column format. Making it more readable. The health here is just indicator of shards whether it's condition is healthy, caution or danger but as far as I know, this not really mean much. So don't mind it. Oh and don't forget to take snapshot 📷.

## CA Certificate Authorities Configuration and Elastic IP

But why making CA certificate even though Elastic come with it's own default certificate? To my knowledge the default protocol when access Elastic is HTTP but we want it to be HTTPS, so this is where CA certificate comes in handy. With it, we could enabling HTTPS connection.

Now, without further a do, use ``cd /usr/share/elasticsearch`` to navigate to ``elasticsearch`` directory and enter ``sudo touch instances.yml`` and then open the text editor ``sudo nano instances.yml``. After opening the file, insert this properties
```bash
instances:
        - name: "elasticsearch"
          ip:
                  - "YOUR_OWN_IP"
        - name: "kibana"
          ip:
                  - "YOUR_OWN_IP"
        - name: "fleet"
          ip:
                  - "YOUR_OWN_IP"
```
The IP inserted there is for fleet service able to reach out to it and make CA certificate according to the given IP there. Before exiting, save that file. And yeah, if you want, don't forget to take snapshot 📷.

## Time to Create CA Certificate and Other Certificates

So next is to generate and create other certificates as well, head to ``cd /usr/share/elasticsearch`` and we enter command ``sudo /usr/share/elasticsearch/bin/elasticsearch-certutil ca --pem``. That basically mean we using executable file that generate the certs itself, and the encryption format of are X.509. Which is a public key certificates used in TLS/SSL, and HTTPS as well to secure the web, more about that can be found [here](https://en.wikipedia.org/wiki/X.509). As for the [``--pem``](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail) itself, it's a standard file format of storing and sending encrypted key and certificate. And with that, we will get output as follows:

![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/creating-ca-and-unzip-ca.png){: width="800"}
_CA Certificates Output_

One more thing to know based on that output is that, we using the ``instances.yml`` that made previously to help us generate these certificates. And we compressing it in zip file format, as for the file name I choose to name it ``elastic-stack-cat.zip`` but you can choose whatever name convenient to you. With it, we enter ``sudo unzip elastic-stack-ca.zip`` to unzipping the file.

Nicely done, next steps is to put use of these certificates by using it to generate private key and another certificates. Use this command ``sudo /usr/share/elasticsearch/bin/elasticsearch-certutil cert --ca-cert ca/ca.crt --ca-key ca/ca.key --pem --in instances.yml --out cert.zip``. Follow the screenshot below.

![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/private-key-output.png){: width="800"}
_Generating Private Keys_

I'll go with ``cert.zip`` as the name but and I we also need to unzipping this file too. And after that we need to use command ``sudo unzip cert.zip`` to unzipping it.

![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/unzipping-ca-cert-and-making-dir-certs.png){: width="800"}
_Unzipping Cert And Making Directory Cert_

Notice that Linux making it's own directory after unzipping it, because of that the next thing is to move all of those certificates to our newly created directory. We make directory to store all those certificates with ``sudo mkdir certs``, after the directory created use ``sudo mv /usr/share/elasticsearch/elasticsearch/* certs/`` first, then ``sudo mv /usr/share/elasticsearch/kibana/* certs/``, lastly ``sudo mv /usr/share/elasticsearch/fleet/* certs/``. After the copying steps done, we then use ``sudo mkdir -p /etc/elasticsearch/certs/ca && sudo mkdir -p /etc/kibana/certs/ca && sudo mkdir -p /etc/fleet/certs/ca`` to make CA own directory just like screenshot below where we could store our CA certificates.

The CA directory has been made, now we copy CA certs and other certificates to their respective directory with these command.
- ``sudo cp ca/ca.* /etc/elasticsearch/certs/ca && sudo cp ca/ca.* /etc/kibana/certs/ca && sudo cp ca/ca.* /etc/fleet/certs/ca``
- ``sudo cp certs/elasticsearch.* /etc/elasticsearch/certs/ && sudo cp certs/kibana.* /etc/kibana/certs/ && sudo cp certs/fleet.* /etc/fleet/certs/``

I used list command to check the directory and noticing that there's still certs directories left around. So lastly just like mom used to say, clean up after playing with your toy and like a good kid we are we clean up our mess with ``sudo rm -r elasticsearch/ kibana/ fleet/``
![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/copy-private-key-and-ca-with-clean-up.png){: width="800"}
_Moving The Certificates Files_


Also don't forget to take snapshot 📷 hehehehe.

## Best Practice For SIEM Deployment

This section is for us to implement best practice when we deploying SIEM is to have least privilege within our systems and according nubb, the PrintNightmare threat is still lurking rampant out there so gotta be careful. More about that you could read [here](https://www.sygnia.co/threat-reports-and-advisories/demystifying-the-print-nightmare-vulnerability/).

And this also correlate with our usage of sudo command where we want the least privilege. With that in mind, head to ``cd /usr/share`` and execute commands:
- ``sudo chown -R elasticsearch:elasticsearch elasticsearch/``,  which will recursively set the ownership of the elasticsearch.
- ``sudo chown -R elasticsearch:elasticsearch /etc/elasticsearch/certs/ca``
The first command let us set the ``elasticsearch`` directory to **Elasticsearch** and also change its group to ``elasticsearch`` that helps us limit permission to again, **Elasticsearch**. Second command is doing the same thing but for ``ca`` directory.

![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/best-practice-for-siem-deployment.png){: width="800"}
_Best Practice SIEM Deployment_

Based on that screen there's couple of command flag that I want to explain to the best of my understanding. ``-in`` flag used to specifying the certificate to check out, ``-text`` to make the output in readable format and lastly ``-noout`` is basically minimizing the gibberish encoded message to help further making it more readable.

## Setting Up HTTPS Connection With the Certificates

First head to the ``kibana.yml`` file with ``sudo nano /etc/kibana/kibana.yml`` and we copy this YML script below:
```yml
server.ssl.enabled: true
server.ssl.certificate: "/etc/kibana/certs/kibana.crt"
server.ssl.key: "/etc/kibana/certs/kibana.key"

elasticsearch.hosts: ["https://YOUR_OWN_IP:9200"]
elasticsearch.ssl.certificateAuthorities: ["/etc/kibana/certs/ca/ca.crt"]
elasticsearch.ssl.certificate: "/etc/kibana/certs/kibana.crt"
elasticsearch.ssl.key: "/etc/kibana/certs/kibana.key"

server.publicBaseUrl: "https://YOUR_OWN_IP:5601"

xpack.security.enabled: true
xpack.security.session.idleTimeout: "30m"
xpack.encryptedSavedObjects.encryptionKey: "min-32-byte-long-strong-encryption-key"
```
Remember to change ``YOUR_OWN_IP`` to your own IP and also some of you might notice that I input ``10.0.2.15`` as the IP right? Well that just me being dumbo and you will see what happen when you don't correctly set the IP in the next section. And all of These properties is all within the ``kibana.yml`` but it will be pain in the ass to search each of it and editing them so yeah, just copy that. And it will look like this.

![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/configuring-kibana-ssl-https.png){: width="800"}
_Configuring Kibana SSL_

The properties here somewhat self explanatory, the public base URL is our Kibana address, and others is for our encryption thingies.  But albeit, I agree with nubb here, I was also confused when first time handling the certificates and encryption stuff of ELK stack. But basically it's just how Kibana telling Elastic that the communication between them is secured with various encrypted certificates and that Kibana make sure, it is trusted Kibana that talking with Elasticsearch. 

Or, cmiiw that is just how I understands these whole shenanigan. Proceed to next step that is basically the same but for ``elasticsearch.yml``. ``sudo nano /etc/elasticsearch/elasticsearch.yml`` and start add the properties. 

```yml
xpack.security.enabled: true
xpack.security.authc.api_key.enabled: true

xpack.security.transport.ssl.enabled: true
xpack.security.transport.ssl.verification_mode: certificate
xpack.security.transport.ssl.key: /etc/elasticsearch/certs/elasticsearch.key
xpack.security.transport.ssl.certificate: /etc/elasticsearch/certs/elasticsearch.crt
xpack.security.transport.ssl.certificate_authorities: ["/etc/elasticsearch/certs/ca/ca.crt"]

xpack.security.http.ssl.enabled: true
xpack.security.http.ssl.verification_mode: certificate
xpack.security.http.ssl.key: /etc/elasticsearch/certs/elasticsearch.key
xpack.security.http.ssl.certificate: /etc/elasticsearch/certs/elasticsearch.crt
xpack.security.http.ssl.certificate_authorities: ["/etc/elasticsearch/certs/ca/ca.crt"]
```

![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/configuring-elasticsearch-ssl-https.png){: width="800"}
_Configuring Elasticsearch SSL_

I'll leave the [official documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/security-settings.html#token-service-settings) for you to read more information and nubb explanation about this SSL things. And I agree again with nubb here, are the Elastic devs high on something when making the script to not use quotation for file path? Only God and them knows why.

With that said and done, we have to ``sudo systemctl restart elasticsearch`` and next ``sudo systemctl restart kibana`` on respective order for enabling us applying the changes. With that don't forget to take snapshot 📷.

## Testing Connections to Elasticsearch

Now let's what all of these headache got us. Execute the command ``curl --get https://YOUR_OWN_IP:9200``.

![Desktop View](assets/img/posts/testing-curl-for-https-and-ssl-with-insecure-flag.png){: width="800"}
_Testing Connections_

Notice that it failed for the first time? Yeah, like I said, if it's too good to be true in IT, chances are you missing something. Apparently the reason for the failed testing is because while the Kibana and Elasticsearch trust each other, the browser is don't. 

Kinda like when you bring your other friend to your other friend, of course they still don't trust each other instantly right?. That is why as their friend you would say "Trust me bro, this kid is good" and that is exactly what we would do with the command ``curl --get https://10.0.2.15:9200 --insecure | jq`` the ``--insecure`` is the "trust me bro" in this case where we bypassing it temporarily and as for the ``jq`` telling the output to make on JSON format. This why we install JSON packages previously.

## Creating Credentials

Now one more thing before we see ELK stack frontend, we have to generate credential in order to login to ELK stack. Here's the command ``sudo /usr/share/elasticsearch/bin/elasticsearch-setup-passwords auto`` and basically this means we let the Elastic generate it for us. All the information about it can be found [here](https://www.elastic.co/guide/en/elasticsearch/reference/current/setup-passwords.html). Here's what it will look like

![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/generating-credential-for-elastic.png){: width="800"}
_Generated Credentials_

And don't forget to save all of that credentials, oh and also **don't change the ``elastic.username``** it's for superuser and it basically the core user of ELK. Changing it will make ELK stack error.

Now with all of that restart the ELK stack with ``sudo systemctl restart kibana && sudo systemctl restart elasticsearch``.

Now we ready to login ELK stack and using it. But before that, don't forget to take snapshot 📷.

## Moment of Truth

Oh and if you just start again don't forget to start the ELK stacks. I'll explain it further but take a look this screenshot.

![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/our-first-kibana-logging-in.png){: width="800"}
_Let The Browser Trust The Connection_

The browser showing that because it still don't trust the ELK stack connection yet and warning us. But we know this is a trusted connection and let the browser know that by clicking **Advanced...** and **Accept the Risk and Continue**

![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/kibana-server-is-not-ready-yet.png){: width="800"}
_Kibana Server Not Ready Yet_

Interesting that we access it with ``https://127.0.0.1:5601`` but not with ``YOUR_OWN_IP``. Why is that? Well, that is because the ELK stack hosted on your own IP but not accessible publicly, that is why accessing it locally with ``127.0.0.1``. And what's that `not ready yet` message? Welp, just like the name suggested, the system is indeed not ready yet, maybe warm up or smth `¯/_(ツ)_/¯`.

We a bit and..... voilà, the ELK stack front ladies and gentlemen. Phew... that was long journey huh? Nicely done guys. 

![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/voila-the kibana is-ready.png){: width="800"}
_ELK Stack Finally Able To Use_

## What's About the Wrong IP Before?

This last section is to explanation the silliness that made previously by entering wrong IP. Basically what will happen is that ELK stack will infinitely on **Not Ready Yet** status and to fix it?

![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/kibana-server-is-not-ready-yet.png){: width="800"}
_Infinite Kibana Server Not Ready Yet_

To fix it we have to change it back to your correct IP in ``kibana.yml`` like shown in the picture below.

![Desktop View](assets/img/posts/2025-05-20-ELK-installation-with-two-vm/if-youre-not-a-dumbo-change-the-goddamn-ip.png){: width="800"}
_Change that 10.0.2.15 With Your Own Ip_

And that `10.0.2.15` needed to be change to your own IP. With that, you finally fixing the **Not Ready Yet** status.

## Last Words and References

Thank you so much for spending your time following and reading through all of the yapping from me. I really hope this guide helping you on some way with your learning journey of cyber security stuff. And with this final words, I want to reference some of the sources that I used in this part of guide.

| References                                                                                                                                                                                                         |
| :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| nubb guide on github. [Elastic-SIEM-Setup](https://github.com/nubbsterr/Elastic-SIEM-Setup)                                                                                                                        |
| LevelEffect guide by Will Nissler. [How to Set Up Your Own Home Lab with ELK](https://www.leveleffect.com/blog/how-to-set-up-your-own-home-lab-with-elk)                                                           |
| ELK stack definition. [elastic](https://www.elastic.co/elastic-stack)                                                                                                                                              |
| Elastic discovery documentation. [Discovery and cluster formation settings](https://www.elastic.co/docs/reference/elasticsearch/configuration-reference/discovery-cluster-formation-settings )                     |
| X.509 definition. [X.509](https://en.wikipedia.org/wiki/X.509)                                                                                                                                                     |
| PrintNightmare information. [Demystifying The PrintNightmare Vulnerability](https://www.sygnia.co/threat-reports-and-advisories/demystifying-the-print-nightmare-vulnerability/)                                   |
| SSL and other security information of ELK stack documentation. [Security settings in Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/security-settings.html#token-service-settings) |
| PEM certificate formats information. [Privacy-Enhanced Email](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail)                                                                                                 |
| Kibana endless not ready yet status fix. [Kibana server is not ready yet [closed]](https://stackoverflow.com/questions/58011088/kibana-server-is-not-ready-yet)                                                    |
