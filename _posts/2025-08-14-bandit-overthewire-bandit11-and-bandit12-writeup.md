---
title: Bandit OverTheWire Writeup, Bandit11 & Bandit12
date: 2025-08-14 17:00 +0700
categories: [Linux, Bandit OverTheWire, Learning]
tags: [linux, linux command, learning, os, writeup]
---

## Level 11 → Level 12

Let's get into the point straight, today we have challenge 11 and 12, here we go.

![Desktop View](assets/img/posts/2025-08-14-bandit-overthewire-bandit11-and-bandit12-writeup/bandit11-to-bandit12-task.png){: width="750"}
_Level 11 Task_

So we begin as usual by reading what the external reading has to offered. In this case, OverTheWire guide us to read about [**ROT13 in wikipedia**](https://en.wikipedia.org/wiki/ROT13) in order to solve this level, well let's see then what is this ROT 13 about.

![Desktop View](assets/img/posts/2025-08-14-bandit-overthewire-bandit11-and-bandit12-writeup/bandit11-to-bandit12-first-step.png){: width="750"}

Oh, so it's basically switching position of alphabetical order by rotating it 13 places, well let's read further then. And what is this....

![Desktop View](assets/img/posts/2025-08-14-bandit-overthewire-bandit11-and-bandit12-writeup/bandit11-to-bandit12-second-step.png){: width="750"}

Ohhh looks like we got our answer bois... In order to make this encryption possible, we need `tr` command and combine it with how we want to switch each letter complete with it's cases. Okay, next we check the `data.txt` file and inside of the file.

![Desktop View](assets/img/posts/2025-08-14-bandit-overthewire-bandit11-and-bandit12-writeup/bandit11-to-bandit12-third-step.png){: width="750"}

It is indeed being encrypted, and now let's see what's linux **man page** has to say about `tr` command so we then could start encrypting it.

![Desktop View](assets/img/posts/2025-08-14-bandit-overthewire-bandit11-and-bandit12-writeup/bandit11-to-bandit12-fourth-step.png){: width="750"}

"Translate or delete character" okay, this is the command that we need, and the STRING options indicates that is where we put those switching mechanic. Well, let's get to it then, to encrypt the file.

![Desktop View](assets/img/posts/2025-08-14-bandit-overthewire-bandit11-and-bandit12-writeup/bandit11-to-bandit12-fifth-step.png){: width="750"}

Finally we got our password for next challenge, okay, quite nice for warming up, let's get into level 12.

## Level 11 → Level 12

For level 12, we have this task at hand, it's quite lengthy instruction but we will learn a lot from this.

![Desktop View](assets/img/posts/2025-08-14-bandit-overthewire-bandit11-and-bandit12-writeup/bandit12-to-bandit13-task.png){: width="750"}
_Level 12 Task_

So, let's get to the wikipedia about what is [hexdump](https://en.wikipedia.org/wiki/Hex_dump) is.

![Desktop View](assets/img/posts/2025-08-14-bandit-overthewire-bandit11-and-bandit12-writeup/bandit12-to-bandit13-first-step.png){: width="750"}

Something something about viewing text, ASCII, encryption, and reverse engineer? I still don't quite get the meaning, so let me search it a bit in the internet in simple term then.

![Desktop View](assets/img/posts/2025-08-14-bandit-overthewire-bandit11-and-bandit12-writeup/bandit12-to-bandit13-second-step.png){: width="750"}

Ahah, so it turn binary file content into hexadecimal format, decimal, or ASCII(i.e. the word, letter, and sentence on your screen) so that we could understand what is the inside of that binary file, sounds an amazing tool then. Now, let's check the file then, and then we make temporary directory on `/tmp/` and copy `data.txt` file to there but before that, I wanted to ask ChatGPT, what is `mktemp` is about.

![Desktop View](assets/img/posts/2025-08-14-bandit-overthewire-bandit11-and-bandit12-writeup/bandit12-to-bandit13-third-step.png){: width="750"}

Oh okay, we don't need to specify the name because it will be created automatically, we also need the flag `-d` to indicate us want to make directory and the location of this temporary directory.

![Desktop View](assets/img/posts/2025-08-14-bandit-overthewire-bandit11-and-bandit12-writeup/bandit12-to-bandit13-fourth-step.png){: width="750"}

Okay, done, but ngl, I still don't quite understand what's the task want us to search for, so let me ask ChatGPT to clear it up.

![Desktop View](assets/img/posts/2025-08-14-bandit-overthewire-bandit11-and-bandit12-writeup/bandit12-to-bandit13-fifth-step.png){: width="750"}

![Desktop View](assets/img/posts/2025-08-14-bandit-overthewire-bandit11-and-bandit12-writeup/bandit12-to-bandit13-sixth-step.png){: width="750"}

![Desktop View](assets/img/posts/2025-08-14-bandit-overthewire-bandit11-and-bandit12-writeup/bandit12-to-bandit13-seventh-step.png){: width="750"}

Nicely explained, so I need to first convert the binary file using hexdump command `xxd`, use `file` command to confirm the format of the file, decompress the file based on the compression format, and keep going until we see a human-readable format. Gotcha, got a work to do then, this will be long lol.

But just to be clear, I want to confirm command `xxd` first from it's **man page**.

![Desktop View](assets/img/posts/2025-08-14-bandit-overthewire-bandit11-and-bandit12-writeup/bandit12-to-bandit13-eighth-step.png){: width="750"}

Okay, the explanation literally so concise and clear, well then let's begin then.

![Desktop View](assets/img/posts/2025-08-14-bandit-overthewire-bandit11-and-bandit12-writeup/bandit12-to-bandit13-ninth-step.png){: width="750"}

![Desktop View](assets/img/posts/2025-08-14-bandit-overthewire-bandit11-and-bandit12-writeup/bandit12-to-bandit13-tenth-step.png){: width="750"}

Oops, I got stuck at `tar` command, I forgot about this compression type, lemme check on internet what's is this compression format again. 

![Desktop View](assets/img/posts/2025-08-14-bandit-overthewire-bandit11-and-bandit12-writeup/bandit12-to-bandit13-eleventh-step.png){: width="750"}

Ah yes, the archiving thingy in linux, I remember it needed flag to specify that we want to do extraction, okay then, continue the work again.

![Desktop View](assets/img/posts/2025-08-14-bandit-overthewire-bandit11-and-bandit12-writeup/bandit12-to-bandit13-twelveth-step.png){: width="750"}

Nicely done, we got password the next level, level 13. Well, I really am learning a lot from this alone, such a nice challenge to refresh my knowledge in linux. I learn simple encryption, simple reverse engineering, different type of compression, file types and many more regarding files. 

Thank you again for OverTheWire team for this challenge, see you on the next challenge again.