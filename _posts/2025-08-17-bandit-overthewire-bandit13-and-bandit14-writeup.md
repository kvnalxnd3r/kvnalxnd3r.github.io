---
title: Bandit OverTheWire Writeup, Bandit11 & Bandit12
date: 2025-08-17 18 :00 +0700
categories: [Linux, Bandit OverTheWire, Learning]
tags: [linux, linux command, learning, os, writeup]
---

## Level 13 → Level 14

Another challenge for today, so let's try solving it, here's the challenge.

![Desktop View](assets/img/posts/2025-08-17-bandit-overthewire-bandit13-and-bandit14-writeup/bandit13-to-bandit14-task.png){: width="750"}
_Level 13 Task_

Now, we being tasked to use SSH key instead of the usual password that we have been using so far. Interesting, let's read what is the reference being cited in the task about. I also curious what is the deal with this whole "key" thingies.

![Desktop View](assets/img/posts/2025-08-17-bandit-overthewire-bandit13-and-bandit14-writeup/bandit13-to-bandit14-first-step.png){: width="750"}

Now I see why it's more secure using the keys instead of password. It's because only us and the other side that knows it and it's because of the agreement being made upon making this key using the certificates we able to communicate each other and even more securely. If you guys wanted, you could read more about this SSH key [here](https://help.ubuntu.com/community/SSH/OpenSSH/Keys).

Well, I know that it's a substitute for plain password, my instinct tells me that this will be combined with SSH command, but I just forgot the command for it, so I just searched it on the internet and here's the result.

![Desktop View](assets/img/posts/2025-08-17-bandit-overthewire-bandit13-and-bandit14-writeup/bandit13-to-bandit14-second-step.png){: width="750"}

Ah okay gotcha, but now, before I'm about to connect to `bandit14`, I have to check the password for the `bandit14` first just to be sure.

![Desktop View](assets/img/posts/2025-08-17-bandit-overthewire-bandit13-and-bandit14-writeup/bandit13-to-bandit14-third-step.png){: width="750"}

Okay, it's there, let's get into `bandit14`, use this command.

```bash
ssh -i ssh.key -p 2220 bandit14@localhost
```

Because we already on the `bandit` machine and **"localhost being a hostname that refers to the machine you are working on"** that is why we only need `localhost`. Okay, this should made us get into `bandit14` now.

## Level 14 → Level 15

![Desktop View](assets/img/posts/2025-08-17-bandit-overthewire-bandit13-and-bandit14-writeup/bandit14-to-bandit15-task.png){: width="750"}
_Level 14 Task_

If you guys wanted, you could read the references listed there in with this link below, but for me because I'm coming from Network Engineer background and feel I have solid foundation of networking in general, I will just skip this, as I mainly here for strengthening my Linux skill.
1. [How The Internet Works in 5 Minutes](https://www.youtube.com/watch?v=7_LPdttKXPc)
2. [IP Address](https://www.youtube.com/watch?v=7_LPdttKXPc)
3. [IP Address on Wikipedia](https://en.wikipedia.org/wiki/IP_address)
4. [Localhost on Wikipedia](https://en.wikipedia.org/wiki/IP_address)
5. [Ports](https://en.wikipedia.org/wiki/IP_address)
6. [Port (computer networking) on Wikipedia](https://en.wikipedia.org/wiki/IP_address)

Okay, without anymore delay, we will just `cat` the password from the known directory before that is, `/etc/bandit_pass/bandit14` and here's the result.

![Desktop View](assets/img/posts/2025-08-17-bandit-overthewire-bandit13-and-bandit14-writeup/bandit14-to-bandit15-first-step.png){: width="750"}


And.. bingo! We got it, so let's use this password whenever we need to connect to bandit14 but quick reminder, you have to **logout** `bandit14` and `bandit13` or you will get this error if you do that.

![Desktop View](assets/img/posts/2025-08-17-bandit-overthewire-bandit13-and-bandit14-writeup/bandit14-to-bandit15-second-step.png){: width="750"}

So next step is to log out like picture below.

![Desktop View](assets/img/posts/2025-08-17-bandit-overthewire-bandit13-and-bandit14-writeup/bandit14-to-bandit15-third-step.png){: width="750"}

After logged out, we then try the password previously obtained and connect back to `bandit14`. And I tried to use port `30000` like being told, but I got no response and I just terminate the process and try using port `2220`, as you can see, it worked like a charm.

![Desktop View](assets/img/posts/2025-08-17-bandit-overthewire-bandit13-and-bandit14-writeup/bandit14-to-bandit15-fourth-step.png){: width="750"}

And that's it for the challenge today, it mainly focusing on the SSH itself
