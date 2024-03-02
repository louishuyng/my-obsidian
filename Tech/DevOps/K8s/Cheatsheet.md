---
tags: K8s, Devops
---
# Core Concepts

View resources in namespace `dev`:

```bash
kubectl get pods -n dev
```

View all pods in all namespaces:

```bash
kubectl get pods -A
```

View all resources in all namespaces:

```bash
kubectl get all -A
```

Generate a pod yaml file with `nginx` image and `label env=prod`:

```bash
kubectl run nginx --image=nginx --labels=env=prod --dry-run=client -o yaml > nginx_pod.yaml
```

Delete a pod `nginx` fast:

```bash
kubectl delete pod nginx --grace-period 0 --force
```

Generate Deployment yaml file:

```bash
kubectl create deploy --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml
```

Access a service `test-service` in a different namespace `dev`:

```bash
test-service.dev
```

Create a service for a pod `valid-pod`, which serves on port 444 with the name `frontend`:

```bash
kubectl expose pod valid-pod --port=444 --name=frontend
```

Recreate the contents of a yaml file:

```bash
kubectl replace --force -f nginx.yaml
```

Edit details of a deployment `nginx`:

```bash
kubectl edit deploy nginx
```

Set image of a deployment `nginx`:

```bash
kubectl set image deploy nginx nginx=nginx:1.18
```

Scale deployment `nginx` to 4 replicas and `record` the action:

```bash
kubectl scale deploy nginx --repliacs=4 --record
```

Get events in current namespace:

```bash
kubectl get events
```

## Scheduling

Get pods with their labels:

```bash
kubectl get pods --show-labels
```

Get the pods that are labeled `env=dev`:

```bash
kubectl get pods -l env=dev
```

Get taints of node `node01`:

```bash
kubectl describe node node01 | grep -i Taints:
```

Label node `node01` with label `size=small`:

```bash
kubectl label nodes node01 size=small
```

Default static pods path:

```bash
/etc/kubernetes/manifests
```

Check pod nginx logs:

```bash
kubectl logs nginx
```

Check pod logs with multiple containers:

```bash
kubectl logs <pod_name> -c <container_name>
```

## Monitoring

Check node resources usage:

```bash
kubectl top node
```

Check pod and their containers resource usage:

```bash
kubectl top pod --containers=true
```

## Application Lifecycle Management

Check rollout status of deployment `app`:

```bash
kubectl rollout status deployment/app
```

Check rollout history of deployment `app`:

```bash
kubectl rollout history deployment/app
```

Undo rollout:

```bash
kubectl rollout undo deployment/app
```

Create configmap `app-config` with `env=dev`:

```bash
kubectl create configmap app-config --from-literal=env=dev
```

Create secret `app-secret` with `pass=123`:

```bash
kubectl create secret generic app-secret --from-literal=pass=123
```

## Cluster Maintenance

Drain node `node01` of all workloads:

```bash
kubectl drain node01
```

Make the node schedulable again:

```bash
kubectl uncordon node01
```

Upgrade cluster to 1.18 with kubeadm:

```bash
kubeadm upgrade plan
apt-get upgrade -y kubeadm=1.18.0-00
kubeadm upgrade apply v1.18.0
apt-get upgrade -y kubelet=1.18.0-00
systemctl restart kubelet
```

Backup etcd:

```bash
export ETCDCTL_API=3
etcdctl \
--endpoints=https://127.0.0.1:2379 \
--cacert=/etc/kubernetes/pki/etcd/ca.crt \
--cert=/etc/kubernetes/pki/etcd/server.crt \
--key=/etc/kubernetes/pki/etcd/server.key \
snapshot save /tmp/etcd-backup.db
```

Restore etcd:

```bash
ETCDCTL_API=3 etcdctl snapshot restore /tmp/etcd-backup.db --data-dir /var/lib/etcd-backup
```

After edit `/etc/kubernetes/manifests/etcd.yaml` and change `/var/lib/etcd` to `/var/lib/etcd-backup`.

## Security

Create service account `sa_1`

```bash
kubectl create serviceaccount sa_1
```

Check kube-apiserver certificate details:

```bash
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout
```

Approve certificate singing request for user john:

```bash
kubectl certificate approve john
```

Check the current kubeconfig file:

```bash
kubectl config view
```

Check current context:

```bash
kubectl config current-context
```

Use context dev-user@dev:

```bash
kubectl config use-context prod-user@production
```

Validate if user `john` can create deployments:

```bash
kubectl auth can-i create deployments --as john
```

Create role `dev` to be able to create secrets:

```bash
kubectl create role dev --verb=create --resource=secret
```

Bind the role `dev` to user `john`:

```bash
kubectl create rolebinding dev-john --role dev --user john
```

Check namespaced resources:

```bash
kubectl api-resources --namespaced=true
```

## Troubleshooting

View all the kube-system related pods:

```bash
kubectl get pods -n kube-system
```

Check if all nodes are in `ready` state:

```bash
kubectl get nodes
```

Check memory, cpu and disk usage on node:

```bash
df -h
top
```

Check status of `kubelet` service on node:

```bash
systemctl status kubelet
```

Check `kubelet` service logs:

```bash
sudo journalctl -u kubelet
```

View kubelet service details:

```bash
ps -aux | grep kubelet
```

Check cluster info:

```bash
kubectl cluster-info
```

## Gather info

Find pod CIDR:

```bash
kubectl describe node | less -p PodCIDR
```

Get pods in all namespaces sorted by creation timestamp:

```bash
kubectl get pod -A --sort-by=.metadata.creationTimestamp
```

Find the service CIDR of `node-master`:

```bash
ssh node0master
cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep range
```

Find which CNI plugin is used on `node-master`:

```bash
ls /etc/cni/net.d/
```

Find events ordered by creation timestamp:

```bash
kubectl get events -A --sort-by=.metadata.creationTimestamp
```

Find internal IP of all nodes:

```bash
kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="InternalIP")].address}'
```

## General notes

- To create a daemonset, use `kubectl create deploy` command to create a .yaml file and then change the `kind` and remove `replicas` & `strategy`.
- To find the static pod manifest path, check the exec command of `kubelet service` or `staticPodPath` parameter of kubelet’s config file.
- To create a static pod, place a yaml definition file in the `staticPodPath` directory.
- To identify static pods look for the suffix `-<node_name>` on pods.
- To add a new scheduler copy the existing one and add to the container’s command the flags`--leader-elect=false` and `--scheduler-name=my-scheduler-name`. To use the new scheduler under `spec` of a pod definition file specify the option `schedulerName`.
- To add a default command to a pod use `command` that overrides the default `ENTRYPOINT` from Dockerfile. Use `args` to override the Dockerfile `CMD` command for the commmand’s extra parameters.
