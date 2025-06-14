---
title: "Lab 1 — Insecure Direct Object Reference (IDOR) in OWASP crAPI - p4p2"
description: "Step-by-step walkthrough of exploiting an IDOR vulnerability in OWASP crAPI, complete with Burp Suite traffic, Docker tips, and Mermaid diagrams."
date: 2025-06-14 12:00:00 +0530

categories: 
  - crAPI
  - Labs
  - API Security
tags: [owasp, crapi, api-security, hacking, docker, linux, vulnerable-api, pentest-lab]
toc: true
  
---

### 🛠️ What Is IDOR?

Insecure Direct Object References (IDOR) are a type of access control vulnerability that occurs when user-supplied input is used to access objects (like files, user profiles, or records) directly without proper authorization checks.

While the term IDOR was first highlighted in the OWASP Top 10 (2007), it remains a widespread issue. IDOR typically results in horizontal privilege escalation (accessing another user's data), but it can also lead to vertical escalation (gaining higher privileges).

## Step-by-Step Guide: Exploiting IDOR in OWASP crAPI

##  🚀 Getting Started with crAPI

To follow along, you should have OWASP crAPI running locally via Docker.

💡 Note: If you're unsure how to set up crAPI with Docker, check out my [crAPI installation blog post](https://p4p2.github.io/posts/crapi-install/)

Once crAPI is up and running:

register a new user (user1@example.com)

Retrieve the PIN code from MailHog

Log in and register a vehicle

![cr-api Image](/assets/van.png)

### 🔎 Step 1: Find the Vulnerable Endpoint

Once logged in, I explored the frontend and found an interesting feature called Contact Mechanic.

![cr-api Image](/assets/idor-mechanic.png)

After submitting the form with some test data, I checked the Burp Suite HTTP History and found a suspicious POST request.


![cr-api Image](/assets/idor-link.png)

In the response, I noticed a link—this is our lead.


### 🎯 Step 2: Exploit the IDOR



![cr-api Image](/assets/idor-link1.png)

When you copy and paste the link from the response into a new browser tab, you’re greeted with a message:

  "JWT Token required!"

That’s expected—the endpoint requires authentication.

Now let’s exploit this using Burp Suite Repeater:

Capture the request with Burp

Add your valid JWT token from a logged-in session

Change the userId parameter to another user's ID (e.g., from 1001 to 1002)

Resend the request

![cr-api Image](/assets/idor-burp.png)

💥 Boom! By simply modifying the ID, we’ve accessed another user’s data.


🚨 What's the Root Cause?

Even though the endpoint checks for a valid JWT, it doesn't validate ownership of the requested object.

    ⚠️ JWT tokens handle authentication, not authorization. If the backend doesn’t enforce access controls based on the token’s subject (e.g., user ID), IDOR becomes possible.


🔒 Security Best Practice

✅ Mitigation: Always enforce object-level authorization on the backend. Never trust user-supplied IDs in URLs or payloads. Instead:

    Use the user ID from the JWT claims or session context

    Ensure the authenticated user has permission to access the requested resource

### 🔁 Bonus: Another IDOR in crAPI

In the previous section, we exploited an IDOR by modifying a `report` parameter. Now, let’s explore another IDOR—this time involving vehicle location tracking.

## 🗺️ Finding the Vulnerable Endpoint

On the homepage, there’s a **Map section** (see the image avobe 2nd). Try refreshing that map.

When the map reloads, Burp Suite catches a suspicious GET request.

It's like crAPI is saying:
"Here, take this secret GPS data. You seem cool."

Spoiler: we're not that cool—we're just curious (and ethical).

![cr-api Image](/assets/idor-map.png)

you can see the GET request /identity/api/v2/vehicle/35962936-4b55-4441-91e0-f6d7e3eca99a/location and you can see the response 

This endpoint responds with sensitive data, including:

- Vehicle ID
- Current location coordinates
- Owner’s name
- Owner’s email

This information should definitely **not** be exposed to other users.

## 🧠 Thinking Like an Attacker

The response contains a `vehicleId`. If you're thinking like an attacker, your next question should be:

> *What if I change the `vehicleId` in the URL?*

That’s exactly what we’re going to try—but first, we need to **gather valid vehicle IDs** from other users.

--- 


## 🔍 Information Gathering

Let’s go to the Community section—because apparently, people just post their GPS-linked car IDs like it's Facebook in 2006.
Thanks for the free intel, Robot. You're really living up to the name.

![cr-api Image](/assets/idor-community.png)


![cr-api Image](/assets/idor-car-id.png)

After inspecting the traffic in Burp Suite, we find another `GET` request revealing full post details:

```json
"id": "i7zAtiDa2JLjydYcTmXZLN",
"title": "Title 3",
"content": "Hello world 3",
"author": {
  "nickname": "Robot",
  "email": "robot001@example.com",
  "vehicleid": "4bae9968-ec7f-4de3-a3a0-ba1b2ab5e5e5"
}

Bingo! Each post includes the author’s vehicle ID. This is another vulnerability known as:

🚨 Excessive Data Exposure — the API leaks more data than it should.

## 💥 Exploiting the IDOR

Now that we have another user's vehicleId, let’s copy it and go back to our previous map location request in Burp Suite.

Replace your own vehicleId in the URL with the new one, and resend the request.

![cr-api Image](/assets/idor-location.png)


And there it is—we’ve successfully accessed another user’s vehicle location!

🚗 Location unlocked.
We now know where someone else's car is parked.
(Don't worry, we’re not coming for your tires.)

## 🔒 Summary

This is a second, distinct IDOR vulnerability in crAPI, showing how:

    Lack of object-level authorization allows users to access data they shouldn't.

    Sensitive information (like vehicle ID) is exposed through unrelated features (community posts), enabling IDOR chaining.

Stay alert when testing APIs—IDORs often hide behind features that seem harmless.