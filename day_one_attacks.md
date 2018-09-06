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

