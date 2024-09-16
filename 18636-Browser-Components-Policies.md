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

JavaScript from one origin cannot read from or write to the storage belong to another origin.

### SOP for network access

Different between writes (sending information) vs reads (accessing information)

Cross-origin writes (sending information)  are allowed

- Form submission
- Redirect

Cross-origin reads are generally not allowed

- script from a.com cannot inspect information loaded (response) from b.com

### Resource Embedding

Cross-origin embedding is typically allowed

JavaScript with `<script src="B"> </script>`

- B has origin A if loaded by A
- Can access A's dom, local storage,etc.

CSS with `<link rel="stylesheet" href="...">`

Images with `<img>`

Media files with `<video>` and `<audio>`

Plug-ins with `<object>`, `<embed>` and `<applet>`

anything with `<frame>` or `<iframe>`

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
   2. add to`.htaccess`

For example: 
```php+HTML
<IfModule mode_header.c>
Header set Access-Control-Allow-Origin "*"
</IfModule>
```

2. Nginx

   1. using the headers core module
   2. E.g., `add_header Access-Control-Allow-Origin "*"`

## SOP and CORS with attacks

**Why SOP and CORS cannot prevent XSS and CSRF attack?** 

Attacker injects code into victim's page

Injected code are considered to have the origin of victim page

Cross origin writes are allowed

<img src="3.JPG" width="50%" height="50%">

## CSP (Content Security Policy)

Content Security Policy (CSP) is a security measure designed to protect websites from malicious content injections like XSS attacks. By default, CSP disables risky behavior such as inline scripts and `eval()`, but it can be customized based on specific requirements.

### Key Aspects of CSP:

- **Explicit Allow List**: CSP defines a whitelist of trusted domains for cross-origin resources, controlling what can be loaded and from where.
- **Cross-Origin Interaction**: Specifies how the site's content interacts with resources from other origins, enhancing security by limiting external resource use.

### CSP Directives:

- URI Directives

  : Control specific content types such as:

  - `script-src`, `image-src`, `object-src` (restrict loading of scripts, images, objects, etc.)
  - `frame-ancestors` (limits embedding of the site)

- **Violation Reports**: CSP allows reporting violations of the policy, enabling developers to monitor and address security breaches.

### Level 3 Enhancements:

CSP Level 3 introduces improvements in directive flexibility, reporting capabilities, and stricter security controls.

### CSP implementation

Through HTTP response header: 

<img src="4.jpg" width="50%" height="50%">

Or, Instead of HTTP response header, it can also be include on a page directly

`<meta http-equiv="Content-Security-Policy" content="default-src">`

### CSP Example

- Allow everything but only from the same origin: `default-src 'self'`
- Only Allow Script from the same origin: `script-src 'self'`
  - Note that inline script are not considered to be from self
- Only Allow trusted Scripts with a specific nonce: `script-src 'stric-dynamic' 'nonce-R4nd0m'`
  - ex: `<script nonce="r4nd0m" src="//good.come/good.js">`
- Allow Google Analytics, Google AJAX CDN and same origin: `script-src 'self www.google-analytics.com ajax.googleapis/com'`

- Starter Policy: 
  - This policy allows images, scripts, AJAX and CSS from the same origin, and does not allow any other resources to load, such as object, frame, media, etc. It's a good starting point for many sites.
  - `default-src 'none'; script-src 'self'; connect-src 'self'; img-src 'self'; style-src 'srlf';`

### CSP: Pros and Cons

#### pros

Better Security

Backward compatible, so amenable to incremental deployment

Native browser support

#### cons

Header integrity

- CSP policies are delivered through HTTP headers, which means the integrity of these headers is crucial. If the headers are modified or intercepted by network attacker, the security provided by CSP could be compromised. Some browser extensions have the ability to modify HTTP headers, including CSP headers too. 

CSRF

- CSP primarily targets content injection attacks like XSS but doesn’t directly address CSRF. 



## SRI (Subresource Integrity)

SRI can Controlling which script to load and allows browsers to verify that fetched resources (e.g., from a CDN) have not been altered. This prevents attackers from injecting malicious content into files hosted on external sources.

- **How It Works**:

  - The integrity of a resource is verified by including a cryptographic hash of the file in the `integrity` attribute.
  - Allowed hash algorithms: `sha256`, `sha384`, and `sha512`.

- **Example Usage**:

  ```html
  <script src="https://example.com/example-framework.js"
  integrity="sha384-oqVuAfXRKap7fdgcCY5uykM6+R9GqQ8K/uxy9rx7HNQlGYl1kPzQho1wx4JwY8wC"
  crossorigin="anonymous"></script>
  ```

- **Cross-Origin Considerations**:

  - For cross-origin requests, the resource must be served with the appropriate CORS (Cross-Origin Resource Sharing) headers.

- **SRI vs CSP 'strict-dynamic'**:

  - **SRI** ensures the integrity of individual external scripts by validating their hashes.

  - **CSP strict-dynamic** dynamically adjusts which scripts are allowed to execute based on a trusted nonce value.

  - Example of CSP with strict-dynamic and  nonce:

     ```html
    script-src 'strict-dynamic''nonce-R4nd0m'
    <script nonce="r4nd0m" src="//good.com/good.js"></script>
    ```
