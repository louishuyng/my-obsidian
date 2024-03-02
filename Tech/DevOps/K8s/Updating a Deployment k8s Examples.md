---
tags: K8s, Devops, Hands-on
---
Here are some handy examples related to updating a Kubernetes Deployment:

## **Creating a deployment, checking the rollout status and history:**

In the example below, we will first **create** a simple deployment and inspect the **rollout status** and the **rollout history**:

```bash
1. master $ kubectl create deployment nginx --image=nginx:1.16
2. deployment.apps/nginx created

4. master $ kubectl rollout status deployment nginx
5. Waiting for deployment "nginx" rollout to finish: 0 of 1 updated replicas are available...
6. deployment "nginx" successfully rolled out

8. master $

11. master $ kubectl rollout history deployment nginx
12. deployment.extensions/nginx
13. REVISION CHANGE-CAUSE
14. 1     <none>

16. master $
```
  
## **Using the --revision flag:**
    
Here the revision 1 is the first version where the deployment was created.

You can check the status of each revision individually by using the **--revision flag**:
```bash
1. master $ kubectl rollout history deployment nginx --revision=1
2. deployment.extensions/nginx with revision #1

4. Pod Template:
5.  Labels:    app=nginx    pod-template-hash=6454457cdb
6.  Containers:  nginx:  Image:   nginx:1.16
7.   Port:    <none>
8.   Host Port: <none>
9.   Environment:    <none>
10.   Mounts:   <none>
11.  Volumes:   <none>
12. master $ 
```


## **Using the --record flag:**

You would have noticed that the "**change-cause**" field is empty in the rollout history output. We can use the **--record flag** to save the command used to create/update a deployment against the revision number.

```bash
1. master $ kubectl set image deployment nginx nginx=nginx:1.17 --record
2. deployment.extensions/nginx image updated
3. master $master $

5. master $ kubectl rollout history deployment nginx
6. deployment.extensions/nginx

8. REVISION CHANGE-CAUSE
9. 1     <none>
10. 2     kubectl set image deployment nginx nginx=nginx:1.17 --record=true
11. master $
```

You can now see that the **change-cause** is recorded for the revision 2 of this deployment.

Let's make some more changes. In the example below, we are editing the deployment and changing the image from **nginx:1.17** to **nginx:latest** while making use of the --record flag.
```bash
1. master $ kubectl edit deployments. nginx --record
2. deployment.extensions/nginx edited

4. master $ kubectl rollout history deployment nginx
5. REVISION CHANGE-CAUSE
6. 1     <none>
7. 2     kubectl set image deployment nginx nginx=nginx:1.17 --record=true
8. 3     kubectl edit deployments. nginx --record=true

12. master $ kubectl rollout history deployment nginx --revision=3
13. deployment.extensions/nginx with revision #3

15. Pod Template: Labels:    app=nginx
16.     pod-template-hash=df6487dc Annotations: kubernetes.io/change-cause: kubectl edit deployments. nginx --record=true

18.  Containers:
19.   nginx:
20.   Image:   nginx:latest
21.   Port:    <none>
22.   Host Port: <none>
23.   Environment:    <none>
24.   Mounts:   <none>
25.  Volumes:   <none>

27. master $
```

## **Undo a change:**

Lets now rollback to the previous revision:
```bash
1. controlplane $ kubectl rollout history deployment nginx
2. deployment.apps/nginx 
3. REVISION  CHANGE-CAUSE
4. 1         <none>
5. 3         kubectl edit deployments.apps nginx --record=true
6. 4         kubectl set image deployment nginx nginx=nginx:1.17 --record=true

10. controlplane $ kubectl rollout history deployment nginx --revision=3
11. deployment.apps/nginx with revision #3
12. Pod Template:
13.   Labels:       app=nginx
14.         pod-template-hash=787f54657b
15.   Annotations:  kubernetes.io/change-cause: kubectl edit deployments.apps nginx --record=true
16.   Containers:
17.    nginx:
18.     Image:      nginx:latest
19.     Port:      <none> 
20.     Host Port:  <none>
21.     Environment: <none>       
22.     Mounts:     <none>
23.   Volumes:      

25. controlplane $ kubectl describe deployments. nginx | grep -i image:
26.     Image:        nginx:1.17

28. controlplane $
```


With this, we have rolled back to the previous version of the deployment with the **image = nginx:1.17.**

```bash
1. controlplane $ kubectl rollout history deployment nginx --revision=1
2. deployment.apps/nginx with revision #1
3. Pod Template:
4.   Labels:       app=nginx
5.         pod-template-hash=78449c65d4
6.   Containers:
7.    nginx:
8.     Image:      nginx:1.16
9.     Port:       <none> 
10.     Host Port:  <none>
11.     Environment: <none>     
12.     Mounts:     <none>
13.   Volumes:      

15. controlplane $ kubectl rollout undo deployment nginx --to-revision=1
16. deployment.apps/nginx rolled back

```

To rollback to specific revision we will use the `--to-revision` flag.  
With `--to-revision=1`, it will be rolled back with the first image we used to create a deployment as we can see in the `rollout history` output.

```bash
1. controlplane $ kubectl describe deployments. nginx | grep -i image:
2. Image: nginx:1.16
```
