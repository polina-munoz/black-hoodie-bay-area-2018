# day one - theory

CIA Model

- Confidentiality: rules to limit access to information
  - Privacy, PII (personally identifiable information)
  - Consider impact of social engineering
  - Data encryption, passwords
- Integrity: information is trustworthy and accurate
  - Version control
  - Checksums
  - Backups
  - Redundancies
- Availability: reliable access to information by authorized users
  - Redundancies
  - Failover
  - Use firewalls or proxy servers to guard against downtime and unreachable data due to Denial of Service (DoS) attacks

Web in general

- to some extent you can baseline truth and build something off experience: not with the web!
- the concepts is what you need to stick to, not the technology
- stick to the concepts, understand the nuances in depth. you will start reading RFCs

HTTP

- HTTP is on port 80: HTTPS is 443
- www.example.org is not the same as example.org (different domain)
- Origin requires same domain, port, and protocol
- Hypertext transfer protocol
- Specifies how client machines request information or take actions from servers
- HTTP is a pretty simple plaintext protocol
- netcat (nc) can set up a TCP connection to the destination server and send a HTTP request for you
- HTTP/2 is the next version of HTTP
- All data is in packets, series of bits split into sections (headers)
- For a GET request to a website you need to do the following:
  - Get the IP address from the DNS server (104.28.7.94)
  - Open up a socket: "I want to start talking TCP to 104.28.7.94 on port 80"
  - Client (web browser) does a TCP handshake over the socket connection with server (syn, ack, syn ack).
  - Client does the GET request now: "GET /cat.png HTTP/1.1 HOST: fb.co User-Agent: curl"
  - Server sends a response back: "HTTP/1.1 200 OK Content-type: image/png Content-Length: 123123
  - Cleanup: Close the socket connection, put the bytes for the png in a file

DNS

- In order to send a packet to a server on the internet you need an IP address (like 104.28.7.94)
- fb.com is a domain name. 
- Domain Name System (DNS) is the protocol to get an IP address from a DNS server
- The DNS request/response are usually UDP packets.
- 8.8.8.8 is Google's DNS server
- There are recursive DNS servers and authoritative DNS servers
- Example: 

Sockets

- Once you get an IP address you have to open a socket.
- Your server codes does now know how to do TCP
- You ask your OS for a socket and then connect the socket to an IP address and port
- You write to the socket to send data
- Four common socket types: TCP, UDP, raw (lets you send ICMP packets), unix (in order to talk to programs on the same computer)

TCP

- When you send a packet on the internet sometimes it gets lost
- TCP lets you send a stream of data reliability even if packets get lost or sent in the wrong order
- TCP header has: source port, destination port, sequence number, acknowledgement number, checksum, window, etc.
- Every TCP connection starts with a handshake, if "connection refused" or "connection timeout" it means the handshake did not finish.

HTTP Headers

- Every browser interprets and enforces HTTP headers in nuanced ways.
- User-Agent: check if you're a bot or what browser you're using
- Host: tell the server which website you want them to serve to you
  - this happens when you host multiple domains on the same IP address
  - It's the only required HTTP headers
  - Security: if you have no host header, you will get denied
  - If you have an invalid host header, it will default to the first virtual host in the list 
  - You can change the host header through Burp
  - It's unsafe for the application to trust the host header
  - Case Study: Password reset emails
    - "I want to make my password resets generic so it can be used by all of them"
    - Password reset links are going to come with a one time token
    - attackers can make the pw reset request for someone else and if the server trusts that host header and uses that it can give the pw token to the attacker. Security people will mouse over the links but if you don't it's a problem.
  - Case Study: 10k Host Header: https://www.theregister.co.uk/2017/08/10/schoolboy_google_bug_bounty_http_host/
- Accept-Encoding: server might compress response if you set this to gzip if you want to save bandwidth
- Cookie: data to let the server know that you're logged in
- Origin Header:
  - added by the browser
  - cannot be altered (you can change it if you have Burp)
  - This is added to cross origin requests
- Referer
  - intentionally spelled incorrectly
  - contains previous page you were on
  - contains URI of the page that requested the resource
  - it's great for tracking how users found out about your website
  - Security ppl have a problem with the referer header
  - You don't want the referer header to leek the token
  - Privacy perspective: you don't want to leek this info
- Referrer-Policy header
  - only FireFox and Chrome support it
  - certain extensions might help you drop referer headers
  - tor browser; helps prevent fingerprinting
  - ex. amazon referred links
  - take a look at the metatag

