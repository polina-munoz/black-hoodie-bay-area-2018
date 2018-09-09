# post workshop thoughts

- Use HTTP the right way: if you use GET to do state change, all the params are in the body and you may not get CSRF protection in Rails
- Look at your HTTP headers! What part of CSP are we taking advantage of?
- What parts of our application are vulnerable to XSS?
- Our security scanner points out code that is vulnerable to SQL injection, that's good : ) 
- Is there a security scanner for pointing out XSS?
- Maybe we should turn on CSP reporting mode to see what's up.
- Consider our authorization strategy: are we ignoring this?
- Consider logic errors: we always need to carefully consider what our desired input is.
- What is our CORs policy? What are setting?
- Learn more about how the different browsers behave -- test security vulnerabilities (and patches) in all browsers.

Main themes:

- logic errors
- authorization policy
- using HTTP correctly 
- our CORs policy
- our CSP policy 
- looking at our HTTP headers and what we set
- Understanding how our application implements CSRF protection
- test across browsers
- leverage security scanners and third party vulnerability checkers
