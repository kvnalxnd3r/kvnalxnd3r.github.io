---
title: Bandit OverTheWire Writeup, Bandit14, Bandit15, Bandit16, Bandit17
date: 2025-08-19 16:21 +0700
categories: [Linux, Bandit OverTheWire, Learning]
tags: [linux, linux command, learning, os, writeup]
---

## Level 14 → Level 15

Okay, so yesterday I wasn't able to keep up to post my daily write up for `bandit14` and `bandit15` because of works, and in this writeup, I'll just combine them, let's get into it here's the task.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit14-to-bandit15-task.png){: width="750"}
_Level 15 Task_

So we resume our search for `bandit15` password and, I decided to read again the referenced link in the page because I thought again "Hey, one or two more knowledge despite solid fundamental wouldn't hurt", and I feel like I might gained more insight, because previously I mainly curious about what is `nc` in there. So here's my walkthrough each article being linked.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit14-to-bandit15-first-step.png){: width="750"}

Okay, nice to know that my understanding of port is some sort of **door** that being used on the internet for specific **room** that we want our network to enter is in-line with the explanation above. Okay, let's read next article and btw, you can read the whole article [here](https://computer.howstuffworks.com/web-server8.htm).

Next, let's see what is about `nc` command or more known `netcat` is, and let's see what is it all about [in the article](https://phoenixnap.com/kb/nc-command).

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit14-to-bandit15-second-step.png){: width="750"}

So it's a way for between two devices utilizing TCP or UDP protocol for communication, whether it is to to read or write data. Interesting, maybe this is what this task asking us about but let's continue to further read the article.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit14-to-bandit15-third-step.png){: width="750"}

Gotcha, looks like we may find out our answer for this task, the command asking for `host` and `port` specifically. Okay, nice to know that, but we still have other reference that we need to cross-check before making final decision, let's read it.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit14-to-bandit15-fourth-step.png){: width="750"}

This time it's an [article about SSL](https://www.ssldragon.com/blog/what-is-openssl/), and from the explanation, it's a more secure version of `netcat` then... but I'm guessing we don't need this yet, but it's good to know that there's always more secure and better way. Let's dive into another article.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit14-to-bandit15-fifth-step.png){: width="750"}

Last article is [about `nmap`](https://www.freecodecamp.org/news/what-is-nmap-and-how-to-use-it-a-tutorial-for-the-greatest-scanning-tool-of-all-time/) ah, this tool that I often heard huh, well looks like for me it really is an interesting tool, making us able to see who, what, and where in the network. But gotta be mindful with how we use this tool then, it's a double-edged sword. Nevertheless, it's an Interesting tool, let's scroll down to know more about it.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit14-to-bandit15-sixth-step.png){: width="750"}

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit14-to-bandit15-seventh-step.png){: width="750"}

Really interesting indeed, but looks like we don't need this tool, **yet**. And let's back to `nc` or `netcat` and see how it goes if we tried to use it. For that, we need to look up it's **man page** first to know more in-depth about this tool. 

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit14-to-bandit15-eighth-step.png){: width="750"}

Oh just like the article explained, it asking for port and host. Alright, let's try it then.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit14-to-bandit15-ninth-step.png){: width="750"}

And we got the answer, but let me explain a little about what is happening here. When we first use `netcat` and we want to make communication with the `port 30000` inside `localhost`, we've been asked to input password and this password is our `bandit14` password and just like that, we got our `bandit15` password.

Now this level done, let's move to the next level.

## Level 15 → Level 16

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit15-to-bandit16-task.png){: width="750"}
_Level 16_

Okay, what a nice task, if before we use `netcat` for communication, now we use `SSL/TLS` for the communication. I like the way the tasks/challenges being structured so far, for that I would likely to thanks OverTheWire team for this learning experience.

Now back to topic, let's read some of the explanation first from the article, so that we may know what is this `SSL/TLS` about.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit15-to-bandit16-first-step.png){: width="750"}

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit15-to-bandit16-second-step.png){: width="750"}

Btw, here's the complete article link for [SSL Wikipedia explanation](https://en.wikipedia.org/wiki/Transport_Layer_Security) and [the SSL cookbook](https://www.feistyduck.com/library/openssl-cookbook/online/testing-with-openssl/index.html) if you're interested reading it in detail.

Now, let's see what's in store for the **man page** about SSL, and see how it work in practice.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit15-to-bandit16-third-step.png){: width="750"}

Okay that is the instruction for using SSL command, let's test it then.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit15-to-bandit16-fourth-step.png){: width="750"}

The command is working as intended, let's scroll down a bit to see the rest of the output.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit15-to-bandit16-fifth-step.png){: width="750"}

