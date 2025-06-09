---
title: "TryHackMe - Net Sec Challenge Walkthrough"
date: 2025-06-08 12:00:00 +0530
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