
## Course 

https://app.pluralsight.com/library/courses/kubernetes-developers-core-concepts/table-of-contents


## Course Samples

You can find the course’s Github repository here:

https://github.com/DanWahlin/DockerAndKubernetesCourseCode


## Notes

```sh

kubectl
kubectl get pods --all
kubectl get pods --all-namespaces
kubectl get nodes
kubectl version
kubectl cluster-info
kubectl get all
alias k="kubectl"
k version
kubectl --help
kubectl get pods
kubectl get services


kubectl describe secret -n kube-system # not available
kubectl get namespaces
kubectl describe secret -n kubernetes-dashboard # not useful

```

### Web UI Dasboard

Go here: https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

```sh
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.6.1/aio/deploy/recommended.yaml

cd kubernetes-developers-core-concepts

```

[Creating sample user for Web UI](https://github.com/kubernetes/dashboard/blob/master/docs/user/access-control/creating-sample-user.md)

**Create**  `dashboard-adminuser.yaml`

**Copy manifest below:**

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard

---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard

```

**Create resources and proxy**

```sh

kubectl apply -f dashboard-adminuser.yaml
kubectl -n kubernetes-dashboard create token admin-user # token is printed
kubectl proxy # dashboard is started

```

## Creating Pods