Oh, what is this? Why is it stops there? At first I thought the code has error, or the terminal is not responding, I spent quite some time here and eventually asking ChatGPT about it and here's what ChatGPT told me.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit15-to-bandit16-sixth-step.png){: width="750"}

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit15-to-bandit16-seventh-step.png){: width="750"}

Ahhhh now I understand it completely when using it in-practice, the certificate that I see previously was just the way linux telling that this communication is encrypted and here's the proof of it with certificate. But we still need to input the password for the answer to show up.

Now then let's restart the SSL communication and input the password in the last.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit15-to-bandit16-eighth-step.png){: width="750"}

And voilà, here's the answer, we now can move to the next level. 

## Level 16 → Level 17

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit16-to-bandit17-task.png){: width="750"}
_Level 17_

The task now asking us to combining `nmap` to search for the available port and use the `SSL` command after we find the available port, okay then, let's try it but first, some article reading time lel. I just wanted to know some addition knowledge about port scanner in general.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit16-to-bandit17-first-step.png){: width="750"}

Okay, so with this tool I could see what is **open and closed** port of services on the network and there's some security concern regarding it's usage. Well, nice basic to know and you could read it more [here](https://en.wikipedia.org/wiki/Port_scanner). Let's begin solving this level by reading **man page** for port scanner `nmap`.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit16-to-bandit17-second-step.png){: width="750"}

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit16-to-bandit17-third-step.png){: width="750"}

Okay, now that I know what exactly `nmap` in linux, I could then search scanning command that made us able to **scan wide range** of ports on the internet. And I got this clue here.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit16-to-bandit17-fourth-step.png){: width="750"}

Okay, we now know how to scan range of ports, a little side note here is that `<target IP>` for our case should be changed to `localhost` because we're on a local environment. So let's give it a try then.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit16-to-bandit17-fifth-step.png){: width="750"}

So we 5 available ports and to know which one can be sended SSL communication, we need to test it one-by-one. Here's how it went for me.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit16-to-bandit17-sixth-step.png){: width="750"}

First try didn't get any respond, let's continue again.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit16-to-bandit17-seventh-step.png){: width="750"}

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit16-to-bandit17-eighth-step.png){: width="750"}

It got response but I still didn't quite understand by what **KEYUPDATE** mean, so I asked internet for what it is.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit16-to-bandit17-ninth-step.png){: width="750"}

Well looks like it tries to refresh the session but after trying to refresh it, it just `closed` again. Looks like it's not the port that we wanted to, so resume our search then.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit16-to-bandit17-tenth-step.png){: width="750"}

Still no answer on this port either, so another port again.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit16-to-bandit17-eleventh-step.png){: width="750"}

And we got response now, but I still didn't quite understand of what purpose this certificate and why there's so much output being thrown at me, so I'll ask ChatGPT for some clearance.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit16-to-bandit17-twelveth-step.png){: width="750"}

After that prompt and some digging, I know that the certificates being shown for us to use it combining with SSH connection and we could add `-quiet` flag to reduce the noise of the output, so let's copy it then and put it on a file.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit16-to-bandit17-thirteenth-step.png){: width="750"}

Let's make the file with command `nano bandit17.key` and copy the certificate inside it.
![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit16-to-bandit17-fourteenth-step.png){: width="750"}

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit16-to-bandit17-fifteenth-step.png){: width="750"}

Whoops, what is this? Permission denied? Why is that? After some digging, turns out that we cannot make, edit, or erase file directly onb the localhost of bandit. So, let's head to `/tmp` directory, this is where we have permission to perform any action to a file.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit16-to-bandit17-sixteenth-step.png){: width="750"}

don't forget to change the owner of the file using `chmod 400 bandit17.key`.
![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit16-to-bandit17-seventeenth-step.png){: width="750"}

And after that, let's put to the test the key we obtained previously.
![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit16-to-bandit17-eighteenth-step.png){: width="750"}

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit16-to-bandit17-nineteenth-step.png){: width="750"}

Huh? Why is it denied? And what is this about port 22? Another digging and search, the error occurs because we tried to communicate with port 22 but it is no permitted to do that, that is why we need to use the `-port` flag using port 2220, so that the command knows that we want to communicate with port 2220

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit16-to-bandit17-twentieth-step.png){: width="750"}

And we finally able to get inside `bandit17`, from here on, we now could printout the pass for `bandit17`.

![Desktop View](assets/img/posts/2025-08-19-bandit-overthewire-bandit14-bandit15-bandit16-bandit17-writeup/bandit16-to-bandit17-twentieth-first-step.png){: width="750"}

And we finally got it, nice.

