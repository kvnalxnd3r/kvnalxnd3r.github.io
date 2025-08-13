---
title: Bandit OverTheWire Writeup, Bandit10 & Bandit11
date: 2025-08-13 15:40 +0700
categories: [Linux, Bandit OverTheWire, Learning]
tags: [linux, linux command, learning, os, writeup]
---

## Level 9 → Level 10

The challenge we have today is as follow on the picture below.

![Desktop View](assets/img/posts/2025-08-13-bandit-overthewire-bandit9-and-bandit10-writeup/bandit9-to-bandit10-task.png){: width="750"}
_Level 7 Task_

We still have to learn about `uniq` occurrence of strings data type and sorting it among the sea of gibberish character huh.... Well this will be interesting, I get the feeling we'll use this a lot too IRL scenario, so let's jump into it.

![Desktop View](assets/img/posts/2025-08-13-bandit-overthewire-bandit9-and-bandit10-writeup/bandit9-to-bandit10-first-step.png){: width="750"}

As per usual, we'll need to check on the `data.txt` file, and it's in our current directory, nice. Let's go in to my next step to find the password. In my first attempt below I tried to use previous challenge command `sort data.txt | uniq -u`, let's see what'll happen.

![Desktop View](assets/img/posts/2025-08-13-bandit-overthewire-bandit9-and-bandit10-writeup/bandit9-to-bandit10-second-step.png){: width="750"}

Hhhmm.... it really is packed with many gibberish character huh, but let's try combined that command with `grep` command because I remember people often used `grep` command to search for text inside a file and maybe we'll have the answer if we do so. Here's my attempt below.

![Desktop View](assets/img/posts/2025-08-13-bandit-overthewire-bandit9-and-bandit10-writeup/bandit9-to-bandit10-third-step.png){: width="750"}

Interesting it says `standard input` alongside `binary file matches`. I got curious and asked ChatGPT what is this about and here's what ChatGPT response.

![Desktop View](assets/img/posts/2025-08-13-bandit-overthewire-bandit9-and-bandit10-writeup/bandit9-to-bandit10-fourth-step.png){: width="750"}

Oh, so `grep` command works only when the input is text and in this case because our `data.txt` mostly consist of non-text character, it triggers the error saying that there's binary in there. Okay, got it, now, let's the **man page** again to check what are our options and how to use `grep` command. Here's below what the **man page** says.

![Desktop View](assets/img/posts/2025-08-13-bandit-overthewire-bandit9-and-bandit10-writeup/bandit9-to-bandit10-fifth-step.png){: width="800"}

Ahah, see the `-a` flag option? It says it'll process even binary file, so let's get to it then!

![Desktop View](assets/img/posts/2025-08-13-bandit-overthewire-bandit9-and-bandit10-writeup/bandit9-to-bandit10-sixth-step.png){: width="750"}

```bash
sort data.txt | grep -a "=====" | uniq -u
```

In the command above, I put multiple It it indeed a human-readable text, I could notice "=" to matched the condition on the given task, and looks like it worked! But seeing it now, it really is so noticeable a human-readable set of string. But with this, we done searching for bandit10 password, let's move to the next challenge.

## Level 10 → Level 11

![Desktop View](assets/img/posts/2025-08-13-bandit-overthewire-bandit9-and-bandit10-writeup/bandit10-to-bandit11-task.png){: width="750"}

We now in **bandit10** user and we need to search for the password of **bandit11** user. And based on that task above, we still need to search in on `data.txt` but this time, we are being told to visit [Base64 wiki explanation](https://en.wikipedia.org/wiki/Base64). Why is that? well let's check the `data.txt` first and see what really is inside of it.

![Desktop View](assets/img/posts/2025-08-13-bandit-overthewire-bandit9-and-bandit10-writeup/bandit10-to-bandit11-first-step.png){: width="750"}

I first like usual, check the `data.txt` if it's in our directory, and it's a yes. Next, I tried to compile what is inside `data.txt` using `cat`. And file `data.txt` is indeed here but it contained an encrypted strings. So I decide to learn more this **Base64** in the attached link on the task page.

After a while for me reading the Wikipedia page, I notice this below.

![Desktop View](assets/img/posts/2025-08-13-bandit-overthewire-bandit9-and-bandit10-writeup/bandit10-to-bandit11-second-step.png){: width="750"}

So it basically helps turning a readable strings into a encrypted strings huh. Well, nice to know that, so for that, I'm back to my Linux and open **man page** for the `base64` and see how to use this command.

![Desktop View](assets/img/posts/2025-08-13-bandit-overthewire-bandit9-and-bandit10-writeup/bandit10-to-bandit11-third-step.png){: width="750"}

And in it, I found the option on the **DESCRIPTION** of base64 to use `-d` flag if we want to decode an base64 strings. So let's try it.

![Desktop View](assets/img/posts/2025-08-13-bandit-overthewire-bandit9-and-bandit10-writeup/bandit10-to-bandit11-fourth-step.png){: width="750"}

And looks like we got it ladies and gentlemen. And that's it for today, see you on the next challenge tomorrow.