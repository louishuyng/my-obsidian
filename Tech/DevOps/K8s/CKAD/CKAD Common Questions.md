---
tags: K8s, Devops, Exam
---

1. List all namespaces in the cluster and redirect the output to a file
```bash
k get ns -A -o name | sed 's/namespace\///' > all-ns.txt
```
2. Create a pod with specified container name, image and labels
```bash
k run ${pod_name} --image=${image_name:tag} --label=app=v1
```
3. Create a secret and mount it as environment variable into a pod
```bash
k create secret generic ${secret_name} --from-literal=app=v1
k get secret ${secret_name}
```
4. Create a deployment with X image and Y tag. Change the Y tag to Z and record the changes
```bash
k create deployment ${deploy_name} --image=${x_image:y_tag} --dry-run=client -o yaml > deploy.yaml | k create -f deploy.yaml

k set image deploy/${deploy_name} x_image=x_image:z_taag
```
5. Set memory request for a Pod's container to xMi and limit to half of maximum allowed for pods of that namespaces
https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#example-1
7. RBAC: ClusterRole, ClusterRoleBindings, Role, RoleBinding, Service Accounts
https://github.com/louishuyng/awesome-tech/tree/main/devops/ckad/authorization
https://github.com/louishuyng/awesome-tech/tree/main/devops/ckad/service-account
9. Add a sidecar container that prints the logs of main container. You need to create emptvDir and mount to both the containers. Command to print logs will be given
10. Create a pod with 2 containers. Image names and other info will be provided.
---
1. Create a network policy that allows all pods with label key: value to allow traffic from only Y & Z pods in the same namespace. Allow Egress traffic on Port 53 also
https://github.com/louishuyng/awesome-tech/tree/main/devops/ckad/services-and-networking/network-policy
2. Setting CPU & Mem limits to containers of a Pod
	https://kubernetes.io/docs/concepts/policy/limit-range/
3. An ingress route is not working. Investigate and fix the issue. Debug service selector etc
https://github.com/louishuyng/awesome-tech/tree/main/devops/ckad/services-and-networking/ingress
5. Set security context on container to disable privilege escalation and run as user 2000
https://github.com/louishuyng/awesome-tech/tree/main/devops/ckad/security-contexts
6. Create a canary deployment that route only 20% of traffic
```bash
k scale deployment ${name_deployment} --replicas=4 # Scale for version we want to down traffic or up traffic
```
7. Create a pod and expose via NodePort 32000
```bash
kubectl expose pod nginx --port=80 --name nginx-service --type=NodePort --dry-run=client -o yaml
```
8. Create a cronjob that runs every 5 minutes, pods to delete after 10s, 3 completions and 3 failed
https://github.com/louishuyng/awesome-tech/blob/main/devops/ckad/pod-design/cron-job.yaml
---
1. Sort all pods by their order of creation
```bash
k get pods --sort-by=.metadata.creationTimestamp
```
2. Write all namespaced resources to a file
```bash
k get ns -A > file.txt
```
3. Filter pods by certain labels and write the results to a file
```bash
k get pods -l 'app in (v1,v2)' #Multiple values
k get pods -l app=v2 #Single value
```
4. Deploy a Helm chart with certain replicas
```bash
helm show values bitnami/node | grep -i replica
helm install mynode bitnami/node --set replicaCount=5
```
5. Upgrade the helm chart to the latest version
```bash
helm upgrade -f myvalues.yaml -f override.yaml redis ./redis
```
6. Rollback helm release
	https://helm.sh/docs/helm/helm_rollback/#helm
7. Build container image from a Dockerfile using Docker or Podman. Push them to the registry.
8. A mounted service account is unable to list PVCs in the given namespace. Fix it
9. Copy service account token to a file
```bash
k create token ${service_account} > file.txt #Generate token
kubectl describe secrets/${service_secret_annotation} # Get the token there and copy to the file
```
10. Migrate a deployment and its service from X Namespace to Y
---
1. Create a job that runs an image and a command inside it. The job should complete successfully for 3 times. You can create multiple pods to finish your job faster.
	https://github.com/louishuyng/awesome-tech/blob/main/devops/ckad/pod-design/job.yaml
	Using parallelism for faster completions and multiple pods also
2. Find the Helm release which is in pending state
3. Add readiness probe to the container to get http response from endpoint /health on port 5432. Probe should initially wait for 5s and periodically execute every 10 seconds
	https://github.com/louishuyng/awesome-tech/blob/main/devops/ckad/observability/readiness-probes.yaml
4. A deployment is using deprecated API. Fix it
	Using `kubectl convert`
	https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-kubectl-convert-plugin
5. A pod template is given. Create a deployment of 3 replicas from this template
```bash
k create -f pod_template.yaml
k create deployment deployment_name --image=pod_image --replicas=3
```
6. Create a PV and PVC. Mount this PVC at /mp of the container
	https://github.com/louishuyng/awesome-tech/blob/main/devops/ckad/state-persistence/volumes.yaml
7. Create a TLS secret. Cert and Key are provided
```bash
k create secret tls name_cert --cert=path/to/tls.cert --key=path/to/tls.key
```
8. Create a configmap from a file and mount it as Volume
```bash
k create configmap my-config --from-file=path/to/bar
```
	https://kubernetes.io/docs/concepts/storage/volumes/#configmap
---
1. Endpoints of a service are empty. Investigate and fix
2. Change the service type of a service from ClusterIP to NodePort using kubectl patch. Write the command to a file
	https://kubernetes.io/docs/tasks/manage-kubernetes-objects/update-api-object-kubectl-patch/
3. Create a ResourceQuota with the given specifications. Create a Pod with the resources given.
	https://github.com/louishuyng/awesome-tech/blob/main/devops/ckad/resource-requirement/resource-quota.yaml
4. Use an existing service account X with an existing deployment.