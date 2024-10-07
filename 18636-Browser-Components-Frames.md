---
title: 'Browser Components: Frames'
date: 2024-09-16 09:53:05
categories: 学习笔记
tags: 
- Browser Security
- 18636
- CMU
---

This Note contains: 

- Security problems and mitigation strategies
- Browser frames
- iframes 
- Problem with un-constrained frame communication
- HTML5 PostMessage

<!-- more -->
<!-- toc -->

## **Frames**

- **Old Days:**
  Frames were used primarily to organize webpage layouts but are now obsolete.

- **Example:**

  ```html
  <HTML>
    <FRAMESET cols="20%, 80%">
      <FRAME src="http://cnn.com">
      <FRAME src="http://nytimes.com">
    </FRAMESET>
  </HTML>
  ```

  This format allowed different sections of the page to load from different URLs.



## **Inline Frames (iframes)**

- **Purpose:**
  iframes embed another webpage into a sub-window on the current page. The parent page controls the size and position of the sub-window.

- **New Features in HTML5:**
  1. **Seamless:** The iframe inherits some styles from the parent page.
  2. Sandbox:
      A security feature where iframe content is treated as originating from a unique domain, which restricts actions like:
     - Disabling forms and scripts
     - Disabling plugins
       (These restrictions can be relaxed using specific attributes, e.g., `sandbox="allow-forms"`)

  Example:

  ```html
  <iframe src="login.html" sandbox="allow-forms"></iframe>
  ```

- **Benefits of iframes:**

  - **Modular design**
  - **Isolated content**: Useful for embedding ads or videos from other domains.

### **Security Issues with iframes**

**User Confusion:**

The source of the iframe content is not clearly visible, which can mislead users:

- The URL bar might show a trusted domain (e.g., trusted.com), but the iframe might load content from an untrusted source (e.g., ad.com).
- Phishing example: A fake URL like [www.paypa1.com](http://www.paypa1.com) could iframe actual PayPal content to trick users.

**Phishing/Clickjacking Attacks:**

- **Clickjacking:** Attacker embeds the victim’s page into an iframe, tricking the user into interacting with it.
- **Cross-Origin Embedding:** Many websites allow cross-origin embedding, which attackers can exploit.



## **Frame-Busting Techniques**

Websites use **JavaScript** to detect and prevent their pages from being embedded in iframes. Example:

```javascript
if (top != self) {
  top.location = self.location;
}
if (top.location != self.location) {
  parent.location = self.location;
}
```



### **Attacker Defeats Frame-Busting**

Attackers can exploit iframe sandboxing options to disable frame-busting scripts, creating a fundamental security challenge:
- **Who to trust?**
  Both defenders and attackers can use iframes, making it difficult to determine trustworthy content.



### **Defense Against Clickjacking**

1. **X-Frame-Options Header:**
   This header tells the browser not to allow iframe embedding of your page.
2. **Content Security Policy (CSP) - frame-ancestors:**
   Specifies allowed sources for embedding (or disables embedding entirely).
3. **JavaScript Frame-Busting Scripts:**
   - Use well-established scripts for this; custom scripts may be vulnerable to attacks.
   - Scripts should either prevent the page from being embedded or stop rendering if they cannot break from the iframe.





 
