- Networking has two sorts of IPs. IPv4 and IPv6
- IPv4 is still the most common format used online
- IPv6 is newer and solves problems for the IOT
- IPv4 allows for 3.7 billion different address in the public space
- ![[Screen Shot 2022-04-03 at 11.23.15.png]]

### Public IP
- Public IP means the machine can be indetified on the internet
- Must be unique across the whole web (not two machines can have the same public IP)
- Can be geo-located easily
### Private IP
- Private IP means the machine can only be identified on a private network only
- The IP must be unquie across the private network
- BUT two different private networks (two companies) can have the same IPs
- Machines connect to WWW using a NAT + internent gateway (a proxy)
- Only a specified range of IPs can be used a private IP
### Elastic IPs
- When you stop and then start an EC2 instance, it can change its public IP
- If you need to have a fixed public IP for your instance, you need an Elastic IP
- An Elastic IP is a public IPv4 IP you own as long as you don't delete it
- You can attach it to one instance at a time
- With an Elastic IP address, you can mask the failure of an instance or software by rapidly remapping the address to another instance in your account
- You can only have 5 Elastic IP in your account
#### Overall, try to avoid using Elastic IP:
- They often reflect poor architectural decisions
- Instead, use a random public IP and register a DNS name to it
- Or,  use a Load Balancer and don't use a public IP