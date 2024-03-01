
### Distributed Systems
Distributed system is a group of connected servers that work together in such a way that appears to be a single unit by the end user

- Scales horizontally - increasing capacity by adding more servers that work together
- Scales vertically - increasing capacity by adding more memory or using a faster CPU
- Sharding - splitting the server into multiple servers; aka partitioning


### Core Networking Fundamentals
7 layers of the OSI Model
- Physical (1) - physical medium
	- Physical structure
	- Coax, fiber, wireless, hubs, repeaters
- Data link (2) - format of the data on the network
	- Frames
	- Ethernet, PPP, Switch, Bridge
- Network (3) - decides the physical path of the data
	- Packers
	- IP, ICMP, IPSec, IGMP
- Transport (4) - transports the data using transmission protocols
	- End-to-end connections
	- TCP, UDP
- Session (5) - maintaining sessions and configuring ports
	- Sync and send to port
	- APIs, sockers, WinSock
- Presentation (6) - ensures data is in a usable format and where encryption occurs if any
	- Syntax layer
	- SSL, SSH, IMAP, FTP, MPEG, JPEG
- Application (7) - this is where humans come in; human-computer interaction layer
	- End user layer
	- HTTP, FTP, IRC, SSH, DNS

L4 and L7 are the important layers to know

### TCP / IP

Primary way data gets around the internet

Ports:
- 80: HTTP
- 22: SSH
- 53: DNS
- 443: HTTPS
- 25: SMTP
- 3306: MySQL


### HTTP/HTTPS

Key protocols for cloud architects
HTTP works on L7 of the OSI model

Status codes:
- 100: Information
	- 100 - Continue
	- 101 - Switching protcol
- 200: Successful Response
	- 200 - Okay
	- 201 - Created
	- 202 - Accepted
	- 204 - No content
	- 206 - Partial content
- 300: Redirection
	- 301 - Moved permanently
	- 304 - Not modified (caching)
	- 307 - Temporary redirect
	- 308 - Permanent redirect
- 400: Client error
	- 400 - Bad request
	- 401 - Unauthorized
	- 403 - Forbidden
	- 404 - Not found
	- 408 - Request timeout
	- 429 - Rate limits exceeded
- 500: Server error
	- 500 - internal server error
	- 501 - not implemented
	- 502 - bad gateway
	- 503 - service unavailable
	- 504 - gateway timeout
	- 511 - network authentication required


### SRE Principles

SLI are metrics over time
- Specific to a user journey such as a request/response, data processing, or storage
- Examples:
	- Request latency: how long it takes to return a response to a request
	- Failure rate: a fraction of all rates received (unsuccessful requests / all requests)
	- Batch throughput: proportion of time = data processing rate > set threshold

SLOs specify a target level for the reliability of your service
- Aiming for 100% is never a good idea because its much more expensive and complex
- Users don't need 100% to be acceptable
- Less than 100% leaves room for new features

SLAs are an explicit or implicit contract with your users that includes consequences of missing or meeting the SLOs they contain

**SLI's drive SLO's while SLO's inform SLA's**

