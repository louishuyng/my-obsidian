### SSL/ TLS - Basic
- An SSL Certificate allows traffic between your clients and your load balancer to be encrypted in transit (in-flight encryption)
- SSL refers to Secure Sockets Layer, used to encrypt connections
- TLS refers to Transport Layer Security, which is a newer version
- Nowadays, T:S certificates are mainly used, but people still refer as SSL
- Public SSL certificates are issued by Certificate Authorities (CA)
- Comodo, Symantec, GoDaddy, GlobalSign, Digicert, Letsencrpyt, etc...
- SSL certificates have an expiration date (you set) and must be renewed
- ![[Screen Shot 2022-04-17 at 16.07.51.png]]
- The load balancer uses an X.509 certificate (SSL/ TLS server certifcate)
- You can mange certificates using ACM (AWS Certificate Manager)
- You can create upload your own certificates alternatively
- HTTPS listener:
	- You must specify a default certificate
	- You can add an optional list of certs to support multiple domains
	- Clients can use SNI (Server Name Indication) to specify the hostname they react
	- Ability to specify a security policy to support older versions of SSL/ TLS (legacy clients)
### SSL - Server Name Indication
- SNI solves the problem of loading multiple SSL certificates onto one web server (to serve multiple websites)
- It's a 'newer' protocol, and requires the client to indicate the hostname of the target server in the intial SSL handshake
- The server will then find  the correct certificate, or return the default one
- Note:
	- Only works for ALB & NLB (newer generation), CloudFront
	- Does not work for CLB (order gen)
	- ![[Screen Shot 2022-04-17 at 16.21.20.png]]
### Class Load Balancer (v1)
- Support only one SSL certificate
- Must use multiple CLB for multiple hostname with multiple SSL certificates
### Application Load Balancer (v2) and Network Load Balancer (v2)
- Supports multiple listeners with multiple SSL certificates
- Uses Server Name Indication (SNI) to make it work