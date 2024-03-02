---
tags: Network, Devops
---

https://www.youtube.com/watch?v=-2WnHiKPrgE


## **What is Anycast?**

The Internet Protocol (IP) uses three types of addressing schemes: Unicast, Multicast, and Anycast.

A **Unicast** address is used to identify a single unique host. It is used to send data to a single destination. In computer networking, unicast communication is a one-to-one transmission from one point in the network to another. 

A **Multicast** address is used to deliver data to a group of destinations (a one-to-many transmission). IP multicast group addresses are represented by class-D IP addresses reserved specifically for multicast communications, ranging from 224.0.0.0 through 239.255.255.255. Any IP packet sent to a multicast address is delivered to only those hosts that have joined that particular IP Multicast group, resulting in less network traffic, thereby reducing bandwidth and network overhead. If the host hasn’t joined the group, the receiver ignores the packets at the hardware level, eliminating platform software resource consumption in that network element. IPv6 multicast replaces broadcast addresses that were supported in IPv4. 

**Anycast**, also known as IP Anycast or Anycast routing, is an IP network addressing scheme that allows multiple servers to share the same IP address, allowing for multiple physical destination servers to be logically identified by a single IP address. Based on the location of the user request, the anycast routers send it to the server in the network based on a least-cost analysis that includes assessing the number of hops, shortest distance, lowest transit cost, and minimum latency measurements to optimize the selection of a destination server.