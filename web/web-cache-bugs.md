## web cache flow
For a website that uses a CDN, the user’s request first reaches the CDN server. If the CDN has the website’s content cached, it directly returns the cached content to the user. Otherwise, the CDN contacts the website’s origin server, fetches the latest content, caches it, and then delivers that content to the user.
### CDN
these CDN (content delivery networks) server locates at multiple locations all over the globe. some famous CDN providers are:
- cloudflare
- amazon cloudfront
- akamai
- google cloud cdn
- microsoft azure cdn
etc...
the main goal of CDN servers are to cache the websites to deliver faster content to the users. But it also enhance security of the website if configured correctly. few major attacks that CDN prevents:
- **DDOS (distributed denial of service) attack:** by identifying and filtering malicious traffic and distributing traffic across different global **edge servers (CDN servers located globally).** CDN also prevents rate limit attacks.

> there are 3 kinds of CDN servers:
> **origin servers:** the server of origin website from where the CDN edge server fetches and cache data. its part of the caching process thats why it is considered as one of the CDN server.
> **edge servers:** the servers that are located all around the globe that caches and store static document for faster delivery to the end user.
> **DNS servers:** for a CDN enabled websites, the DNS servers direct users to an appropriate CDN edge location instead of directly to the origin server.

- **Web Application Firewall (WAF):** many CDN servers provide WAF protection to the customers, that protects various attacks like SQL injection, XSS etc. by analyzing incoming traffic, WAF blocks malicious requests.
- CDN also provides SSL/TLS encryption, Content Integrity and Authenticity protection and Secure token authentication that prevents Data breaches, MITM (man in the middle) attack.


## what is web cache deception?
