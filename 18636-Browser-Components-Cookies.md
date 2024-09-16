---
title: 'Browser Components: Cookies'
date: 2024-09-16 08:20:21
categories: 学习笔记
tags: 
- Browser Security
- 18636
- CMU
---

This Notes contains: 

- Cookies
- How browser handle cookies

<!-- more -->
<!-- toc -->

## Cookies

A **cookie** is a small piece of data stored by a web browser that helps websites remember information about a user, such as login sessions, preferences, or tracking data. Cookies are sent between the browser and server with each request, allowing for continuity across sessions.

<img src="1.jpg" width="50%" height="50%">

### What Are Cookies Used For?

1. **Session Management**:

   - Cookies are commonly used to manage user sessions (e.g., keeping a user logged in).

   <img src="2.jpg" width="50%" height="50%">

2. **Personalization**:

   - Cookies store user-specific settings or preferences.
     - Example: `Shoppingcart=150`
     - Example: `Language=en`

3. **Tracking**:

   - Cookies help remember a user’s prior visits, enabling tracking of user behavior.

4. **Reflected in HTML**:

   - Cookies can be accessed and reflected into HTML.
   - Example in PHP:

```php
<?php
if (isset($_COOKIE["language"])) {
echo $_COOKIE["language"];
} else {
echo "<em>not set</em>";
}
?>
```

### Cookie Attributes:

1. **Customized Name/Value Pair**:
   - Each cookie contains a unique name/value pair.
     - Example: `sessionID=4fGRet67gr8`, `shoppingCart=300`
2. **Domain and Path (Scope of the Cookie)**:
   - Specifies which website (domain) and URL path the cookie belongs to.
   - Browsers follow specific rules to set and send cookies, which may vary slightly.
3. **Expires/Max-Age**:
   - Defines when the cookie expires.
   - If not set, the cookie will be treated as a session cookie and deleted when the browser tab is closed.
4. **Secure**:
   - If enabled, the cookie is only sent over secure HTTPS connections.
5. **HttpOnly**:
   - If enabled, the cookie cannot be accessed via JavaScript, offering some protection against cross-site scripting (XSS) attacks.

### Identification of Cookie

Cookies are identified by (name ,domain , path). 

Browser will update the value of the cookie if browser identify 2 cookies are same.

For example: 

cookie 1: 
```
name = userId
value = test
domain = x.site.com
path = /
secure
```

cookie 2: 

```
name = userId
value = test
domain = .site.com
path = /
secure
```

These 2 cookies are not the same even they have same name and same path because the domain is different. 

### Setting Cookies:

Cookies can be set by two main methods:

1. **HTTP Response Header**:
   - The server sends cookies to the browser using the `Set-Cookie` header.
2. **JavaScript**:
   - Cookies can be set using JavaScript, provided the `HttpOnly` flag is **not** set, which would restrict JavaScript access to the cookie.

#### Cookie Attributes (Review):

- **Domain**:
  - The domain attribute can be set to any suffix of the domain.
    - Example: If the host is `x.site.com`, the cookie can be set for both `x.site.com` and `site.com`.
    - However, it **cannot** be set for `y.site.com`, `.com`, or `anothersite.com`.
- **Path**:
  - The path attribute defines the URL path where the cookie is accessible. It can be set to any valid URL path on the server.
- **Secure and HttpOnly**:
  - `Secure` cookies are only sent over HTTPS connections.
  - `HttpOnly` cookies cannot be accessed via JavaScript (discussed later).

### Deleting Cookies:

To delete a cookie:

- **Set the same cookie with an expiration date in the past**. This will remove the cookie.
- Note: The cookie will only be deleted if the domain and path attributes of the deletion request match the original cookie's domain and path.

### Sending Cookies:

- The browser automatically sends all cookies that match the URL scope of the server request.
  - **Server URL format**: `protocol://domain/path`
  - A cookie is sent if:
    - The **cookie domain** is a suffix of the **URL domain**.
    - The **cookie path** is a prefix of the **URL path**.
    - If the cookie is marked `Secure`, the **protocol** must be HTTPS.

**Example:** 

1. **Cookie 1**:

   ```
   name = userid
   value = u1
   domain = login.site.com
   path = /
   secure
   ```

   - Sent to:
     - `https://login.site.com` (because it's a secure cookie and matches domain and path).
   - Not Sent to:
     - `http://checkout.site.com` (because it's not secure and the domain is not match).
     - `http://login.site.com` (because it's not secure).
   
2. **Cookie 2**:

   ```
   makefileCopy codename = userid
   value = u2
   domain = .site.com
   path = /
   non-secure
   ```

   - Sent to:
     - `http://checkout.site.com`
     - `http://login.site.com`
     - `https://login.site.com` (secure connections are fine even if the cookie isn't secure).

### Access Cookies with JavaScript

In the browser, JavaScript can be used to read, modify, and delete cookies through the `document.cookie` API, following the same rules as when sending cookies to the server (based on protocol, domain, and path).

#### Setting a Cookie:

To set a cookie in JavaScript:

```javascript
document.cookie = "name=value; expires=...";
```

- You can specify additional attributes such as `expires`, `domain`, and `path` to control the scope and lifespan of the cookie.

#### Reading a Cookie:

To read the current cookies:

```javascript
alert(document.cookie);
```

- This prints a string containing all cookies available to the current document (based on the page’s protocol, domain, and path).

#### Deleting a Cookie:

To delete a cookie, set its expiration date to the past:

```javascript
document.cookie = "name=; expires=Thu, 01-Jan-1970";
```

- This removes the specified cookie from the browser.

### Cookie in Browser UI

<img src="3.jpg" width="70%" height="70%">

### Threat Model of Cookies

Network attacker can steal/set cookies by sniffing/altering network traffic

 Web attacker can steal/set cookies via XSS 

User can modify cookies

<img src="4.jpg" width="50%" height="50%">

