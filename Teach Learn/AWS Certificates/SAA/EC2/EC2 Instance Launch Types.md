### Overview
- **On-Demand Instances**: short workdload, predictable pricing
- **Reserved** (MINIMUM 1 year)
	- **Reserved Instances**: long workload
	- **Convertible Reserved Instances**: long workload with flexible instances
	- **Scheduled Reserved Instances**: example - every Thursday between 3 and 6 pm
- **Spot Instances**: short workloads, cheap, can lose instances (less reliable)
- **Dedicated Hosts**: book an entire physical server, control instance placement

### On Demand
- Pay for what you use:
	- Linux or Windiows - billing per second, after the first minute
	- All other operating systems - billing per hour
- Has the highest cost but no upfront payment
- No long-term commiment
- Recommended for short-term and un-interrupted workloads, where you can not predict how the application will behave
### Reserved
- Up to 75% discount compared to On-demand
- Reservation period: 1 year = + discount |  3 years = +++ discount
- Purchasing options: no upfront | partial upfont =+ | All upfront = ++ discount
- Reserve a specific instance type
- Recommended for steady-state usage applications (think `database`)
####  Convertible Reserved
- can change the EC2 instance type
- Up to 54% discount
#### Scheduled Reserved
- launch within time window you reserve
- When you require a fraction of day / week / month
- Still commitment over 1 to 3 years
### Spot 
- Can get a discount of up to 90% compared to On-demand
- Instances that you can lose at any point of time if your max price is less than the current spot price
- The most cost-efficient instances in AWS
- Useful for workloads that are resilient to failure:
	- Batch jobs
	- Data analysis
	- Image processing
	- Any distributed workloads
	- Workloads with a flexible start and endtime
- Not suitable for critical jobs or 
 [[EC2 Spot Instance Request]]
 [[EC2 Spot Fleets]]

### Dedicated Hosts
- An Amazon EC2 Dedicated Host is a physical server with EC2 instance capicity full dedicated to your use. Dedicated Hosts can help you address **compliance requirments** and reduce costs by allowing you use your **exisiting server-bound software licenses** 
- Allocated for your account for a 3 year period reservation
- More expensive
- Useful for software that have complicated licensing model (BYOL - Bring your own license)
- Or for companies that have strong regulatory or compliance needs
### Dedicated Instances
- Instances running on hardware that's dedicated to you
- May share hardware with other instances in same account
- No control over instance placement (can move hardware after Stop /  Start)
- ![[Screen Shot 2022-04-02 at 23.01.59.png]]