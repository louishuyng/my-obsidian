---
tags: Network, Devops
---
There are four different types of NAT with different behaviors to consider:

## **Full-Cone NAT**
This NAT type is also called one-to-one NAT and is the least restrictive NAT type. This maps one local IP address and port to one public IP address and port. Once a NAT translation occurs or a static one-to-one NAT is configured for a local IP address and port, any external host sourced from any port can send data to the local host through the mapped NAT IP address and port.

## **Restricted-Cone NAT**
This NAT is similar to Full-Cone NAT but is more restrictive. Once an internal host A sends a packet to an external host B and a NAT translation occurs for the local IP address and port, only the external host B (sourced from any port) can send data to the local host A through the mapped NAT IP address and port

## Full-Cone and Restricted-Cone NAT
[![](https://ccieme.files.wordpress.com/2021/02/image.png?w=1024)](https://ccieme.files.wordpress.com/2021/02/image.png)

## **Port-Restricted-Cone NAT**
This NAT is similar to Restricted-Cone NAT, but the restriction includes port numbers. Once an internal host A sends a packet to an external host B and port number X and a NAT translation occurs for the local IP address and port, only the external host B (sourced only from port X)  
can send data to the local host A through the mapped NAT IP address and port.

## **Symmetric NAT**
This is the most restrictive NAT and is similar to Port Restricted-Cone NAT, where only the external host B (sourced only from port X) can send data to the local host A through the mapped NAT IP address and port. Symmetric NAT differs in that a unique source port is used every time host A wants to communicate with a different destination. Symmetric NAT can cause issues with STUN servers because the IP address/port mapping the STUN server learns is a different mapping to another host.

## Port-Restricted -Cone NAT and Symmetric NAT
[![](https://ccieme.files.wordpress.com/2021/02/image-1.png?w=1024)](https://ccieme.files.wordpress.com/2021/02/image-1.png)

## References
https://info.support.huawei.com/info-finder/encyclopedia/en/NAT.html
https://ccieme.wordpress.com/2021/02/01/sd-wan-and-nat/