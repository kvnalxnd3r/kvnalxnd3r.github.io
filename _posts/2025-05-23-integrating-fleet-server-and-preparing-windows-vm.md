---
title: Integrating Fleet Server And Preparing Windows VM
date: 2025-05-20 15:15 +0700
categories: [Tutorial, SIEM, ELK Stack]
tags: [elasticsearch, siem, windows, fleet server, elastic agent, endpoint]
---

## A Little Reminder

**This is part two**, continuation from my post previously where we walk through installing ELK stack SIEM. On this part two, we will take a look at how to integrating our ELK stack with endpoint, in this case is a Windows VM. 

We will learn also what is **Filebeat**, **Fleet Server**, **Elastic Agent**, **Elastic Integration**, and many more tools for getting our lab work as a complete eco system for **simulating SOC monitoring, log capturing, threat hunting, and maybe many more** in the future. Without further a do, let's get into it.

## Creating Administrator User

For this section we will create administrator user so that instead using ``superuser`` account all the time. The reason being is for best practice about privilege(cmiiw). Alright, let's just login to Kibana first using our ``superuser`` like in the screenshot below.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/login-with-superuser.png){: width="800"}
_Login To Kibana Using `Superuser`_

With ``superuser`` account, we could login to Kibana and start to make new user. In the screenshot shown below we will be greeted with **Home** page menu, don't worry if it's just showing template menu, that is just how it is for fresh installment. Now click on the **Hamburger Menu** on the left side and there will be more sub-menu. We need to click on **Stack Management** menu.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/make-new-user.png){: width="800"}
_Heading To Stack Management Menu_

After that, we will be directed to the **Stack Management** page. Here, there's so many options for you to set but for now let's just click on the **Users** sub-menu on the **Security** category.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/navigate-to-user-menu.png){: width="800"}
_Click on Users Sub-menu_

Next there will be **Users** being shown. In here, listed various users, each with it's own **roles**, **status**, and etc. Want we want to do here is click on **Create user** button on the upper right corner.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/click-create-user.png){: width="800"}
_Click on **Create user**_

With that, we will then begin to inputting necessary properties for this planned administrator user. There's various values and properties that you could play around if you want to set something in the future but for now we just focused on these properties in the screenshot below.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/finish-creating-user.png){: width="800"}
_Setting Up New Administrator User_

What basically say to Kibana is that we want this newly created user to have username ``kibana-lab``, password that we set, and the privileges such as ``beats-admin``, ``kibana_admin``, and ``superuser``. From the looks of it, the selected roles is somewhat self-explanatory but if you want to know more of it, [here's the full documentation of it](https://www.elastic.co/guide/en/elasticsearch/reference/current/built-in-roles.html). And with that we could login to the new created user.

## Creating And Integrating Fleet Server

Now that we have new administrator user, we then could login onto it and start integrating Fleet server to our Ubuntu Server machine. This is where our Elastic agents being managed, more about fleet server could be found [here](https://www.elastic.co/docs/reference/fleet/fleet-server). 

To add our new Fleet server just head to **Management** sub-menu inside the hamburger menu and click **Fleet** just like screenshot below.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/navigate-to-fleet-server-menu.png){: width="800"}
_Fleet Server Page_

Fleet server is need some time to load but once its done, click on **Add Fleet Server** button, it will then navigate us to this page where we could download Fleet Server installer. 

Screenshot below shows us that we first need to download the file, for next is to set the ELK stack to **Production** mode because we have our own certificates that we generated on [previous tutorial]((https://kvnalxnd3r.github.io/posts/ELK-installation-with-two-vm/)). And on the fourth steps, we specified the location of where we want to attached our Fleet server host. In here, I insert the address of our Ubuntu Server machine and put it in port ``8220`` and then click 

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/adding-fleet-server-host.png){: width="800"}
_Adding Fleet Server_

Now we go back to our Ubuntu Server VM, head to ``cd /etc/fleet`` and insert this command to download the Fleet server:
``sudo wget https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-7.17.6-linux-x86_64.tar.gz``

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/download-elastic-agent-on-linux.png){: width="800"}
_Downloading Fleet Server/Elastic Agent Installer FiLe_

After done downloading it, we need to extract it using tar command:
``sudo tar -xvf ./elastic-agent-7.17.6-linux-x86_64.tar.gz``
![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/download-and-unzip-the-elastic-agent.png){: width="800"}
_Extracting Fleet Server/Elastic Agent Installer_

And this will make directory named ``elastic-agent-7.17.6-linux-x86_64``, but if you like me and somehow the files is on the ``Desktop`` directory of your linux unlike **nubb** tutorial. Then follow the screenshot below.
![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/moving-elastic-agent-file.png){: width="800"}
_Moving Fleet Server/Elastic Agent Installer Folder_

Here's the steps that you could follows based on that screenshot
1. ``ls``, I first checking on the files or folders my current directory. With it I could see there's only 2 inside, which is just tarball of the installer file and the extracted file.
2. ``sudo mv elastic-agent-7.17.6-linux-x86_64/ /etc/fleet``, is to move the extracted folder to ``/etc/fleet``. Previously failed attempt happen because I don't include ``sudo`` to elevate our privilege. 
3.  ``cd /etc/fleet/elastic-agent-7.17.6-linux-x86_64/``, to navigate inside it.

After we successfully navigating inside the installer folder, we then could insert these commands shown below.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/generate-service-token-and-installation-command.png){: width="800"}
_Generating Fleet Server Installation Command And Token_

