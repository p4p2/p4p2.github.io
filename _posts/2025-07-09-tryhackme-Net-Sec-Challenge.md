---
title: "TryHackMe - Net Sec Challenge Walkthrough"
date: 2025-06-09 12:00:00 +0530
categories: [TryHackMe, CTF]
tags: [tryhackme, ctf, walkthrough, Net-Sec-Challenge, hacking,nmap,telnet,linux,ssh,hydra]
toc: true
---

---
![net-sec Image](/assets/net-sec.png)

###  Summary: This walkthrough to test your mastery of the skills you have acquired in the Network Security module. All the questions in this challenge can be solved using only nmap, telnet, and hydra. This workthrough you can learn this all tools and about ports and sevices also you will learn how to do password attacks.



##  Task 1: we have to start the vm and the attack box. 

if you are using the openvpn you can connect with the try hackme server and use in your local machine as a attacking macine don't need to use your attck box.

## Task 2: What is the highest port number being open less than 10,000?

so for find port numbers we have to run nmap and we will run nmap -sT -sV -p1-10000 10.10.149.59.

![net-sec Image](/assets/net-sec-nmap.png)

Answer: 8080

## Task 3: There is an open port outside the common 1000 ports; it is above 10,000. What is it?

Now we have to find that port which is above 10,000 for that we have to  scan all ports (65535 ports) so i will use -p- flag in nmap.

nmap -sS -sV -T4 -vv -O 10.10.149.59 -p- 

![net-sec Image](/assets/net-sec-nmap2.png)

Answer: 10021

## Task 4: How many TCP ports are open?

if you see the nmap above nmap result you can see how many ports are open 

Answer: 6

## Task 5: What is the flag hidden in the HTTP server header?

for find the flag now we have to use the nmap script --script=http-headers.

![net-sec Image](/assets/net-sec-flag.png)

you can see  we got the flag.

## Task 6: What is the flag hidden in the SSH server header?

For ssh sever header flag we have to scan port 22 by nmap and we use here nmap script --script=ssh2-enum-algos

![net-sec Image](/assets/net-sec-ssh.png)

Answer: THM{946219583339}

## Task 7: We have an FTP server listening on a nonstandard port. What is the version of the FTP server?

if you go 2nd mass scan of nmap result you can see the ftp version.

Answer:  vsftpd 3.0.5

## Task 8: We learned two usernames using social engineering: eddie and quinn. What is the flag hidden in one of these two account files and accessible via FTP?

we got two usernames by social engineering and we know that this two username of ftp's but we dont have password,so for password we will use hydra tool.

hydra -l eddie -P /usr/share/wordlists/rockyou.txt ftp://10.10.149.59:10021 

![net-sec Image](/assets/net-sec-hydra.png)
![net-sec Image](/assets/net-sec-hydra1.png)

ok you can see the passwords

login: eddie   password: jordan

login: quinn   password: andrea


now we have to find the flags lets go and login we have to check where we can get the flag.

Nothing was discovered in “Eddie’s” account.

![net-sec Image](/assets/net-sec-ftp-flag.png)
 you can see i have used lftp -p 10021 -u quinn,andrea 10.10.149.59

 if you want to use ftp you can

 Answer: THM{321452667098}

## Task 9: Browsing to http://10.10.149.59:8080 displays a small challenge that will give you a flag once you solve it. What is the flag?

last and final flag we have to discover but for that we have to go  http://10.10.149.59:8080 and here is written Your mission is to use Nmap to scan 10.10.149.59 (this machine)
as covertly as possible and avoid being detected by the IDS. if you know how to use nmap flgas and all flags you heard about null flag -sN so we will use it.

![net-sec Image](/assets/net-sec-web.png)

![net-sec Image](/assets/net-sec-last.png)

Answer: THM{f7443f99} 

## Final Thoughts

The room effectively reinforces core concepts:

 1. Port scanning (normal & stealth modes)

 2. Service enumeration (Nmap scripts, headers)

 3. Brute-force FTP credentials with Hydra

 4.   Importance of stealth in evading IDS using null scans

It's a solid, practical exercise tying together key tools and techniques from the Network Security module.