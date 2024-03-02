---
tags: K8s, Devops
---
Issue as screenshot below
![[Pasted image 20230729172658.png]]

This can easily be resolved by editing the deployment yaml files and adding the `hostNetwork: true` after the `dnsPolicy: ClusterFirst`

```yaml
kubectl edit deployments.apps -n kube-system metrics-server
```

insert:

```yaml
hostNetwork: true
```