On the screenshot we could see the command being:
```bash
sudo ./elastic-agent install --url=https://YOUR_OWN_IP:8220 \
  --fleet-server-es=https://YOUR_OWN_IP:9200 \
  --fleet-server-service-token=YOUR_SERVICE_TOKEN \
  --fleet-server-policy=499b5aa7-d214-5b5d-838b-3cd76469844e \
  --certificate-authorities="/etc/fleet/certs/ca/ca,crt" \
  --fleet-server-es-ca="/etc/elasticsearch/certs/elasticsearch.crt" \
  --fleet-server-cert="/etc/fleet/certs/fleet.crt" \
  --fleet-server-cert-key="/etc/fleet/certs/fleet.key"
```

Keep in mind, **dont FORGET change the all of the IP's** to **YOUR OWN IP**. If we don't do that, the server won't be able communicate with the ELK stack. Here's what it would look like on the terminal.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/installing-elastic-agent-with-the-given-steps.png){: width="800"}
_Fleet Server/Elastic Agent Installation Process_

Last steps is to make sure the newly made Fleet server is able to communicate with the rest of our ELK stack. With that being set, go to **Fleet page** once again and click on the **Fleet Settings** on the upper right corner of the page. We then being showed this window.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/changing-fleet-server-address.png){: width="800"}
_Setting Fleet Server Address_

Make sure your setting is matched with your own IP, otherwise, the Fleet server **will not showing any captured logs** like this screenshot below.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/what-happen-if-fleet-server-address-not-set-correctly.png){: width="800"}
_False IP Address Causing No Logs Appearing_

And lets check on the Fleet server page, it should be succeeded by now.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/fleet-server-connected.png){: width="800"}
_Fleet Server/Elastic Agent Installed Successfully_

And we finally able integrating the Fleet server with our Ubuntu Server machine.

## Creating Agent Policies And Adding Integration

Now that the medium to gather and managed the all of the logs has been created, we need to create utilization in order for us to able collecting the logs right? And that is what [**Agent Policies**](https://www.elastic.co/docs/reference/fleet/agent-policy) are, they are a set of input that has been defined in order collecting data according to their configuration. The agents also worked with Filebeat to get specified logs and could be set working in tandem for log ingestion.

Before we continue further let's check on our Fleet server status and see what it's look like.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/fleet-server-status-and-making-new-agent-policies.png){: width="800"}
_Fleet Server Status_

The status of our Fleet server is **healthy**, that itself is self explanatory but there's also several status such as **Unhealthy**, **Offline**. Next we will be adding new policy.

Now for us to be able making this policies we need to navigate by clicking **Create Agent Policy** tab on the Fleet page and then we will be shown this page.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/creating-agent-policy.png){: width="800"}
_Fleet Agent Policies Tab_

On the image, we also seeing the default policies but what we need is our own custom policy and for that just click on the **Create agent policy** button for then will be showed **Create agent policy window**.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/creating-agent-policy-for-windows.png){: width="800"}
_Creating Agent Policy_

