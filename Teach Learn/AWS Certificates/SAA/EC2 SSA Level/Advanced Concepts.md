### EC2 Nitro
- Underlying Platform for the next generation of EC2 instances
- New virtualization technology
- Allows for better performance:
	- Better networking options (enhanced networking, HPC, IPv6)
	- **Higher Speed EBS** (Nitro is necessary for 64,000 EBS IOPS - max 32,000 on non-Nitro)
	- Better underlying security
### vCPU
- Multiple threads can run on one CPU (multithreading)
- Each thread is represented as a virtual CPU (vCPU)
- Example: m5.2xlarge
	- 4 CPU
	- 2 threads per CPU
	- => 8 vCPU in total
![[Screen Shot 2022-04-03 at 21.31.43.png]]
![[Screen Shot 2022-04-03 at 21.34.42.png]]
### EC2 Capacity Reservations
- Capacity Reservations ensure you have EC2 Capacity when needed
- Manual or planned end-date for the reservation
- No need for 1 or 3-year commitment
- Capacity accesss is immediate, you get billed as soon as it starts
- Specify:
	- The Availibity Zone in which to reserve the capacity (only one)
	- The number of instances for which to reserve capacity
	- The instance attributes, including the instace type, tenancy, and platform/OS
- Combine with Reserved Instances and Savings Plans to do cost saving
