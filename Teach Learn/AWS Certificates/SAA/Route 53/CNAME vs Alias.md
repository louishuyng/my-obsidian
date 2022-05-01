- AWS Resources (Load Balancer, Cloudfront...) expose an AWS hostname:
	- ib1-1234.us-east-2.elb.amazonaws.com and you want myapp.mydomain.com
- CNAME:
	- Points a hostname to any other hostname (app.mydomain.com => blabla.anything.com)
	- Only for **NON ROOT domain** (aka.something.mydomain.com)
- Alias
	- Points a hostname to an AWS Resource (app.mydomain.com => blabla.amazonaws.com)
	- Works for **ROOT DOMAIN and NON ROOT DOMAIN** (aka mydomain.com)
	- Free of charge
	- Native Health Check
	
### Alias Record
- Maps a hostname to an AWS resource
- An extension to DNS functionality
- Automatically recognizes changes in the resource's IP address
- Unlike CNAME, it can be used for the top node of a DNS namespace (Zone Apex), eg: example.com
- Alias Record is always of type A/AAAA for AWS resources (IPv4/ IPv6)
- You can't set TTL
- ![[Screen Shot 2022-04-30 at 13.46.32.png]]

### Alias Record Targets
![[Screen Shot 2022-04-30 at 13.50.24.png]]
- **You cannot set an ALIAS record for an EC2 DNS name**