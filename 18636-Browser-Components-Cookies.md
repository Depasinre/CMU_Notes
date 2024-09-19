---
title: 'Browser Components: Cookies'
date: 2024-09-16 08:20:21
categories: 学习笔记
tags: 
- Browser Security
- 18636
- CMU
---

This Note contains: 

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

## Attacks Around Cookies

### 1. Cookie Theft (Attack on Secrecy)

- Cookies are used to maintain session state (e.g., for authenticated users).
- If stolen, an attacker can impersonate the user, leading to **session hijacking**.

####  Defenses Against Cookie Theft:

- **Set the HttpOnly Flag**:
  - Prevents JavaScript from accessing cookies, limiting the impact of XSS (cross-site scripting).
  - **Limitations**: Cookies can still be seen in HTTP request/response headers.
  - Note: JavaScript access to headers was once a bug but has been fixed in most browsers.
- **Set the Secure Flag**:
  - Ensures cookies are only sent over secure (HTTPS) connections, reducing the risk of **packet sniffing** by network attackers.
  - **Old Attacks**: Previously, attackers could set secure cookies over an HTTP connection, but this has been patched in modern browsers.

### 2. Cookie Poisoning (Attack on Integrity)

#### Attackers can modify cookie values.
  - Example: If a cookie stores the value of a shopping cart, an attacker might modify the value (e.g., from `shopping-cart-total=150` to `shopping-cart-total=15`), exploiting weaknesses in early 2000s shopping sites.

##### Defenses Against Cookie Poisoning:

- Add Cryptographic Checksums:
  - Use a server-side key to generate a hash (e.g., HMAC) for cookie values.
  - The server validates the cookie by comparing the checksum when the cookie is sent back.
  
  <img src="5.jpg" width="50%" height="50%">

#### Cookie Expiration Manipulation

- Never Expired Cookie::
  - If the cookie's expiration is not set, the browser should delete it when the tab closes.
  - **Issue**: The "restart/restore" feature of browsers (e.g., Chrome, Firefox) can keep cookies alive indefinitely, which is a potential risk, especially on public computers.

##### Lesson Learned:

- Do not rely solely on the browser to expire cookies. Use explicit expiration dates.

### 3. Cookie Injection

#### Session Fixation Attack:
An attacker tricks a user into using a pre-set session token.
The attacker injects this token into the user's browser, and when the user logs in, the attacker hijacks the session using the same token.

##### Defense:
When elevating a user from anonymous to logged-in, always issue a **new session token** that the attacker cannot predict or reuse.
Once user logs in, token changes to value unknown to attacker.  -->Attacker’s token is not elevated.

#### Cookie Injection with the Secure Flag:
If a cookie is set with the secure flag, it should only be transmitted over HTTPS.
Previously, insecure HTTP connections could overwrite secure cookies, but modern browsers prevent this.

##### New Browser Feature:
  - Secure cookies can only be set via HTTPS with the secure flag enabled (since Chrome 52, Firefox 52, and higher).
- **HTTP Strict Transport Security (HSTS)**:
  - The HSTS header forces browsers to only accept HTTPS connections and can apply this rule to all subdomains (using `includeSubDomains`).

#### Shadow Cookies with Secure Flag (with HSTS)

- Example: **Hijacking Gmail Chat Window**:

  - A network attacker can set a malicious cookie for chat.google.com, causing the victim's chat window to be compromised.

  <img src="6.jpg" width="50%" height="50%">

  - **Defense**: Enforce HTTPS on all subdomains to prevent shadowing of secure cookies by insecure connections.

####  Cookie Injection Leading to XSS
- If cookies are reflected into HTML without proper validation, attackers can inject malicious code.

- Example:

```php
if (isset($_COOKIE["language"])) {
echo $_COOKIE["language"];
} else {
echo "<em>not set</em>";
}
```

<img src="7.jpg" width="50%" height="50%">

##### Defense: 

- Always validate cookies before reflecting them into HTML to prevent XSS.
- Assuming previously set cookies are the ones being sent is not always sound 
  - Attacker may inject cookies in between
- Secure cookies can only be set by HTTPS connection with secure flag on
  - Chrome 52 and higher and Firefox 52 and higher 
> Reasoning about the logic of web applications is not quite enough, need to know what browser is doing also

## CSRF Revisited and the SameSite Attribute

- **CSRF (Cross-Site Request Forgery)** occurs when cookies are sent to `bank.com` while browsing `attacker.com`.
- The `SameSite` attribute helps prevent this by restricting cookies to first-party or same-site contexts.

<img src="8.jpg" width="50%" height="50%">

**Browser Defaults**:

- If `SameSite` is not set, most modern browsers (Chrome, Edge, Firefox, Brave) default it to **Lax**, which helps prevent CSRF attacks.

## Cookie Prefixes for Added Security

- **__Host- Prefix**:
  - A cookie with this prefix is only accepted if:
    - The Secure flag is set.
    - It is sent from a secure origin.
    - It has no Domain attribute (so it can only be set by the host, not subdomains).
    - Its Path attribute is set to `/`.
- **__Secure- Prefix**:
  - A cookie with this prefix is only accepted if the Secure flag is set and it is sent from a secure origin.

## Key Takeaways:

- Cookies are essential for managing sessions, personalizing websites, and tracking users.
- **Cookie Scoping Rules** can be problematic, especially with shared domain providers. Awareness of the **Public Suffix List** is important.
- **Cookie Attacks** include theft, poisoning, and injection, allowing attackers to impersonate users or gain access to resources.
- Browser Defenses::
  - Use HttpOnly and Secure flags, full HSTS (with subdomains), and cookie prefixes to enhance security.

