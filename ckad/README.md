## CNCF Certification
- **Certified Kubernetes Application Developer:** https://www.cncf.io/certification/ckad/
- **Candidate Handbook:** https://www.cncf.io/certification/candidate-handbook
- **Exam Tips:** https://docs.linuxfoundation.org/tc-docs/certification/tips-cka-and-ckad


Keep the code - 20KLOUD handy while registering for the CKA or CKAD exams at Linux Foundation to get a 20% discount.

https://github.com/mmumshad/kubernetes-the-hard-way

https://kodekloud.com/pages/community

---
### Accessing Labs

**Link:** https://uklabs.kodekloud.com/courses/labs-certified-kubernetes-application-developer
Apply the coupon code **udemystudent030485**

---

### Pods
https://uklabs.kodekloud.com/topic/pods-4/
```bash
kubectl get pods
kubectl run nginx --image nginx
kubectl descibe pod my-pod
kubectl delete pod my-pod

kubectl get pods -o wide
kubectl run redis --image redis123 --dry-run -o yaml #deprecated
kubectl run redis --image redis123 --dry-run=client -o yaml

```
#### Edit Pods
A Note on Editing Existing Pods
In any of the practical quizzes, if you are asked to **edit an existing POD**, please note the following:

If you are given a pod definition file, edit that file and use it to create a new pod.

**If you are not given a pod definition file**, you may extract the definition to a file using the below command:
```sh
kubectl get pod <pod-name> -o yaml > pod-definition.yaml
```
Then edit the file to make the necessary changes, delete, and re-create the pod.

To modify the properties of the pod, you can utilize the `kubectl edit pod <pod-name>` **command**. Please note that only the properties listed below are editable.

- spec.containers[*].image
- spec.initContainers[*].image
- spec.activeDeadlineSeconds
- spec.tolerations
- spec.terminationGracePeriodSeconds

---

### ReplicaSets
- ReplicationController **(Old)**
  ![ReplicationController](rc.png)
- ReplicaSet **(New)**
  ![ReplicaSet](rs.png)

#### Scaling Replicaset
![Scale ReplicaSet](scale-rs.png)

#### Commands
```bash
kubectl create -f rs-definition.yml
kubectl get replicaset
kubectl get rs
kubectl delete replicaset myapp-replicaset
kubectl replace replicaset rs-definition.yml
kubectl scale -replicas=6 -f rs-definition.yml
```
#### Practice Test - ReplicaSets
https://uklabs.kodekloud.com/topic/replicasets-2/

```bash
kubectl explain rs

# modifying image of a replicaset
kubectl edit rs new-replica-set

# Image is updated but pods are still down
kubectl describe rs new-replica-set

# delete all pods. ReplicaSet will create new pods with the updated image
kubectl delete pod new-replica-set-62tmv  new-replica-set-6g6jf  new-replica-set-jw7cd  new-replica-set-pv442

kubectl get rs
NAME              DESIRED   CURRENT   READY   AGE
new-replica-set   4         4         4       27m

#Scale
kubectl scale --replicas=5 rs new-replica-set
#OR
kubectl scale rs new-replica-set --replicas=2

```

### Deployments

![Deployment Definition](deployment.png)

```bash
kubectl get all
```

#### Practice Test - Deployments
https://uklabs.kodekloud.com/topic/deployments-5/

```bash

controlplane ~ ➜  kubectl get pods
No resources found in default namespace.

controlplane ~ ➜  kubectl get rs
No resources found in default namespace.

controlplane ~ ➜  kubectl get deployments
No resources found in default namespace.

controlplane ~ ➜  kubectl get deployment
No resources found in default namespace.

```
**Create a deployment with yaml**

```bash
kubectl create deployment --help

arvins-mac @ ~/1-gitspace/kubernetes  (main)
 [28] → kubectl create deployment --help
Create a deployment with the specified name.

Aliases:
deployment, deploy

Examples:
  # Create a deployment named my-dep that runs the busybox image
  kubectl create deployment my-dep --image=busybox

  # Create a deployment with a command
  kubectl create deployment my-dep --image=busybox -- date

  # Create a deployment named my-dep that runs the nginx image with 3 replicas
  kubectl create deployment my-dep --image=nginx --replicas=3

  # Create a deployment named my-dep that runs the busybox image and expose port 5701
  kubectl create deployment my-dep --image=busybox --port=5701

```
![Deployment Help](deployment-help-cmd.png)

