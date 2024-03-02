---
tags:
  - Devops
  - Consul
---
Source: https://developer.hashicorp.com/consul/docs/architecture
## Introduction

Consul provides a control plane that enables you to register, access, and secure services deployed across your network. 

The _control plane_ is the part of the network infrastructure that maintains a central registry to track services and their respective IP addresses.

![Diagram of the Consul control plane](https://developer.hashicorp.com/_next/image?url=https%3A%2F%2Fcontent.hashicorp.com%2Fapi%2Fassets%3Fproduct%3Dconsul%26version%3Drefs%252Fheads%252Frelease%252F1.16.x%26asset%3Dwebsite%252Fpublic%252Fimg%252Fconsul-arch%252Fconsul-arch-overview-control-plane.svg%26width%3D960%26height%3D540&w=1920&q=75)
## Data Center

The Consul control plane contains one or more _datacenters_. 

A datacenter is the smallest unit of Consul infrastructure that can perform basic Consul operations. 

You can create multiple datacenters and allow nodes in different datacenters to interact with each other.

### [](https://developer.hashicorp.com/consul/docs/architecture#clusters)Clusters

A collection of Consul agents that are aware of each other is called a _cluster_. 

## [](https://developer.hashicorp.com/consul/docs/architecture#agents)Agents

You can run the Consul binary to start Consul _agents_, which are daemons that implement Consul control plane functionality. 

You can start agents as servers or clients

### [](https://developer.hashicorp.com/consul/docs/architecture#server-agents)Server agents

Consul server agents store all state information, including service and node IP addresses, health checks, and configuration. 

#### [](https://developer.hashicorp.com/consul/docs/architecture#consensus-protocol)Consensus protocol

Consul clusters elect a single server to be the _leader_ through a process called _consensus_. 

##### Leader
The leader processes all queries and transactions, which prevents conflicting updates in clusters containing multiple servers.

The leader replicates the requests to all other servers in the cluster. Replication ensures that if the leader is unavailable, other servers in the cluster can elect another leader without losing any data.

##### Followers
Servers that are not currently acting as the cluster leader are called _followers_. 
Followers forward requests from client agents to the cluster leader. 

Consul servers establish consensus using the Raft algorithm on port `8300`. Refer to [Consensus Protocol](https://developer.hashicorp.com/consul/docs/architecture/consensus) for additional information.

![Diagram of the Consul control plane consensus traffic](https://developer.hashicorp.com/_next/image?url=https%3A%2F%2Fcontent.hashicorp.com%2Fapi%2Fassets%3Fproduct%3Dconsul%26version%3Drefs%252Fheads%252Frelease%252F1.16.x%26asset%3Dwebsite%252Fpublic%252Fimg%252Fconsul-arch%252Fconsul-arch-overview-consensus.svg%26width%3D960%26height%3D540&w=1920&q=75)

### [](https://developer.hashicorp.com/consul/docs/architecture#client-agents)Client agents

Consul clients report node and service health status to the Consul cluster. 

Clients use remote procedure calls (RPC) to interact with servers.

You can also run Consul with an alternate service mesh configuration that deploys Envoy proxies but not client agents. Refer to [Simplified Service Mesh with Consul Dataplanes](https://developer.hashicorp.com/consul/docs/connect/dataplane) for more information.