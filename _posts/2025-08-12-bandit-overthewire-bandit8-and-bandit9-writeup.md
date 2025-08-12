---
title: Bandit OverTheWire Writeup, Bandit8 & Bandit9
date: 2025-08-12 17:20 +0700
categories: [Linux, Bandit OverTheWire, Learning]
tags: [linux, linux command, learning, os, writeup]
---

## Level 7 → Level 8

Today we have level 7 and here we gonna learn something new again from this task below.

![Desktop View](assets/img/posts/2025-08-12-bandit-overthewire-bandit8-and-bandit9-writeup/bandit7-to-bandit8-task.png){: width="750"}
_Level 7 Task_

Now, we being tasked for finding password stored in `txt` file and that it located near the word **`millionth`**. Well, this is interesting, because for me, I have some cases IRL that when I needed to search some words but I just know the words surrounding it. So, let's begin how I being able to locate the password by first checking if the `data.txt` really is in our current directory by using `ls` command.

First, enter the `ls` command.
```bash
ls -a
```
![Desktop View](assets/img/posts/2025-08-12-bandit-overthewire-bandit8-and-bandit9-writeup/first-confirming-the-data-txt-file-location.png){: width="750"}

We use `-a` flag to told linux that we want **all of the things in the directory**, and we have it. Now that we confirmed the location, I remembered that in the task it hinted that I should look up `grep`, and I remember also that one of the most used command to search for words, strings, etc. But, I still don't know how to use `grep`, so based on a lot of peoples suggestion to always look up **man page** if in-doubt of linux commands. 

And here it is, the information contained in the **man page**.

![Desktop View](assets/img/posts/2025-08-12-bandit-overthewire-bandit8-and-bandit9-writeup/second-use-man-grep-page-to-know-how-to-use-grep.png){: width="750"}

And to my surprise, the answer already contained on the **SYNOPSIS** part in the **man page**, and for linux **man page** consist of several sections like, **NAME**, **SYNOPSIS**, **DESCRIPTION**, etc. And for the **SYNOPSIS** section, it explain how the command is used, and when I sees this, it already answered it. And now, I could just use the command like this below.

![Desktop View](assets/img/posts/2025-08-12-bandit-overthewire-bandit8-and-bandit9-writeup/third-applying-grep-knowledge-rightaway-and-got-the-answer.png){: width="750"}

From the **SYNOPSIS section in man page** I understand that `grep` command consist of `grep [OPTION ... ] PATTERNS [FILE ... ]` that the command first being told, second the option or the flag, third is the pattern in this case **"millionth"** and finally the file on where to look the patterns. And just like that, I have the password for the next level.

## Level 8 → Level 9

After entering next level, level 8. I presented with this task below.

![Desktop View](assets/img/posts/2025-08-12-bandit-overthewire-bandit8-and-bandit9-writeup/bandit8-to-bandit9-task.png){: width="750"}

So, on the task we have new hint being **Pipeline and Redirection**, I know it refers to this `|` thing but I just couldn't wrapped my mind at this point of it's concept. So, let's see what this is all about by clicking the link attached on the link.

![Desktop View](assets/img/posts/2025-08-12-bandit-overthewire-bandit8-and-bandit9-writeup/first-checking-on-the-pipeline-definition.png){: width="750"}

Okay.... from that I understand that pipe kinda like combining command like???? Oh gotta tried it to confirm my understanding, but first, let's check the **data.txt** existence in our current directory by `ls` command again.

![Desktop View](assets/img/posts/2025-08-12-bandit-overthewire-bandit8-and-bandit9-writeup/second-confirming-again-data-txt-location.png){: width="750"}

Good, the file exist now all is left to do is to combine the command that I remember will help me search the answer, the `sort` and `uniq` command. I remembered that `sort` from it's name sorting the output and `uniq` is that only searching for a uniq occurrence of a string and given the task said that it only occurred once, it's matched with the condition of the command. So here's what the **man page** says about those command.

![Desktop View](assets/img/posts/2025-08-12-bandit-overthewire-bandit8-and-bandit9-writeup/third-checking-sort-man-page.png){: width="750"}

![Desktop View](assets/img/posts/2025-08-12-bandit-overthewire-bandit8-and-bandit9-writeup/third-checking-uniq-man-page.png){: width="750"}

Oh, from the looks of it they're similar and doesn't have that much differences. So in my understanding at this point was that, I could just use the pipeline, no matter how the positioning of the command. So here below, me testing my understanding.

![Desktop View](assets/img/posts/2025-08-12-bandit-overthewire-bandit8-and-bandit9-writeup/fourth-testing-my-understanding-using-pipeline.png){: width="750"}

Hhhhmmmmm.... The output was sorted but the `uniq` command seems not working. I feels like I'm still poorly understand the pipeline and the commands itself. So, next thing I do was to asked ChatGPT and verified my understanding. Here's what ChatGPT gives.

![Desktop View](assets/img/posts/2025-08-12-bandit-overthewire-bandit8-and-bandit9-writeup/fifth-asking-chatgpt-for-clearing-up-misunderstanding(1).png){: width="750"}

![Desktop View](assets/img/posts/2025-08-12-bandit-overthewire-bandit8-and-bandit9-writeup/fifth-asking-chatgpt-for-clearing-up-misunderstanding(2).png){: width="750"}

Ahhhh from that explanation I could conclude that I indeed have misunderstanding of how pipeline and `uniq` works. So, pipeline function as a way to sent first command output acting as an input for the second command. And because the nature of `uniq` command that need an input first before being able to executed, it needed to be placed on the second command. And because `sort` command output's could work as an input, this made the pipeline comes in handy when conjugating these two commands. 

And this is what I learn from this, **the behavior of the command and the positioning of command in pipeline is important**. So, with that in mind, here's how I finally able to find the pass.

![Desktop View](assets/img/posts/2025-08-12-bandit-overthewire-bandit8-and-bandit9-writeup/sixth-got-the-answer.png){: width="750"}

And I've done it, you noticed that I put `-u` flag on the `uniq` command, that is because to help the command be more specific to only find the unique occurrence.

So yeah, this is my journey figuring out this challenge, hope you learn something from this and thank you for your time to read about this writeup.
