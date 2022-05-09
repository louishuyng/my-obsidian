- Route traffic to your resources based on the geographic  olocation of users and resources
- Ability to shift more traffic to resources based on the defined bias
- To change the size of the geographic region, specify bias values:
	- To expand (1 to 99) - more traffic to the resource
	- To shrink (-1 to -99) - less traffic to the resource
- Resources can be:
	- AWS resources (specify AWS region)
	- Non-AWS resources (specify Latitude and Longitude)
- You must use Route 53 Traffic Flow (advanced) to use this feature


![[Screen Shot 2022-05-09 at 09.56.18.png]]![[Screen Shot 2022-05-09 at 09.57.17.png]]

### Traffic flow
- Simplify the process of creating and maintaining records in large and complex configurations
- Visual editor to manage complex routing decision trees
- Configurations can be saved as Traffic Flow Policy
	- Can be applied to different Route 53 Hosted Zones (different domain names)
	- Supports versioning