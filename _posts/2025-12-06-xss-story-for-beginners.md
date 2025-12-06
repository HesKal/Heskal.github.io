---
layout: default
title: "How I Hacked a Website (Legally) in 5 Minutes: An XSS Story for Beginners"
---

Have you ever wondered how websites get hacked? You might imagine a complex process requiring genius-level skills, but sometimes, all it takes is a simple mistake that a developer overlooked.

In this article, I'll take you on a short, step-by-step journey. We'll use a well-known, open-source test application called **Altoro Mutual**, which is intentionally designed to be vulnerable, to demonstrate a real-world attack in a safe and ethical environment.

---

### Part 1: The Crime Scene - A Trusting Website

Imagine we're looking at a website with a search box. Normally, if you search for "cats," you expect to see results about cats. But what if this website is a bit too... trusting? What if it blindly takes whatever we type and displays it on the page without thinking?

This overly trusting website is our crime scene today.

---

### Part 2: The Investigation, Step-by-Step

As detectives, we won't start with a full-blown attack. Instead, we'll ask the website a few simple questions to see how it responds.

#### Step 1: "Are You Really Listening?" (The HTML Injection Test)

Our first question is simple: are you just a parrot repeating my words, or do you actually understand them?

I typed this into the search box: `<B>Super Bowl</B>`

Normally, the website should search for this strange phrase. But something surprising happened: the words **Super Bowl** appeared in bold text on the screen!

**What does this mean?**
It means the website didn't see `<B>` and `</B>` as part of the search term. It saw them as a **command** to make the text bold. It obeyed the command without question. This is our first major red flag. **The website can't tell the difference between plain text and commands (HTML code).**

![Screenshot of the bold text result](./assets/images/step1-html-injection.png)

#### Step 2: "Will You Obey My Commands?" (The JavaScript Injection Test)

Now that we know the website obeys simple commands, let's try a more powerful one. JavaScript is the language that makes websites interactive. What if we give it a command in that language?

I typed this into the search box: `<script>alert('You have been hacked!')</script>`

**The result? A disaster!**
A pop-up box appeared on the screen with the message "You have been hacked!".

**What does this mean?**
It means we are no longer just controlling how text *looks*; we are now controlling how the website *behaves*. We can run any code we want on the browsers of other visitors. We've gone from being a "visitor" to being a "manager" of the page.

![Screenshot of the pop-up alert](./assets/images/step2-javascript-alert.png)

#### Step 3: Stealing the Crown Jewels (The Cookie Theft Test)

What's the most valuable thing to steal from a user's browser? Their **session cookies**. Think of a cookie as the digital key that keeps you logged into your account. If we steal it, we can get into your account.

I wrote a simple script to grab this key: `<script>alert(document.cookie)</script>`

**The result? The key is ours!**
A pop-up appeared, showing a long string of text and numbers. This is the user's session key. An attacker could now send this key to themselves and use it to impersonate the user completely.

![Screenshot of the cookie theft result](./assets/images/step3-cookie-theft.png)

---

### Part 3: How to Protect Your Castle (The Solutions - Explained for Beginners)

Now that we've seen how easy the attack was, how can we, as new developers, avoid this catastrophic mistake? The solution lies in one golden rule: **NEVER, EVER TRUST THE USER!**

Treat everything a user types as potentially malicious. Here are the three main shields you must use to protect your website.

#### Shield #1: Input Validation (The Gatekeeper)

Think of this as a strict gatekeeper at the entrance of your castle. The gatekeeper has a list of allowed guests. Anyone or anything that doesn't look right is immediately thrown out.

*   **What it is:** Input validation is the process of checking user-provided data *before* you even use or store it.
*   **How to do it:**
    *   **Whitelist, Don't Blacklist:** Don't try to block "bad" things (like `<script>`). Attackers are clever and will find ways around it. Instead, create a strict list of what is **allowed**. For a username, only allow letters and numbers. If the input doesn't match the rule, **reject it**.

#### Shield #2: Output Encoding (The Translator)

This is your most important shield against XSS. Imagine you have a guest who speaks a dangerous language (the language of browser commands). Instead of letting them speak it, you hire a translator to convert everything they say into plain, harmless speech.

*   **What it is:** Output encoding is the process of converting special characters into their safe, text-only equivalents before displaying them on a page.
*   **How to do it:** When you are about to display user-provided text on your HTML page, you must "escape" it.
    *   The character `<` becomes `&lt;`
    *   The character `>` becomes `&gt;`
*   **Example:** If a user types `<script>alert('hack')</script>`, your website will encode it. The browser will receive `&lt;script&gt;alert('hack')&lt;/script&gt;`. It will display this as text, but **will not execute it as code**.

#### Shield #3: Content Security Policy (CSP) (The Rule Book)

This is like giving your browser a strict rule book. You tell the browser: "You are ONLY allowed to run scripts that come from my own server or from trusted sources I approve. Any other script you see, you must ignore."

*   **What it is:** CSP is an extra layer of security that tells the browser which sources of content are trusted.
*   **How it helps:** Even if an attacker bypasses your other shields, CSP acts as a final line of defense. The browser will see the injected script and refuse to run it.

---

### Final Thoughts

The XSS vulnerability is a powerful reminder that sometimes the biggest security risks come from the simplest oversights. By understanding how these attacks work, we can write better, safer code.

Always remember the golden rule: **treat user input like you would a strange package left on your doorstep... with extreme caution!**
