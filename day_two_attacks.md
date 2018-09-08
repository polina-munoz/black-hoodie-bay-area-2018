# day two attacks

## Cross Site Scripting (XSS)

Poster child of web security.

```
response.write("<h1>" + request.params["title"] + "</h1>")
```

Most basic XSS payload:

```
/book?title=<script>alert(document.cookie);</script>
```

JavaScript injection!

```
<h1><script>alert(document.cookie);</script></h1>
```

- Code is injected where data is expected...leading browser to execute code

Reflected XSS

- only valid for the particular user (does not persist in the server side)
- does not persist in the server; only in that request and response
- Ex. https://453f620f9ff35bb3-dot-mehbanking.appspot.com/main?message=%3Cscript%3Ealert%28document.cookie%29%3B%3C%2Fscript%3E

Stored XSS

- user generated content is stored in datastore and fetched back unescaped
- comment fields, profiles, etc.

DOM XSS

- Ex. `https://453f620f9ff35bb3-dot-mehbanking.appspot.com/main#lang=%3Cscript%3Ealert%28document.cookie%29%3B%3C%2Fscript%3E`
- it can exist in the URL or you can source scripts from other domains
  - CORS does not apply bc you are on the victim's page and sourcing the attacker's script
- You can use it as a phishing scheme
- server is never involved
- manipulate the DOM
- trusting user input and doing stuff with it in the DOM

```
const user = document.location.href.substring(document.location.href.indexOf("u=")+2);
let profile = document.createElement('div');
document.write("<p> Hello " + decodeURI(user) + "!</p>");

/profile?u=" onerror=alert(document.domain)">
/profile?u=<script>alert(document.domain)</script>
```

Where to find XSS

- Everywhere! XSS has been reported as the most pervasive vulnerability class
- If you see user input from a form field; check for reflected XSS
- If you see user input from another source (user profile), check for stored XSS
- If you see input taken and displayed without a page load, check for DOM based XSS

## Content Security Policy

## Vulnerability chaining

## SSRF

## XXE
