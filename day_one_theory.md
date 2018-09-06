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

HTTP

- Hypertext transfer protocol
- Specifies how client machines request information or take actions from servers
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

Sockets

- Once you get an IP address you have to open a socket.
- Your server codes does now know how to do TCP
- You ask your OS for a socket and then connect the socket to an IP address and port
- You write to the socket to send data
- Four common socket types: 

TCP


  
HTTP Headers

DOM

Cookies

Same Origin Policy

HTTP Methods

CORS