```yml
controlplane ~ ➜  cat dep-httpd.yaml 
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpd-frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      name: httpd-pod
  template:
    metadata:
      labels:
        name: httpd-pod
    spec:
      containers:
      - name: httpd-container
        image: httpd:2.4-alpines
```

#### Certification Tip: Formatting Output with kubectl
The default output format for all `kubectl` commands is the human-readable plain-text format.

The `-o` flag allows us to output the details in several different formats.

**kubectl [command] [TYPE] [NAME] -o <output_format>**
Here are some of the commonly used formats:
1. `-o jsonOutput` a JSON formatted API object.
2. `-o namePrint` only the resource name and nothing else.
3. `-o wideOutput` in the plain-text format with any additional information.
4. `-o yamlOutput` a YAML formatted API object.

Here are some useful examples:

**Output with JSON format:**
```sh
kubectl create namespace test-123 --dry-run -o json
```
```json
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
**Output with YAML format:**
```sh
kubectl create namespace test-123 --dry-run -o yaml
```
```yml
apiVersion: v1
kind: Namespace
metadata:
  creationTimestamp: null
  name: test-123
spec: {}
status: {}
```
**Output with wide (additional details):**
Probably the most common format used to print additional details about the object:
```sh
kubectl get pods -o wide
NAME      READY   STATUS    RESTARTS   AGE     IP          NODE     NOMINATED NODE   READINESS GATES
busybox   1/1     Running   0          3m39s   10.36.0.2   node01   <none>           <none>
ningx     1/1     Running   0          7m32s   10.44.0.1   node03   <none>           <none>
redis     1/1     Running   0          3m59s   10.36.0.1   node01   <none>           <none>
```

For more details, refer:
https://kubernetes.io/docs/reference/kubectl/overview/
https://kubernetes.io/docs/reference/kubectl/cheatsheet/


### Namespaces

![Namespace DNS](ns-dns.png)
![NS DNS Desc](dns-desc.png)
![Create NS](create-ns.png)
![Switch NS](ns-switch.png)
![Resource Quota](resource-quota.png)

#### Practice Test - Namespaces
https://uklabs.kodekloud.com/topic/namespaces-3/

```sh
kubectl get ns
kubectl get pods -n=research
kubectl get pods --namespace=research

kubectl run redis --image=redis -n=finance
kubectl get pods -n=finance

