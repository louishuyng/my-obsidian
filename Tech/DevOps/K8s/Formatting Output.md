---
tags: K8s, Devops
---
## How to use it
```
**kubectl [command] [TYPE] [NAME] -o <output_format>**
```

Here are some of the commonly used formats:
1. `-o json` Output a JSON formatted API object.
2. `-o name` Print only the resource name and nothing else.
3. `-o wide` Output in the plain-text format with any additional information.
4. `-o yaml` Output a YAML formatted API object.

Here are some useful examples:

## Output with JSON format

```bash
kubectl create namespace test-123 --dry-run -o json

{
    "kind": "Namespace",
    "apiVersion": "v1",
    "metadata": {
        "name": "test-123",
        "creationTimestamp": null
    },
    "spec": {},
     "status": {}
 }
```

## Output with YAML format
```bash
 kubectl create namespace test-123 --dry-run -o yaml
 apiVersion: v1
 kind: Namespace
 metadata:
   creationTimestamp: null
   name: test-123
 spec: {}
 status: {}
```

## Output with wide (additional details)
Probably the most common format used to print additional details about the object:

```bash
 kubectl get pods -o wide
 NAME      READY   STATUS    RESTARTS   AGE     IP          NODE     NOMINATED NODE   READINESS GATES
 busybox   1/1     Running   0          3m39s   10.36.0.2   node01   <none>           <none>
 ningx     1/1     Running   0          7m32s   10.44.0.1   node03   <none>           <none>
 redis     1/1     Running   0          3m59s   10.36.0.1   node01   <none>           <none>
```
