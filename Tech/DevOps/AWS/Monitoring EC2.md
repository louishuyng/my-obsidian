---
tags: AWS, Devops
---
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitoring_ec2.html

![[Pasted image 20230613193549.png]]
CloudWatch has available Amazon EC2 Metrics for you to use for monitoring. 
- CPU Utilization identifies the processing power required to run an application upon a selected instance. 
- Network Utilization identifies the volume of incoming and outgoing network traffic to a single instance. 
- Disk Reads metric is used to determine the volume of the data the application reads from the hard disk of the instance. This can be used to determine the speed of the application.

However, certain metrics are not readily available in CloudWatch, such as memory utilization, disk space utilization, and many others, which can be collected by setting up a custom metric.