I will be naming this policy **Windows-Endpoint** because we will be putting it on windows endpoint machine for collecting logs. And for the description, fills whatever suits your need. After all of that done, just click **Create agent policy**. After that the policy will be show up on the page now.

And after the policy is available, click on it to configure of basically what integrations we will be putting inside of that policy.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/created-windows-policies-and-click-on-it.png){: width="800"}
_Selecting Newly Created Policy_

Now we're in the list page of all the integrations that is inside the **Windows-Endpoint**. In here we can see various integrations that available, right now only **System** integration that is available but we will add another integration. Just click on **Add integration**.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/click-add-integration.png){: width="800"}
_Add Integration_

When we clicked on it, there will be **Browse Integrations** page and in here we can search also choose what integration that we would like to add. For us now, we need to search **Endpoint Security** for us to be able securing windows endpoint, so use the search button to search for it. Once we got the result, just click on it to set it up.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/searching-for-endpoint-security-integration.png){: width="800"}
_Searching Endpoint Security Endpoint_

Now we're able to configure it to our need, I'll name it ``windows-edr-integration``, fill the description and lastly choosing previously made policy **Windows-Endpoint**.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/setting-endpoint-security-integration.png){: width="800"}
_Endpoint Security Integration_

After all that done, just wait a moment and there will be success notification popped up.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/successfully-adding-endpoint-security-integration.png){: width="800"}
_Successfully Adding Endpoint Security Integration_

The window shows up, we will then click on the **Add Elastic Agent later** button and continue to adding also configuring another integration. The integration this time is **Windows** integration, that way it will help us collecting logs from our Windows machine. Just repeat the previous steps for adding it and then we will arrived in the setting window just like before.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/adding-windows-integration.png){: width="800"}
_Configuring Windows Integration_

I just named it ``windows-log-collect`` for the sake of convenient and then choosing **Windows-Endpoint** policy. After that, continue to scroll down until you see **Show advanced setting** and click on it. We will then presented with various input column, we want to search for **windows.advanced.elasticsearch.tls.ca_crt** properties and in it we will paste ``C:\Windows\System32\ca.crt`` path. This path will later be used when we copying the CA certificate from our linux VM to the Windows, but for now just set it. And we done for the Windows integration, just click **Save and continue**. We done with the needed integration, the integration should be ready to integrated to the target machine but because we don't install any Windows VM yet, the next step is to install Windows VM.

## Preparing Windows VM

Here comes another VM installation part, so let get this over quickly because it's basically just the same stuff with the Linux installation.

1. First we want to download the ISO for Win10, to do that just go to [multi-edition download site](https://www.microsoft.com/en-us/software-download/windows10ISO). Just follow along the instruction on the website.
2. Now that we downloaded media installation tool, run it and we will be shown window for us choosing what kind of action we want for the media creation tool. We want it to make ISO image for the Windows 10.
![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/create-installation-media-windows.png){: width="800"}
_Choosing Windows 10 Image Format_
3. After the format has been chosen, we then select ISO file to be the media of our image. If you want it, you could also choose USB drive but for now we want it to be a file for our VirtualBox.
![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/create-windows-ISO-image.png){: width="800"}
_Creating ISO Image_
4. The EULA pop up, just scroll it until done and click **Accept**.
![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/windows-creation-windows-EUA.png){: width="800"}
_Accepting EULA_
5. Now we need to specify where we need to put the ISO file on our computer and don't forget to naming it. With that done, the download then begin.
![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/downloading-windows-ISO-image.png){: width="800"}
_Specifying File Location_
6. When the download is done, head to VirtualBox and then click on **Machine** > **New** and then select the newly downloaded Windows ISO image. 
7. Don't forget to uncheck the **Unattended install** and begin to set your system requirement, I'll go with ``RAM: 4GB, CPUs: 2, Storage: 35 GB``.  
8. Now just following along the installation instruction but when you arrived at **Which type of installation do you want?** part, just choose the **Custom: Install Windows only (advanced)**.
9. For the allocation of the partition, just use all of it but again, just go with whatever suit you if you want to play around with it.
10. And just wait for the installation to complete. After it finished, I suggest you disabled your internet momentarily. Why? Because the Windows will bothering you with all of its bullshit setup like Cortana(fucking hate this, it just bloat the Windows), especially they forcing you to make Windows account and we don't want that. Disabling the internet just skipping all of that hassle. There's other method tho like inputting **Shift + F10** and ``oobe/bypassnro`` in the CMD. But for me it's not working, so I just go with simplest solution.
11. And now, we done installing Windows VM, here's what it look like.
![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/first-look-of-windows-vbox.png){: width="800"}
_Windows VM_
12. For me, we not done yet. There's still one more thing to configure. And that is adding **Guest Addition CD image**. This adds makes us enable to fixing screen display(I don't like the display), making us seamlessly changing screen(**ALT+TAB**), and many more. Just click **Devices** > **Insert Guest Additions CD image**.
![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/fixing-screen-display-windows-virtual-box.png){: width="800"}
_Windows VM_
13. After we done clicking that, there will be ISO image mounted on the Windows machine, just double click on it.
![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/vbox-additional-guest.png){: width="800"}
_Insert Addition Image_
14. Inside the image, we will presented with various installer. Just double click on **VBoxWindowsAddition.exe** to begin the installation. Just for the installation to complete and after it complete, you will be asked to reboot the Windows VM. Just follow along.
![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/vbox-additional-guest.png){: width="800"}
_Mounting Addition Image_
15. We then could check whether the addition image is installed or not by navigate to **View** and see if the **Virtual Screen 1** has options for various screen resolution. And we done for the VM preparation.
   
