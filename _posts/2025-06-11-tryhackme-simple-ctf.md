---
title: "TryHackMe - Simple CTF  Walkthrough"
date: 2025-06-11 12:00:00 +0530
categories: [TryHackMe, CTF]
tags: [tryhackme, ctf, walkthrough, enumeration, privilege-escalation, linux, nmap, telnet, ssh, simple CTF, beginner]
toc: true
---

![simple-ctf Image](/assets/simple-ctf.png)

###  Summary: In this walkthrough, we explore the "Simple CTF" room on TryHackMe — a beginner-friendly challenge designed to sharpen your fundamental cybersecurity skills. You'll learn essential enumeration techniques using tools like nmap, exploit common services such as Telnet and SSH, and perform basic Linux privilege escalation. This guide is perfect for those starting their ethical hacking journey or preparing for more advanced Capture The Flag (CTF) challenges.

## Task 1: How many services are running under port 1000?

First we run nmap to scan the 1000 ports on the target for this we run this nmap command  

{% include command-box.html cmd="nmap -sS 10.10.118.169 -v -O -T4 -F" %}


![simple-ctf Image](/assets/simple-ctf-nmap.png)

{% include answer-box.html ans="2" %}


## Task 2: What is running on the higher port?

Now we need to know about the service on higher port so we will run this command.

{% include command-box.html cmd="nmap -sS 10.10.118.169 -v -O -T4 -p- -sV" %}


![simple-ctf Image](/assets/simple-ctf-nmap2.png)

{% include answer-box.html ans="SSH" %}


we know that there is web hosted on this ip son we go to the ip address lets check what we will get

![simple-ctf Image](/assets/simple-ctf-webde.png)

we got default apche2 page.

But we need more information so we will run gobuster for directory enumaration.

![simple-ctf Image](/assets/simple-ctf-gobuster.png)

we got a directory name /simple lets check

![simple-ctf Image](/assets/simple-ctf-simple.png)

![simple-ctf Image](/assets/simple-ctf-simple1.png)

Here we can see this is a default page for something called “CMS Made Simple” and if we look in the bottom corner we can see it is version 2.2.8.

Now lets check any exploit we can get or not you can use searchsploit or you can use google search .


## Task 3:What's the CVE you're using against the application? 

![simple-ctf Image](/assets/simple-ctf-cve.png)

{% include answer-box.html ans="CVE-2019-9053" %}


## Task 4:To what kind of vulnerability is the application vulnerable?

{% include answer-box.html ans="SQLI" %}

Now lets download the exploit and run usally if you download a exploit and don't know about the syntax then you can see the source code you can get the syntax example

![simple-ctf Image](/assets/simple-ctf-exploit.png)

we run this command python2 exploit.py -u http://10.10.118.169/simple  --crack -w /usr/share/wordlists/rockyou.txt 

NOTE: if you are facing any issue when run this exploit on your local system then you can use the tryhcakme attack box.

## Task 5: What's the password?

![simple-ctf Image](/assets/simple-ctf-password.png)

{% include answer-box.html ans="secret" %}

## Task 6: Where can you login with the details obtained?

{% include answer-box.html ans="SSH" %}


Now we will use ssh for login in user mitch and get the flag

## Task 7: What's the user flag?

![simple-ctf Image](/assets/simple-ctf-flag.png)


{% include answer-box.html ans="G00d j0b, keep up!" %}


## Task 8: Is there any other user in the home directory? What's its name?

{% include answer-box.html ans="sunbath" %}

now we have to privileged escalation so like everyone i also start with sudo -i

![simple-ctf Image](/assets/simple-ctf-vim.png)

uf you see we can't access root but vim editor can access.

so we will go [GTFOBins](https://gtfobins.github.io/gtfobins/vim/) site and check vim

On the site, we can click the sudo button at the top and see if Vim is in the list. If so, then escalation (or some other shenanigans) are possible through the sudo enabled Vim.

![simple-ctf Image](/assets/simple-ctf-gtfo.png)

I used the first example (a), though used bash instead.


{% include command-box.html cmd="sudo vim -c ':!/bin/bash'" %}

## Task 9: What can you leverage to spawn a privileged shell?

{% include answer-box.html ans="vim" %}


## Task 10: What's the root flag?

![simple-ctf Image](/assets/simple-ctf-root.png)


{% include answer-box.html ans="W3ll d0n3. You made it!" %}


🏁 Conclusion

This CTF challenge highlighted the importance of imformation gather vulnerablity research and exploit the vulnerablity. After that all thing privileged exclation find vulnerablity on the os get root access. By leveraging resources like GTFOBins, we demonstrated how commonly available tools can be misused to gain unauthorized access when misconfigured.

In real-world scenarios, this emphasizes the need for strict access control, auditing installed binaries, and ensuring tools like vim are not run with elevated privileges unless absolutely necessary. As always in CTFs, lateral thinking and recon are key—sometimes the tools to escalate are hiding in plain sight.