kubectl get pods --all-namespaces
```

### Imperative Commands

While you would be working mostly the declarative way - using definition files, imperative commands can help in getting one-time tasks done quickly, as well as generate a definition template easily. This would help save a considerable amount of time during your exams.

Before we begin, familiarize yourself with the two options that can come in handy while working with the below commands:

`--dry-run`: By default, as soon as the command is run, the resource will be created. If you simply want to test your command, use the `--dry-run=client` option. This will not create the resource. Instead, tell you whether the resource can be created and if your command is right.

`-o yaml`: This will output the resource definition in YAML format on the screen.

Use the above two in combination along with Linux output redirection to generate a resource definition file quickly, that you can then modify and create resources as required, instead of creating the files from scratch.

```sh
kubectl run nginx --image=nginx --dry-run=client -o yaml > nginx-pod.yaml
```

**Pod**
Create an NGINX Pod
```sh
kubectl run nginx --image=nginx
```
Generate POD Manifest YAML file (`-o yaml`). Don't create it(`--dry-run`)
```sh
kubectl run nginx --image=nginx --dry-run=client -o yaml
```

**Deployment**
Create a deployment
```sh
kubectl create deployment --image=nginx nginx
```
Generate Deployment YAML file (`-o yaml`). Don't create it(`--dry-run`)
```sh
kubectl create deployment --image=nginx nginx --dry-run -o yaml
```
Generate Deployment with 4 Replicas
```sh
kubectl create deployment nginx --image=nginx --replicas=4
```
You can also scale deployment using the `kubectl scale` command.
```sh
kubectl scale deployment nginx --replicas=4
```

Another way to do this is to save the YAML definition to a file and modify
```sh
kubectl create deployment nginx --image=nginx--dry-run=client -o yaml > nginx-deployment.yaml
```
You can then update the YAML file with the replicas or any other field before creating the deployment.

**Service**
Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379
```sh
kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml
```
(This will automatically use the pod's labels as selectors)
**Or**
```sh
kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml 
```
(This will not use the pods' labels as selectors; instead it will assume selectors as `app=redis`. [You cannot pass in selectors as an option](https://github.com/kubernetes/kubernetes/issues/46191). So it does not work well if your pod has a different label set. *So generate the file and modify the selectors before creating the service*)


**Create a Service named nginx of type NodePort to expose pod nginx's port 80 on port 30080 on the nodes:**
```sh
kubectl expose pod nginx --port=80 --name nginx-service --type=NodePort --dry-run=client -o yaml
```
(This will automatically use the pod's labels as selectors, but [you cannot specify the node port](https://github.com/kubernetes/kubernetes/issues/25478). You have to generate a definition file and then add the node port in manually before creating the service with the pod.)

**Or**
```sh
kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml
```
(This will not use the pods' labels as selectors)

Both the above commands have their own challenges. While one of it cannot accept a selector the other cannot accept a node port. I would recommend going with the `kubectl expose` command. If you need to specify a node port, generate a definition file using the same command and manually input the nodeport before creating the service.

**Reference:**
https://kubernetes.io/docs/reference/kubectl/conventions/

#### Practice Test - Imperative Commands
https://uklabs.kodekloud.com/topic/imperative-commands/

```bash
#create a pod
kubectl run  nginx-pod --image=nginx:alpine

#with labels
kubectl run redis --image=redis:alpine -l=tier=db
#verify
kubectl get pods -o wide
kubectl get --help

# --show-labels=true
controlplane ~ ➜  kubectl get pods --show-labels=true
NAME        READY   STATUS    RESTARTS   AGE     LABELS
nginx-pod   1/1     Running   0          4m31s   run=nginx-pod
redis       1/1     Running   0          88s     tier=db

#Create a service redis-service to expose the redis application within the cluster on port 6379.

kubectl create service clusterip --help
kubectl create service clusterip redis-service --tcp=6379:6379 # wrong

#Correct
kubectl expose pod redis --port=6379 --name redis-service

#Create a deployment named webapp using the image kodekloud/webapp-color with 3 replicas.
kubectl create deploy --help
kubectl create deployment webapp --image=kodekloud/webapp-color --replicas=3

#Create a new pod called custom-nginx using the nginx image and expose it on container port 8080.
kubectl run custom-nginx --image=nginx --port=8080

#Create a new namespace called dev-ns
kubectl create ns dev-ns

#Create a new deployment called redis-deploy in the dev-ns namespace with the redis image. It should have 2 replicas.
kubectl create deploy redis-deploy --image=redis --replicas=2 -n=dev-ns

#verify
kubectl get all -n=dev-ns

#Create a pod called httpd using the image httpd:alpine in the default namespace. Next, create a service of type ClusterIP by the same name (httpd). The target port for the service should be 80.
#DID NOT WORK
kubectl run httpd --image=httpd:alpine
kubectl create service clusterip httpd  --tcp=80:80 
#DID NOT WORK

kubectl delete pod httpd
kubectl delete svc httpd

#Solution
kubectl run httpd --image=httpd:alpine --port=80 --expose

```

### Creating an Image - Docker
![Create Image - Docker](create-img-docker.png)
![Dockerfile](dockerfile.png)
![Layered Architecture](docker-layered-arch.png)

#### Practice Test - Docker Images
https://uklabs.kodekloud.com/topic/practice-test-docker-images-2/

![Docker CMD](docker-cmd.png)
![Docker CMD vs ENTRYPOINT](docker-cmd-entrypoint.png)
![Docker CMD & ENTRYPOINT Override at runtime](docker-cmd-entrypoint-ovrd.png)
