## Windows Endpoint Preparation For Elastic Agent

Now we can start by testing the connection of the Windows VM to the Ubuntu Server VM, where our ELK stack installed. 

We start by testing connection alongside the port whether it is listening or not. Just insert command ``Test-NetConnection -port INSERT_RESPECTIVE_PORTS YOUR_OWN_IP``(remember to change it to your own IP and port that you want to test).
![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/testing-windows-VM-connection-to-elasticsearch.png){: width="800"}
_Checking Connection To ELK Stack_

Now we know the connection is succeeded, we then could proceed to installing [**Sysmon**](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon), basically a service for monitoring logs activity of our windows machine. We need to download it through **PowerShell** as **Administrator** and then insert these command:
1. ``Invoke-WebRequest -URI https://download.sysinternals.com/files/Sysmon.zip -OutFile "C:\Program Files\Sysmon.zip"``
2. ``Expand-Archive "C:\Program Files\Sysmon.zip" -DestinationPath "C:\Program Files\Sysmon"``

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/download-sysmon-for-windows.png){: width="800"}
_Downloading Sysmon_

Basically what all of those mean is us to download Sysmon, put it on ``C:\Program Files``, and extracting it. Now that we got the file, we could then ``cd "C:\Program Files\Sysmon"`` and insert the command: 
1. ``Invoke-WebRequest -URI https://raw.githubusercontent.com/SwiftOnSecurity/sysmon-config/master/sysmonconfig-export.xml -OutFile "C:\Program Files\Sysmon\sysmonconfig-export.xml"`` 
2. ``./sysmon.exe -accepteula -i sysmonconfig-export.xml``
  
