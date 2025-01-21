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
- Modern browser security issues
- Monolithic browsers and their issues
- Notable CVEs (Microsoft IE, Mozilla Firefox)
- Three proposals and a real browser example (OP, Chrome, Gazelle)
- Secure browsing design in the OP web browser
- Formal methods, auditing, and model checking
- Comparison with Chromium (Chrome) architecture

<!-- more -->
<!-- toc -->

## 1. History of the Web

- Recall History of the Web:
  - Initially, the web was a collection of static pages.
  - By the 2000s, the web became fully dynamic.
- Key Milestones:
  - **Ajax:** Asynchronous JavaScript and XML enabled dynamic content.
  - **Plugins:** Became common, though they introduced new challenges.

------

## 2. Modern Browser Context

- **Modern Browsers:**
  - Maintain a complex distributed execution environment for web programs.
  - Built on an outdated infrastructure originally designed for static content.
  - Rely on continuous patches and updates.
- **Security Challenges:**
  - Monolithic Design:
    - Lack of architectural support for security.
    - Components often run with full user privileges.
    - No isolation between components: a failure or vulnerability in one component can affect others.
    - Violates the least privilege principle.

------

## 3. Case Studies: CVE Examples

- Microsoft Internet Explorer (IE):
  - Notable series of consecutive CVEs among many.
- Mozilla Firefox:
  - Also has multiple documented CVEs.

------

## 4. Browser Design Proposals: OP, Chrome, Gazelle

### 4.1 Secure Web Browsing with the OP Browser

**Goal:**
Limit the damage of web attacks.

**Approach:**

- Employ OS design principles.
- Utilize formal methods for verification.
- Implement robust auditing mechanisms.

**Key Components of OP Design:**

- Modularization:
  - Decomposes the monolithic design into separate browser subsystems.
  - Runs subsystems in separate OS-level processes.
  - Communication is handled via message passing through a central browser kernel.
- Monitoring and Sandboxing:
  - The kernel monitors all inter-process messages.
  - Uses host OS sandboxing (e.g., SELinux) and a modified Linux kernel that enforces Mandatory Access Control (MAC).
- Web Instance Communication:
  - The HTML rendering engine and plugins communicate directly with a VNC server.
  - The VNC server renders elements locally, enhancing isolation.
- Security Benefits:
  - Partitioned design and constrained communication channels enable new security mechanisms.
  - Clean separation between browser functionality and security controls.
  - Implementation in a type-safe language reduces risks of memory corruption attacks.
- Security Policies:
  - For Plugins:
    - Provider Domain Policy: An embedded plugin inherits the origin of the content provider rather than the embedding page.
    - Freedom Policy: Allows plugins access to network resources at the expense of DOM and local storage access.

------

## 5. Formal Verification and Auditing

**Model Checking the OP Design:**

- Transition Rules:
  - Define rules for major events in the system.
  - An execution trace is constructed from these rules.
- Property Verification:
  - Define properties (e.g., ensuring an attacker cannot update the URL bar).
  - Use a model checker to verify if a state violating these properties is reachable from the initial state.

**Design vs. Implementation:**

- Even if the model ensures that the URL bar cannot be modified by an attacker, the real-world security of the browser depends on:
  - Faithfulness of the model to the implementation.
  - Accuracy of the implementation with respect to the model (e.g., avoiding buffer overruns in implementation steps).
  - Potential errors in the compiler that might generate buggy machine code.

**Auditing Attacks:**

- Use logs to build dependency graphs revealing causal relationships.
- Audit algorithms traverse these graphs to identify relevant attack slices.
- **Challenges:** Managing the volume of data (too much or too little).

------

## 6. Pros and Cons of the OP Design

**Pros:**

- Partitioning and constrained communication enable new security mechanisms.
- Clean separation of browser functionality and security.
- Use of a type-safe language reduces memory corruption attack risks.
- Implementation of plugin-specific security policies.
- Application of formal methods for verifying security invariants.
- Comprehensive auditing capability.

**Cons:**

- Increased complexity and potential performance overhead.
- Formal verification is conducted on models rather than the actual source code.
- Auditing can be challenging due to data volume and complexity.

------

## 7. Chromium (Chrome) Architectural Overview

**Key Design Principles:**

- Modular Design:
  - Separates components to prevent the entire browser from running with full privileges.
  - Implements privilege separation to limit the impact of potential exploits.
- Sandboxing:
  - Critical to ensure that what runs inside the sandbox cannot affect the outside world except through controlled APIs.
  - Sandboxing blocks access to most system objects and resources while allowing specific, controlled system calls.

**Major Components:**

- Render Engine:
  - Parses and renders HTTP responses into bitmaps.
  - Handles HTML, CSS, SVG, XML, etc.
- Network Stack, Cookie/History/Cache Manager, and File System:
  - Manage various browser functionalities while enforcing security policies.

**Plugin Handling:**

- Dilemma:
  - Plugins are widely used but not under the direct control of the browser.
- Common Approaches:
  - Running plugins in the browser kernel is avoided due to security and reliability concerns.
  - Rendering engines remain sandboxed for compatibility issues.
  - Typically, one plugin instance per browser, or plugins run in separate processes (often with full user privileges).

------

## 8. Security Evaluation

**Lessons from Past Vulnerabilities:**

- Modular Design Benefits:
  - Helps mitigate certain types of attacks.
- Limitations:
  - Vulnerabilities due to arbitrary code injection into rendering engines can still occur.
  - Some issues, such as insufficient validation of system calls to the browser kernel, cannot be fully mitigated by sandboxing alone.
- Example – XXE (XML eXternal Entity) Attacks:
  - Can lead to file theft.
  - Mitigation requires modifications to the browser kernel to block requests to `file://` URIs.