SSL/TLS

- SSL encrypts your packets
- TLS is the newer version
- HTTPS is HTTP over SSL/TLS

HTTP Methods

- GET: tell the server it wants information about something identified in the uri
  - do not use for state changing actions --> it can be cached, parameters will be logged in browser history/proxies
  - not recommended for sensitive data (bc it can be cached and viewed by proxies etc)
  - It can also be leaked by HTTP headers through other websites
  - not permitted to have request bodies 
  - You have the URI query string available to you if you need to send additional data to the server
- POST: add an entity as a child of an object identified in the URI
  - used for state changing actions
  - not cached, logged in browser history
  - safe for handling sensitive data
  - data is submitted to server in request body
- Status codes:
  - for security, the most interesting one is 500s, when you stumble across it
- Options: CORs will send a preflight options request


DOM

- You use JavaScript to manipulate the DOM
- It is a document with a logical tree
- Each branch contains a node, and each node contains objects
- DOM API allows programmatic access to the tree
- It allows you to change the documents structure, style, or content
- Nodes also have event handlers attached to them; once an event is triggered, the event handlers get executed
- https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction
- API for HTML/XML documents
- W3C dom standard implemented in most browsers; many browsers extend the standard
- HTML page = DOM + JS
- DOM designed to be independent of any programming language
- an HTML element is a node in a tree of nodes as far as the DOM is concerned

Cookies

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies
- Can be used for:
  - Session management: login, shopping cart
  - Personalization: themes
  - Tracking: analyzing user behavior
- Server sets Set-Cookie header on response.
- Browser then stores the cookie client side.
- It remembers stateful information for a stateless protocol (HTTP)
- authentication cookies tell the server that someone is logged in
- they are "automatically" sent with any request you make to the server
- it is possible for a script to execute client side to get access to your client side cookies
- cookie vs localstorage
- You can set "secure flag" so it's only sent over HTTPs.

Same Origin Policy

- you can send requests to other sites but you can't read the response: CORs is for reading the response
- simple but nuanced: origin needs to have the same domain, port, and protocol
- Most important part of web security foundational knowledge
- Pages can only "interact" with other resources having the same "origin"
  - read contents on pages
  - manipulate the DOM
  - send XMLHttpRequest
- HTML5 has added new rules to this and made it more complicated.
- Restricts how a document or script loaded from one origin can interact with a resource from another origin
- An origin for web content is defined by scheme (protocol), host (domain), and port of the URL used to access it.
- Some operations are restricted to same-origin content and this restriction can be lifted using CORs.

Cross-Origin Resource Sharing (CORS)

- Cookie headers (set-cookie) have a flag to prevent them from being sent across origin. 
- Cross domain requests are forbidden by default by the same origin policy.
- CORs gives web servers cross-domain access controls in order to enable cross domain data transfer.
- only allowing certain origins to edit data on the server
- If you make the request server side you can bypass CORS, but then it wouldn't be able to access your cookies; need to execute script client side to get the cookie
- You want this when you have multiple web servers that want to talk to each other.
- CORs is done by specifying HTTP headers
  - you need a preflight OPTIONS request: server will say what it supports
  - if you do an XMLHTTPRequest, the browser will send a preflight Options request (you don't have to think about it), and CORs will need to be set up by the server. The browser will then cache it.
- `access-control-allow-origin: *` is not what you want (unless you're google analytics)
- Try Cors: http://test-cors.com/
- Mozilla has really good documentation on CORs: https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS
- Anything that is produced by a FORM is going to complicated and require a preflight request.
- Example: Google flights pulling in flight data on other sites: can only do this using CORS

Content Security Policy

- Way to mitigate Cross Site Scripting, XSS (injecting JavaScript into client side code) and data injection attacks
- content-security-policy has multiple directives (default-src, script-src, style-src, connect-src)
- To enable CSP, you need to configure your server to add content-security-policy in response header (X-Content-Security-Policy is the older one)
- https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP


HTTP Strict-Transport-Security (HSTS)

- HSTS is a specification
- HTTP Strict Transport Security lets a web site inform the browser that it should never load the site using HTTP and should automatically convert all attempts to access the site using HTTP to HTTPS requests instead.
- It consists of one response header set by the server: Strict-Transport-Security
- max-age: specifies how long a browser should remember to force the user to access a website using HTTPS
