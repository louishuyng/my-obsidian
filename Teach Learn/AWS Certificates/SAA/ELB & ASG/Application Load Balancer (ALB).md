- Layer 7 (Http)
- Load balancing to multiple HTTP applications acros machines (target groups)
- Load balancing to multiple applications on the samemachines (ex: containers)
- Support for HTTP/2 and WebSocket
- Support redirect (from HTTP to HTTPS for example)
- Routing tables to different target groups:
	- Routing based on path in URL (example.com/users & example.con/posts)
	- Routing based on hostname in URL (one.example.com & other.example.com)
	- Routing based on Query String, Headers (example.com/users?id=123&order=fase)
- ALB are great fit for micro services & container-based application (example: Docker & Amzon ECS)
- Has a port mapping feature to redirect to a dynamic port in ECS 
- In comparision, we'd need multiple Class Load Balancer per application![[Screen Shot 2022-04-16 at 15.31.22.png]]

### Target Group
- EC2 Instances (can be managed by an Auto Scaling Group) - HTTP
- ECS tasks (managed by ECS itself) - HTTP
- Lambda Functions - HTTP request is translated into a JSON event
- IP Address - must be private IPs
- ALB can route to multiple target groups
- Health checks are at the target group level![[Screen Shot 2022-04-16 at 15.56.45.png]]
### Good to know
- Fixed hostname (XXX.region.elb.amazonaws.com)
- The application servers don't see the IP of the client directly
	- The true IP of the client is inserted in the header X-Forwarded-For
	- We can also get Port (X-Forwarded-Port) and proto (X-Forwarded-Proto)![[Screen Shot 2022-04-16 at 15.59.13.png]]