---
title: Browser Security Introduction
date: 2024-09-03 20:38:28
categories: 学习笔记
tags: 
- Browser Security
- 18636
- CMU
---

This Note contains: 

- Background
- Threat Model
- Browser Internals
- Most common web attacks

<!-- more -->
<!-- toc -->

## Background

1. www

   Invented by Tim Berners Lee at CERN in 1989

   Originally meant for automatic information-sharing between scientist in universities and institutes around the world

   The first page was hosted on Berbers Lee's computer

   Software made public in 1993

2. Line-mode browser

   It's almost the first browser, launched in 1992.

3. Timeline: 

   1993:  Mosaic browser, IMG tag proposed, Hypertext Markup Language (HTML) first draft

   1994: Netscape Navigator released

   1995: PHP, IE v1.0 and JavaScript

   1996: Cascading Style Sheets

   1997: Netscape Navigator 4.04 : first modern browser

   2000s: Ajax, flash

   today: chrome, Firefox, safari, etc.

   <img src="2.JPG" width="50%" height="50%">

4. Ajax: Asynchronous JavaScript and XML

   Dynamic display and interaction using the document Object Model

   Data interchange and manipulation using XML

   Asynchronous data retrieval using XMLHttpRequest

   JavaScript binding everything together

	<img src="1.JPG" width="30%" height="30%">

5. Browser is executing unknown code from any website, which can cause issue about integrity, confidentiality and privacy.
   1. Compromise your machine or install a malware rootkit
   2. Steal passwords or read your information
   3. track user browsing behavior

## Feature vs security

Arm race between adding  new features and ensuring security

Cool new features are introduced and adopted before fully tested

New features: 
 	1. widen the attack surface 
 	2. may interact badly with existing protection mechanisms

## Threat Model

### Network attacker

Sit between Alice and Bob

Eavesdrop

**Intercept, Alter and Inject** messages

<img src="3.JPG" width="50%" height="50%">

> Typically. Encryption is the way to protect the information in this threat model. Without the key, even the attacker get the package sent in network, he can't read or change it.  
>
> That's what HTTPS did.

### Web attacker

Own a website

Talks to Alice directly at the same time Alice is talking to Bob

It can do anything an web application can do.

<img src="4.JPG" width="50%" height="50%">

### 3rd party attacker

Attacker is affiliate of Bob

Serve content on Bob's page, such as Ads, google analytics, social media widget

<img src="5.JPG" width="50%" height="50%">

### Extension attacker

Browser extension have special APIs to modify browser functionalities

E.g, can inject script into the page, alter header information, etc.

<img src="6.JPG" width="50%" height="50%">

### Comparing threat models

Network attacker is typically stringer than web attacker over HTTP (Situation changes with HTTPS). 

3rd Party attacker can be deceptive, it's hard to distinguish 1st party vs 3rd party content. 

Extension attacker is stronger than 3rd party attacker because the browser extension exposed to some APIs which is more powerful than what webpage can do.

## Browser Internals

The Main functionalities for a browser are :

1. Request recourses
- Based on Uniform Resource Identifier (URI)
- Either on address bar or from a page

2. Present resources
- HTML, CSS, JavaScript, JPEG, GIF, PNG, PDF, Videos, ...
- MIME type tells browser how to interpret data

3. Interact with the server
- Post forms
- Set/send cookies

4. Managing states
- Bookmarks, History, Password , send cookies

### High-level structure

**The user interface**

​	address bar, back/forward button, bookmarking menu...

**The rendering engine**

​	parses HTML and CSS and displays the parsed content on the screen

**Networking**

​	data request and transfer: http, https

**JavaScript interpreter**

​	parse and execute JavaScript code

**Data storage**

​	Cookies, LocalStorge, FileSystem...

**The browser engine**

​	marshals actions between the UI  and the rendering engine

### Chromium

<img src="7.JPG" width="60%" height="60%">

For every tab, there is a isolate renderer process to prevent any malware.

In every renderer process, it has its own JavaScript engine (V8) which means the code from different page are running in isolate environment which relies on different process in OS level . 

### Rendering

<img src="8.JPG" width="60%" height="60%">

1. Process HTML markup and build the DOM tree
2. Process CSS markup and build the CSSOM tree
3. Combine the DOM and CSSOM into a render tree
4. Run layout on the render tree to compute geometry of each node
5. Paint the individual nodes to the screen

### Event processing

**Events**: onclick, focus,...

**Event Handlers**: Code that run when events are fired E.g. onclick, onfocus, onblur, onketdown...

**Event Loop**:

​	Events are processed one at a time.

​	A single thread for the main event loop, Concurrent worker threads cannot have UI events.

**Event Order**:  Capture and Bubble

#### DOM Event Dispatch Phase

Example: html, body, button all have onclick event handlers. In which order are these handlers fired? 

<img src="9.JPG" width="30%" height="30%">

1. **Capture**

Propagate the event from top down

***html, body***

2. **Target**

The innermost element that trigger the event

***button***

3. **Bubble**

Propagate the event from bottom up

***body, html***

## Most common web attacks: XSS and CSRF
### XSS

Cross Site Scripting

Attacker owns www.attacker.com

Victim goes to www.goodsitecom/...

Script crafted by attacker is executed by the browser

Scripts now is considered to have the privilege of goodsites.com and send data to attacker www.attcker.com

Different types of XSS differs in how the scripts are generated: **Stored**, **Reflected**, **Dom-based**

Reflected XSS: 

<img src="10.JPG" width="50%" height="50%">

### CSRF

Cross-site request forgery

Attacker owns attacker.com

Attacker tricks user to browse attack.com

Browser sends request to bank.com when rendering the page served by attcker.com, such as \<img src="https://bank.com/...">

Browser is called the ***confused deputy***

<img src="11.JPG" width="50%" height="50%">

#### Defending against CSRF

1. Browser sends the referrer header, server decides whether to honor the request  or not
   1. Header is not reliable
   2. CORS policy
2. Browser don't sent cookies
   1. samesite cookies

