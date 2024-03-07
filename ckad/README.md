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

### Practice Test - Pods
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

