- Define **max spot price** and get the instance while current spot price < max
	- The hourly spot price varies based on offer and capacity
	- If the current spot price > your max price you can choose to **stop** or **terminate** your instance with a 2 mintues grace period
- Other strategy: **Spot Block**
	- "block" spot instance during a specified time fram (1 to 6 hours) without interruptions
	- In rare situtations, the instance may be reclaimed
- Used for batch jobs, data analysis, or workloads that are resilient to failures
### How to terminate Spot Instances
![[Screen Shot 2022-04-02 at 23.15.38.png]]![[Screen Shot 2022-04-02 at 23.17.43.png]]

- you can only cancel Spot Instance requests that are open, active or disabled
- Cancelling a Spot Request does not terminate instances
- You must first cacnel a Spot Request, and then terminate the associated Spot Instances