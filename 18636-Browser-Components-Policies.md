---
title: 'Browser Components: Policies'
date: 2024-09-04 12:31:49
categories: 学习笔记
tags: 
- Browser Security
- 18636
- CMU
---

This Notes contains: 

- SOP
- CORS
- CSP
- SRI
- Attacks revisited
- Policy composition

<!-- more -->
<!-- toc -->

## SOP (Same Origin Policy)

The same origin policy restricts how a document or script loaded from one origin can interact with a resource from another origin.

- Generally same origin access are fully arrowed

**Key security mechanism: **

1. Providing isolation
2. script iframed AD cannot read passwords that user types.

**Resources:**

1. DOM
2. Cookies
3. Local Storage
4. Network request

### Origin

**Origin = scheme://host:port**

*Example:*

*http://cnn.com/sports/index.html*

*http://cnn.com/news/index.html*

- same

*http://cnn.com:81/sports/index.html*

- Different: port is different

*https://cnn.com/sports/index.html*

- Different: scheme protocol is different

*http://news.cnn.com/*

- Different: host is different

### SOP for DOM

Origin A can access origin B's DOM if match on (scheme, host, port)

This includes iframes

<img src="1.JPG" width="50%" height="50%">

### SOP for cookies

**Cookies: (complex access control polices)**

- Maintain server state: persistent authentication or preference management
- any server could return a short text token to be stored by the client in a Set-Cookie header, and the token would be stored by clients and included on all future request (in a Cookie header)

Generally speaking, based on: **([scheme], [host], *path*)**

Cookies can only be read by same origin script (if this is allowed)

Built-in domain relaxation using **Domain attribute**

- Example: host = login.cnn.com
  - Can set cookies with Domain: login.cnn.com or cnn.com
  - Disallowed: ad.cnn.com, nytimes.com, .com
- Cookies with Domain = cnn.com is sent to cnn.com and to all subdomains: login.cnn.com, ad.cnn.com

When Domain is not set, default to exact host

### SOP for Local Storage

Local Storage is separate origin



### SOP for network access

Different between writes (sending information) vs reads (accessing information)

Cross-origin writes (sending information)  are allowed

- Form submission
- Redirect

Cross-origin reads are generally not allowed

- script from a.com cannot inspect information loaded (response) from b.com

### Resource Embedding

Cross-origin embedding is typically allowed

JavaScript with \<script src="B">\</script>

- B has origin A if loaded by A
- Can access A's dom, local storage,etc.

CSS with \<link rel="stylesheet" href="...">

Images with \<img>

Media files with \<video> and \<audio>

Plug-ins with 



### Changing Origins?

Once upon a time: Domain relaxation by setting `document.domain`

- A page (s.y.com) can set its domain to it's superdomain
- JavaScript: `document.domain = y.com`

May 30, 2023, Google announced that the deprecation of `document.domain` setter will be effective in Chrome 115.

## CORS (Cross-Origin Resource Sharing)

AJAX cross-origin requests need to be handled with care

- HTTP GET/POST/PPUT/DELETE...
- Request may change the state of the server, require authentication...

Web developers want more freedom to load cross-origin resources, such as using amazon for backend or access google API

Browser support for CORS

- Implemented as (pore-flight headers)

- Browser inject CORS header to request to the server

- Browser interpret CORS header from the server to decide whether requests are allowed or not

**Simple vs pre-flighted requests**

<img src="2.JPG" width="50%" height="50%">

### Server-side set up

1. Apache

   1. user mod_header
   2. add to .htaccess

2. Nginx

   1. using the headers core module
   2. xxxxxxxxxxxxxxxxxxxxxxxxxxxxx

## SOP and CORS with attacks

**Why SOP and CORS cannot prevent XSS and CSRF attack?** 

Attacker injects code into victim's page

Injected code are considered to have the origin of victim page

Cross origin writes are allowed

<img src="3.JPG" width="50%" height="50%">

## CSP (Content Security Policy)

Instead of HTTP response header, CSP can be included on a page directly

\<>
