---
title: Bandit OverTheWire Writeup, Bandit6 & Bandit7
date: 2025-08-11 15:55 +0700
categories: [Linux, Bandit OverTheWire, Learning]
tags: [linux, linux command, learning, os, writeup]
---

## New Series
This is a new series where I go over how I learn linux by completing each level of challenge on [OverTheWire](https://overthewire.org), this being done by me because I still feels my Linux skill still shallow and needed improvement and enter Bandit OverTheWire. Shoutout to the OverTheWire team once again for this useful learning. Let's get straight in to the writeup.

### Small Disclaimer
Why did I start on level 6 tho??? Welp simple, I just recently came to realization to make write up to document my learning when I at level 6, so yeah, in short, **I forgor**.

## Level 6 → Level 7
For the challenge of this level we're being tasked as below.

![Desktop View](assets/img/posts/2025-08-11-bandit-overthewire-bandit6-and-bandit7-writeup/bandit5-to-bandit6-task.png){: width="750"}
_Level 6 Task_

It's quite simple, the way I approach this to first go to the linux manual that is in the task page. And I choose [`find` command manual page](https://manpages.ubuntu.com/manpages/noble/man1/find.1.html#examples), because I think `find` is a correct way to search for this kind of task. And in the manual page, I got this clue to find the password for this level. Here's the clue below.

![Desktop View](assets/img/posts/2025-08-11-bandit-overthewire-bandit6-and-bandit7-writeup/level6-clues(1).png){: width="750"}

![Desktop View](assets/img/posts/2025-08-11-bandit-overthewire-bandit6-and-bandit7-writeup/level6-clues(2).png){: width="750"}

So now that I understand I could use the command like that, I could now use the command like this on the picture below.

```bash
find -type f -size 1033c
```

![Desktop View](assets/img/posts/2025-08-11-bandit-overthewire-bandit6-and-bandit7-writeup/bandit5-to-bandit6-answer.png){: width="750"}

I could now go for level 7.

## Level 7 → Level 8

![Desktop View](assets/img/posts/2025-08-11-bandit-overthewire-bandit6-and-bandit7-writeup/bandit7-to-bandit8-task.png){: width="750"}
_Level 7 Task_

At first, I tried to do this to "check the waters" and see what kind of challenge I'm facing.

![Desktop View](assets/img/posts/2025-08-11-bandit-overthewire-bandit6-and-bandit7-writeup/testing-waters-level7-challenge.png){: width="750"}

Phew... it is indeed a lot, and the fact that the task tasking us to search for the specific it's specific users and in the **whole server** too.... But oh well, let's get to it.

But first, I'm asking ChatGPT to refresh my understanding what is “owned by user” and “owned by group” meant. Were they permissions? Were they some kind of system tags? And how did numbers like 664 or 777 fit into all of this?

![Desktop View](assets/img/posts/2025-08-11-bandit-overthewire-bandit6-and-bandit7-writeup/clearing-up-what-is-owned-by-users-and-group(1).png){: width="750"}

![Desktop View](assets/img/posts/2025-08-11-bandit-overthewire-bandit6-and-bandit7-writeup/clearing-up-what-is-owned-by-users-and-group(2).png){: width="750"}

Okay.... my understanding up until now was aligned with ChatGPT, the file that we want to searched was belong to bandit7 as the owner but it also belong to group of user in the bandit6. But what I still don't understand is that, why this connected to `664` or `777` or `731` etc, because when I back then learned linux, I know this things connected to each other but at this point I still confused about them. And how would I know who is bandit7 and bandit6? I still don't quite get it past these concept. That is why I first tried this command below.

```bash
find / -type f -perm 664 && ls -al
```

![Desktop View](assets/img/posts/2025-08-11-bandit-overthewire-bandit6-and-bandit7-writeup/level7-first-attempt.png){: width="750"}

My initial thought was that, "how about I combined `find` command with `ls` with" **AND** operand but oh well, looks I was wrong and now, I plan to go back to the fundamental and try to understand what is the differences with users group and those numbers thingies. So, I asked ChatGPT again, here's what it helped understand by it's prompt.

![Desktop View](assets/img/posts/2025-08-11-bandit-overthewire-bandit6-and-bandit7-writeup/clearing-up-what-is-owned-by-users-and-group(3).png){: width="750"}

![Desktop View](assets/img/posts/2025-08-11-bandit-overthewire-bandit6-and-bandit7-writeup/clearing-up-what-is-owned-by-users-and-group(4).png){: width="750"}

![Desktop View](assets/img/posts/2025-08-11-bandit-overthewire-bandit6-and-bandit7-writeup/clearing-up-what-is-owned-by-users-and-group(5).png){: width="750"}

Oh okay, now I understand, **the users group are who have authorization over the files, and the numbers thingies are permission, it means what kind of operation and interaction that were allowed**. Oh... okay then.. but I couldn't help to notice when I tried command `ls -al` there's this line in the picture below, that is `drwxr-xr-x` and `lrwxrwxrwx`. What are they?

![Desktop View](assets/img/posts/2025-08-11-bandit-overthewire-bandit6-and-bandit7-writeup/differences-between-drwxr-xr-x-and-lrwxrwxrwx.png){: width="750"}

I got curious by the `l` and `d` but turns out it means:

- d = directory (can contain files, “execute” means you can enter it).

- l = symbolic link (like a shortcut), and its permissions (rwxrwxrwx) don’t actually matter — the target’s permissions do.

That was good to know, but back to the mission.

I then tried this command to once again test my understanding.

```bash
find / -type f -size 33c | ls -al
```

![Desktop View](assets/img/posts/2025-08-11-bandit-overthewire-bandit6-and-bandit7-writeup/level7-second-attempt.png){: width="750"}

Sounds reasonable right, _pipe_ operator for connecting the input and output command… but nope. Looking back with more search, and many ChatGPT later, I could conclude on why it failed:

- `|` sends the output of find to the standard input of ls.
- But ls doesn’t read from stdin like that — it expects file names as arguments.
- So I basically piped text into a command that wasn’t listening for it.

And after some digging on the internet and ChatGPT, I found a proper way to do it and that is

```bash
find / -type f -size 33c 2>/dev/null | xargs ls -al
```

This is a new things for me to learn, because:

- `xargs` collects the paths from find and passes them to ls -al in batches (faster).

- Still hides “Permission denied” errors with 2>/dev/null(sending it to the black hole or berserk eclipse).

With that command I just discovered, I then tried it and here's the result.

![Desktop View](assets/img/posts/2025-08-11-bandit-overthewire-bandit6-and-bandit7-writeup/level7-third-attempt.png){: width="750"}

And it worked! But as you can see it's still rough but I could still find it manually after some scrolling and here's the answer below.

![Desktop View](assets/img/posts/2025-08-11-bandit-overthewire-bandit6-and-bandit7-writeup/level7-answer-location.png){: width="750"}

And with that, here's the answer for level 7.

![Desktop View](assets/img/posts/2025-08-11-bandit-overthewire-bandit6-and-bandit7-writeup/level7-answer.png){: width="750"}

And finally level 7 done.... But after learning more, I could make it more specific and efficient instead of searching it manually and here's the command for it.

```bash
find / -type f -size 33c -user bandit7 -group bandit6 -exec ls -al {} \; 2>/dev/null
```
The more effective command is to:

- Add `-exec` to tell the `find` command to execute another command and that is `ls`.
- `{}` acting as place holder for the `ls`.
- And finally `\;` to make `find` command knows where to end the `-exec` flag command.

And that's it for level 7, see you for the next level in the next write up!.