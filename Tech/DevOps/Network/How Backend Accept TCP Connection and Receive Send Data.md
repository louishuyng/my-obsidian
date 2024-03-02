---
tags: Network, Devops
---
## What is Socket?

https://www.geeksforgeeks.org/socket-programming-cc/

## Accept TCP Connection
![[Pasted image 20230803111137.png]]

- Step 1: `Client` send SYN packet and `Kernel` received that packet and push connection info inside the`Sync Queue`
- Step 2: `Kernel` send back SYN/ACK packet to `Client`
- Step 3: `Client` send ACK packet to Server
- Step 4: `Kernel` receive ACK packet and query to check is that in the `Sync Queue`
	- If in the Sync Queue => Server remove that connection in `Sync Queue` => and push inside `Accept Queue`
	- If not in Sync Queue => It will drop client connection
- Step 5: `Process` (Could be nodeJS process) accept connection from `Accept Queue` and create **file descriptor** save it inside process memory

## Receive & Send Data
![[Pasted image 20230803124913.png]]

After establishing connection in the above. We will go to send packet data

- Step 1: `Client` send **packet data** to **file descriptor** on client
- Step 2: `Kernel` push that bytes to `Receive Buffer Queue`
- Step 3: `Process` read and copy from `Receive Buffer Queue`  through **file descriptor** using **epoll** or other methodology in linux
- Step 4: `Process` now read and parse the bytes (including parse all bytes to http request data, decrypt data and make an request object in C layer or any lower layer)
- Step 5: `Process` will handle processing the request and generate the response (encrypted and parse to bytes)
- Step 6: Process Send the response in bytes format to `Send Buffer Queue` through **file descriptor** 
- Step 7: `Kernel` waits enough bytes in queue (enough TCP Segment) and **send packet data** back to client

## Diagram
![[Pasted image 20230803125844.png]]