# day one attacks

## Cross-site request forgery attacks (CSRF)

- a user issues a request without intent and it causes undesirable change (logout, delete account, change password)
- one click attack, session riding
- exploits the trust that a server has in a client
- it allows malicious user to spoof legitimate requests to the server, pretending to be an authenticated user
- to prevent CSRF, Rails embeds a CSRF token in the HTML and stores the same token in the session cookie
- When a user makes a POST request, CSRF from the HTML gets sent in that request
- Rails compares the token from the page with the token from the session cookie to ensure that they match
- Request can send the CSRF token via the X-CSRF-Token header
- Script kitties: you just need to pull in a payload and execute it and see if you can break in
- What to look for interesting CSRF attacks:
  - authenticated user (anonymous csrf is generally uninteresting)
  - no hard to predict values (large random session identifier)
  - it should change something
  
```
<iframe src="https://someothersite.com/logout.php"></iframe>
```

Example of CSRF attack (session riding): https://jsfiddle.net/dg1t0sa4/15/

Create a link that goes to some html and in the background this happens in iframe:

```
<form id="account_update_form" method='POST' action='https://453f620f9ff35bb3-dot-mehbanking.appspot.com/profile'>
<input name="address_country" value="hey another country!!!">
<input type='submit' value='Save Info to Profile'>
</form>

<script>document.getElementById("account_update_form").submit()</script>
```

How do you prevent this?

- make it unpredictable: CSRF tokens (recommended approach)
- are per user
- should be unpredictable and have a limited lifetime
- can be set in hidden form fields, HTTP headers
- token has to be unpredictable with limited lifetime
  - attacker shouldn't be able to guess it or brute force it
- header checks: origin/host/referer
  - you should blog if these headers are nto present
  - plugins can alter certain HTTP headers
  - fail closed not open 
- Double submit cookies (allows microservice infrastructure if you don't want to maintain state between those servers)
  - used when there is difficulty associating the token to a session
  - a random value is assigned to the hidden form field and a cookie, compared at server side on submission
  - attacker cannot change dom/cookies, so this will work 

## SQL injection (SQLi)

- SQL is inserted into forms
- Allows you to bypass login, retrieve sensitive information, modify/delete data
- String concatenation and a database query is probably going to bite you if you see this
- defend against this using a parametrized query

Example:

```
select user from users where user='${username}' and password='${password}' 
username = admin
password = 1234' or 1==1; --
```

```
query= "select * from users where id=1; DROP TABLE users"
```

## Command Injection

Similar to SQL injection, but in a shell command.

## Clickjacking

- like jacking (have ppl hit something but actually they're clicking the like button)
- New HTTP header called X-Frame-Options specifies whether or not a page can be framed with <iframe> or similar tags
- Chrome and Safari do not support ALLOW-FROM and instead support a similar feature in CSP
- SAMEORIGIN has different behavior in Safari. Only the top level origin is checked (intermediate frames are ignored)
- I imagine Stripe needs to take this into consideration in order to do Stripe Elements (iframes)

## Content Sniffing

- browsers can perform content sniffing to guess the right MIME type when Content Type isn't set or may have been set incorrectly
- When user uploaded or generated content is allowed, this is generated
- HTTP headers can be used to reduce the risk of content sniffing
- Content-Type header, X-Content-Type-Options: nosniff
- You could also treat the content as an attachment and download it as a file

