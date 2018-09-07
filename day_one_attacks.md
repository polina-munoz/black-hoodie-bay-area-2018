# day one attacks

Cross-site request forgery attacks (CSRF)

- one click attack, session riding
- exploits the trust that a server has in a client
- it allows malicious user to spoof legitimate requests to the server, pretending to be an authenticated user
- to prevent CSRF, Rails embeds a CSRF token in the HTML and stores the same token in the session cookie
- When a user makes a POST request, CSRF from the HTML gets sent in that request
- Rails compares the token from the page with the token from the session cookie to ensure that they match
- Request can send the CSRF token via the X-CSRF-Token header

SQL injection (SQLi)

- SQL is inserted into forms
- Allows you to bypass login, retrieve sensitive information, modify/delete data

Command Injection

Broken Session Management

Insecure Direct Object Reference

Missing function access control

Logic Errors

Clickjacking

- like jacking (have ppl hit something but actually they're clicking the like button)
- New HTTP header called X-Frame-Options specifies whether or not a page can be framed with <iframe> or similar tags
- Chrome and Safari do not support ALLOW-FROM and instead support a similar feature in CSP
- SAMEORIGIN has different behavior in Safari. Only the top level origin is checked (intermediate frames are ignored)
- I imagine Stripe needs to take this into consideration in order to do Stripe Elements (iframes)

Content Sniffing

- browsers can perform content sniffing to guess the right MIME type when Content Type isn't set or may have been set incorrectly
- When user uploaded or generated content is allowed, this is generated
- HTTP headers can be used to reduce the risk of content sniffing
- Content-Type header, X-Content-Type-Options: nosniff
- You could also treat the content as an attachment and download it as a file