![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/cd-to-sysmon-dir-and-exporting-xml-file.png){: width="800"}
_Executing The Template Configuration_
Basically the commands says that we downloading template of Sysmon configuration by github user [**SwiftOnSecurity**](https://github.com/SwiftOnSecurity) and [here's the link of the template](https://github.com/SwiftOnSecurity/sysmon-config).

Now we have the template, we need a script to adjusting it, so just enter this ``Shell`` command:
```shell
function Enable-PSScriptBlockLogging
{
$basePath = 'HKLM:\Software\Policies\Microsoft\Windows' +
'\PowerShell\ScriptBlockLogging'

if(-not (Test-Path $basePath))
{
$null = New-Item $basePath -Force
}

Set-ItemProperty $basePath -Name EnableScriptBlockLogging -Value "1"

}
```
The script basically checking whether if there's registry key exist or not and if it's not it will set the boolean condition to **True** with value ``1``.

## Enrolling Elastic Agent

This part where we start by transferring our CA certificate from Ubuntu Server to our Windows but first for extra measure, head to ``cd /usr/share/elasticsearch`` and copy **ca.crt** from there to home directory using ``sudo cp ca/ca.crt /``. If the file is already there then it will showing notification that says there's already same file but if it's not then it will succeeded. Next thing to do, we just need enter this command inside the **PowerShell** as **Administrator**.
``scp YOUR_UBUNTU_USERNAME@YOUR_OWN_IP:/ca.crt ./`` 
![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/copying-ca-certificate-to-windows.png){: width="800"}
_Copy CA Certificate From Ubuntu Server_

That command telling PowerShell to use secure file transfer protocol to copy a file from the Ubuntu Server VM. And now we will do the same with the within the Ubuntu Server that is to insert this command,
1. ``scp YOUR_UBUNTU_USERNAME@YOUR_OWN_IP:/ca.crt /usr/local/share/ca-certificates/`` 
2. ``sudo update-ca-certificates``
It's just copying file but this time we want to update our certificate in case it still uses old certificate.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/copying-ca-certificate-to-linux-ca-certificate-dir.png){: width="800"}
_Copying CA Certificate For Linux Ubuntu Server_

Don't mind the failed first try command, that is happen because I was careless and not include ``sudo`` on my command.

All that left is to download Elastic agent on our Windows VM:
1. ``Start-BitsTransfer -Source https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-7.17.6-windows-x86_64.zip -Destination "C:\Program Files\elastic-agent.zip"``
2. ``Expand-Archive "C:\Program Files\elastic-agent.zip" -DestinationPath "C:\Program Files\elastic-agent"``
![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/download-elastic-agent-on-windows.png){: width="800"}
_Download Elastic Agent For Windows_

Those two commands is just us telling Windows to download it and extracting Elastic agent file on the given directory. With that, Elastic agent installer is on our machine, then we would like to return to **Fleet** page in the Kibana, this time we will adding agent. The page would look like this.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/adding-agent-to-windows-VM.png){: width="800"}
_Adding Agent To Windows_

This page is inside **Fleet** > **Agent Policies** Tab > **Windows-Endpoint**(or your own named Windows endpoint policy). In here we click on **Add agent** on upper menu. After we click on it, there will be window shows up on our right screen that will instruct us on how to adding agent to our endpoint.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/adding-agent-menu.png){: width="800"}
_Add Agent Instruction_

We simply just generate our own enrollment token, also because we already downloading agent installer we just skip to next step that is installation command.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/enrolling-agent-to-windows-command.png){: width="800"}
_Enrollment Installation Comamand_

We inserting this command on our **PowerShell**, basically this command instructing the installer to installing Elastic agent with our address along with attached port, and it's own enrollment token. And don't forget to change to it's directory!.
1. If your PowerShell location outside on any other directory, insert this command ``cd C:\"Program Files"\elastic-agent\elastic-agent-7.17.6-windows-x86_64``. You could replace the ``7.17.6`` with any other version that you installed.
2. ``.\elastic-agent.exe install --url=https://YOUR_OWN_IP:8220 --enrollment-token=YOUR_OWN_TOKEN``. You could also add another flag on that installation command ``--certificate-authorities="C:\Windows\System32\ca.crt"``, but for me I just go along with the command given by the Elastic.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/enrolling-agent-to-windows-command.png){: width="800"}
_Enrollment Installation Command_

And that's it! Just wait a bit for the installation to finish itself and you got your endpoint integrated with Elastic agent policy. Let's check on it, see if it's really installed.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/proves-for-successfully-adding-elastic-agent-on-windows.png){: width="800"}
_Agent Installed Successfully_

We finally Done! What a long way huh, you could use this step to add another agent on different endpoint, adding more integration, making another policy and just explore on your own. I really hope this tutorial make you learn something. Before I finish this tutorial, let's see what would our Kibana catching logs look like.

## Logs Appearing On Kibana

Another moment of truth, let's what our progress done for us now. Click on the hamburger menu button, under Kibana menu section click **Discover** and now you will be shown dashboard.

![Desktop View](assets/img/posts/2025-05-23-integrating-fleet-server-and-preparing-windows-vm/log-appearing-in-kibana.png){: width="800"}
_Captured Logs_

## Final Words

And we officially done with the installation series. From here on, you could start your own exploration, see what kind of scenario you want to set up or you could follow [part 4 of LevelEffect by William Nissler](https://www.leveleffect.com/blog/home-lab-enabling-and-configuring-threat-intelligence-and-detections). This guide is meant to **teaching you guys how to catch the fish** from my POV. You could also add your own twist but for me the basic stays the same.

So again, thank you so much for reading and following this guide and hope you learning something.

