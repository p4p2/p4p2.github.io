---
title: "Installing OWASP crAPI on Linux for API Security Testing"
date: 2025-06-13 12:00:00 +0530
categories: 
  - crAPI
  - Labs
  - API Security
tags: [owasp, crapi, api-security, hacking, docker, linux, vulnerable-api, pentest-lab]
toc: true
---

## 🧪 Introduction

Modern web applications rely heavily on APIs, and with that comes the need to rigorously test them for vulnerabilities. The OWASP crAPI (Completely Ridiculous API) is a deliberately vulnerable API designed for learning and practicing API security testing.

In this blog, we’ll walk through how to install OWASP crAPI on a Linux machine and start using it for security testing.

---

## What Is OWASP crAPI?

OWASP crAPI is a real-world style vulnerable API developed by the OWASP community. It mimics an online car-related service, complete with user registration, authentication, and functionality such as updating personal details, VIN lookup, and more.

It's ideal for practicing:

    Broken authentication

    Broken object-level authorization (BOLA)

    Mass assignment

    API misconfigurations

    Security testing tools like Burp Suite, OWASP ZAP, and Postman


## 🛠️ 🖥️ System Requirements

Before installing crAPI, ensure you have:

    A modern Linux distribution (Ubuntu/Debian recommended)

    Docker and Docker Compose installed

    Internet connection (to pull Docker images)


## 🐳 Step 1: Install Docker and Docker Compose

If not already installed:

    # Update your system
    sudo apt update && sudo apt upgrade

    # Install Docker
    sudo apt install docker.io -y

    # Enable and start Docker
    sudo systemctl enable docker
    sudo systemctl start docker

    # Install Docker Compose
    sudo apt install docker-compose -y

## Verify installation:

    docker --version
    docker-compose --version

## 📦 Step 2: Clone the OWASP crAPI Repository
Clone the official repository from GitHub:

    git clone https://github.com/OWASP/crAPI.git
    cd crAPI
    cd deploy
    cd docker


## ⚙️ Step 3: Configure and Start crAPI

OWASP crAPI uses Docker Compose to spin up all necessary containers:

    docker-compose up -d


## This command will:

    Pull the necessary Docker images

    Spin up containers for the API, frontend, and supporting services (e.g., database)

To verify everything is running:

    docker ps


## 🌐 Step 4: Accessing crAPI

Once the containers are up, visit:

    Frontend: http://localhost:8888

    API Docs (Swagger): http://localhost:8888/swagger

The app simulates a car-related web service. Try registering a new account and explore its functionality.

![cr-api Image](/assets/crapi.png)



## Step 5: Begin API Security Testing

Use tools like:

    Burp Suite: Intercept traffic and identify broken object-level access

    OWASP ZAP: Scan the API and identify common vulnerabilities

    Postman: Send crafted API requests manually

## Start exploring:

    Broken Auth

    Mass Assignment

    Insecure Direct Object References (IDOR)

    Misconfigured JWT tokens

## 🛑 Troubleshooting Tips

    Port already in use? Edit docker-compose.yml to change exposed ports.

    Docker errors? Try docker-compose down and then up -d again.

    Access issues? Ensure firewall rules aren’t blocking port 8888.

## 🔐 Warning: Use Responsibly

crAPI is deliberately vulnerable. Never expose it to the internet or run it on a production machine. Use only in a secure, isolated lab environment.

## ✅ Conclusion

Installing OWASP crAPI on Linux is straightforward with Docker. It’s an excellent tool for learning about API vulnerabilities in a safe, controlled environment. Whether you’re preparing for bug bounty hunting or studying for a certification like OSWE, crAPI provides hands-on experience with real-world API issues.