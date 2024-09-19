---
title: 'Browser Architecture'
date: 2024-09-16 11:13:21
categories: 学习笔记
tags: 
- Browser Security
- 18636
- CMU
---

This Note contains: 

- Security problems and mitigation strategies
- 

<!-- more -->
<!-- toc -->

Recall history of the web

By 2000s the web is fully dynamic

Ajax: Async JavaScript and XML

Plugin: 



Modern browser: 

Browsers Maintain a complex distributed execution environment for programs from the web

Infrastructure is too old. Same Software architecture designed for browsing static pages

Patches and more patches



Monolithic browsers: 

No architectural help for security

Components often have full privileges of user

No isolation: Failure/insecurity of one component affects other components

Not following least privilege principle.



CVE of Microsoft Internet Explorer (IE)

A dew consecutive CVEs among a very long list of them.



CVE of Mozilla Firefox.



Examine two proposals + one real browser
OP, Chrome, Gazelle



Secure web browsing with the OP web browser

Goal: Limit the damage of web attacks

Approach: 
OS design principles 
Formal method for verification
Auditing



OP Design

Decompose monolithic design into browser subsystems

Run in separate OS-level processes

Use messages passing

All message through browser kernel

Components are monitored

Host OS sandboxing

SELinux

Modified Linux kernel that enforces MAC



Web Instance

Communication through the browser kernel
HTML rending engine and the plugins communicate directly with a vnc server

Svnc server renders the elements locally

Design enables security
Partitioning and constrained communication enable new security mechanisms
Clean separation of browser functionality and security

Use a type safe language to reduce the chance of memory correction attacks
policy
plugin security policies
Formal methods

Certify SOP + URL address bar invariant under the assumption that a component is compromised

Plugin
Each plugin runs in its own process

Security policies: 
Provider domain policy
embedded plugin has the origin of content provider's domain, not the embedding page's domain

Freedom policy
can access any network resource but gibe up DOM and local storage access



Model checking OP design



Define transition rules for the major events

A transition rule

An execution trace

Define properties of traces

Ask Model checker to check properties

From initial state, can the system reach a state where the URL is updated by an attacker? 

 

Design vs implementation

Suppose we prove that URL bar cannot be modified by an attacker for the model

Is OP browser secure against URL bar spoofing attacks

It depends: 
Model is true to implementation: Does not miss key details
Implementation is correct w.r.t the model

Implementation transition steps wrong, e.g., has a buffer overrun



What about we verified the source code of OP? 
the complier could have errors and generate buggy machine code

Logs for auditing attacks
Build dependency graphs to reveal causal relation
Audit algorithm traverses the graph and find relevant slices
Challenges: Too much data or too little data



Pros: 
partitioning and constrained communication enable new security mechanisms
clean separation of browser functionality and security
Use a  type safe language to reduce the chance of memory correction attacks
Policy: Plugin security polices
Formal methods
Audit

Cons: 
Complexity, performance
Formal verification is not on source code
Audit can be difficult

Chromium
Modularize Browser
Don't run all parts of browser with full privileges
Some parts more likely to be hacked than others
Use Privilege separation
Limit impact of main exploits
Divided

**********D*SD*SD*



Chrome Arch
Modular design
sandbox

Render engine: 
Render HTTP responses into bitmaps
Parse HTML, CSS, SVG, XML, etc.

Manage of cookies\, history, cache
Network stack
File System

Enforce xxxxx

SandBox
Goal: What runs inside of sandbox can't affect outside world, exxcept via exposed API
Block access to all objects, resources
Allow certain System calls

Approach: 
Start process establish inter process 
Xxxxx


Plugin: 

Pose a Dilemma: widely used, but not under browser's control
in browser kernel? No: Bad for reliability and security

In rendering engine to be sandboxed? 
can't do it: not compatibility with existing plug-ins
At most one plug-in instance per browser

Put in its own process, one per plug-in type, has full user privilege

xxx

xxx





Security evaluation
Look at past vulnerabilities in other browsers.

Results: Modular design can only mitgate certain attacks
Vulnerabilitirs caused by arbitrary code injectuoin in rendering engines

can be mitigated

Insufficient validation of system calls to browser kernel

cannot be mitigated by sandboxing



XXE (XML eXternal Entity): cause file theft

Is only prevented after modifying browser kernel to block request  to file://



