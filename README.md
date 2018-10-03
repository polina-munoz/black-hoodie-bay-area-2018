# black-hoodie-bay-area-2018
[BHBA18] Web Application Security Workshop

- Information about the event: https://www.blackhoodie.re/bayArea/
- Organizer: Marion, https://twitter.com/pinkflawd
- Workshop Instructors: Niru Ragupathy, https://twitter.com/itsc0rg1, Jenna Kallaher
- Banking app: mehbanking [dot] appspot [dot] com 
- Link to Slides: https://www.slideshare.net/nragupathy/introduction-to-web-application-security-blackhoodie-us-2018

Goal of workshop: Basics of web application security and explore common web attacks. At the end of the course you will have understood the concept, exploited and learnt to fix - XSS, CSRF and SQL injection. You will also get an opportunity to dabble in more esoteric attacks like XXE and SSRF on the second day. 

Day 1: 

- Theory - CIA model, HTTP, DOM, Cookies, Same Origin Policy, HTTP Methods, HTTP Headers and CORS
- Attacks - CSRF*, SQLi*, Command Injection, Broken session management*, Insecure Direct Object Reference*, Missing function access control, Logic Errors*


Day 2: 

- Attacks - Reflected XSS*, Stored XSS*, DOM XSS*, CSP*, Vulnerability chaining, SSRF*, XXE*
- Final task - Trying to get Mehbank pro* 


Some Prerequisites

- A basic understanding of HTML, JS
- A laptop with Burp proxy setup (link https://support.portswigger.net/customer/portal/articles/1783055-configuring-your-browser-to-work-with-burp to setup guide), a community version is sufficient for this course.
  - Use Firefox, preferences: network proxy, set it to 8080 
  - Manually add certificate that you can download from going to 8080 (assuming Burp is running)
  
Resources:

- So you want to be a security engineer: https://medium.com/@niruragu/so-you-want-to-be-a-security-engineer-d8775976afb7


