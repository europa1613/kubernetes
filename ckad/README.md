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



