Layer 4 allow to:
- Forward TCP & UDP traffic to your instances
- Handle millions of request per seconds
- Less latency ~ 100 ms (vs 400ms for ALB)

- NLB has **one static IP per AZ**, and supports assigning Elastic IP (helpful for whitelisting specific IP) and don't have fixed host name
- NLB are used for exterme performance, TCP or UDP traffic![[Screen Shot 2022-04-16 at 16.28.10.png]]![[Screen Shot 2022-04-16 at 16.28.55.png]]