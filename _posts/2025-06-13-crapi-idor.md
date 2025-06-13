---
title: "Lab 1 — Insecure Direct Object Reference (IDOR) in OWASP crAPI"
description: "Step-by-step walkthrough of exploiting an IDOR vulnerability in OWASP crAPI, complete with Burp Suite traffic, Docker tips, and Mermaid diagrams."
date: 2025-06-13T12:00:00+05:30   # original publish time
lastmod: 2025-06-13T12:00:00+05:30 # update this whenever you revise
slug: "lab1-idor-owasp-crapi"       # SEO-friendly URL segment
categories:
  - crAPI
  - Labs
  - API Security
tags:
  - OWASP
  - crAPI
  - IDOR
  - API-Security
  - Burp Suite
  - Docker
  - Linux
  - Vulnerable-API
toc: true       # auto-generate table of contents
mermaid: true   # enable Mermaid rendering plugin/shortcode
draft: false    # change to true while still writing
---

### 🔍 Architecture Overview


<div class="mermaid">
sequenceDiagram
    participant Attacker
    participant crAPI_Frontend
    participant crAPI_Backend
    participant Database

    Attacker->>crAPI_Frontend: Login as user1
    crAPI_Frontend->>crAPI_Backend: GET /users/1001/profile
    crAPI_Backend->>Database: SELECT * FROM users WHERE id=1001
    Database-->>crAPI_Backend: Return user1 data
    crAPI_Backend-->>crAPI_Frontend: 200 OK (user1 profile)

    Note over Attacker: Attacker modifies the ID

    Attacker->>crAPI_Backend: GET /users/1002/profile
    crAPI_Backend->>Database: SELECT * FROM users WHERE id=1002
    Database-->>crAPI_Backend: Return user2 data
    crAPI_Backend-->>Attacker: 200 OK (user2 profile)
</div>

## Step-by-Step Guide: Exploiting IDOR in OWASP crAPI

## Step 1: Login as a Regular User

Register a new user (user1@example.com) and log in.

and if you are new in this web or first time add your vichle number and pin code which you got in your MailHog

![cr-api Image](/assets/van.png)
