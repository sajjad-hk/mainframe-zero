---
source: https://www.udemy.com/course/certified-kubernetes-application-developer
tags:
  - Learning
---

## Components
1. API Server (Frontend for Kubernetes)
2. etcd (distributed key-value store)
3. Scheduler (distributing work across multiple nodes)
4. Controller (Brain behind orchestration)
5. Container runtime (Underlying software to run container i.e. docker container)
6. Kubelet (Agent running on each node on a cluster)
## Master VS Worker Nodes
* Worker node or Minion
	* Container runtime 
	* Kubelet agent
* Master node 
	* API Server
	* etcd
	* Controller
	* Scheduler
## Kubectl

```bash
kubectl run mini-kube ## to deploy a application on the cluster
kubectl cluster-info ## view information about cluster
kubectl get nodes ## list all nodes part of a cluster
```
## Recap - Pods
### Pod
* Single instance of a application
* Is the smallest object we can create in Kubernetes
```bash
kubectl run nginx --image nginx ## pull from docker repository image
```


```yml 
## pod-definition.yml
apiVersion: v1
kind: Pod
metadata:
	name: myapp-pod
	labels:
		app: myapp
		type: front-end
spec:
	containers:
		- name nginx-container
		  image: nginx
```

```bash
kubectl create -f pod-definition.yml
```

```bash
kubectl describe pod myapp-pod
```

```bash
kubectl get pods -o wide
kubectl create namespace test-123 --dry-run -o json ## name, wide, yaml
kubectl run redis --image=redis123 --dry-run=client -o yaml > pod-definition.yml
```

* To modify the properties of the pod, you can utilize the `kubectl edit pod <pod-name> ` command. Please note that only the properties listed below are editable.

	`spec.containers[*].image`
	`spec.initContainers[*].image`
	`spec.activeDeadlineSeconds`
	`spec.tolerations`
	`spec.terminationGracePeriodSeconds`
## Kubernetes controllers

```
kubectl explain replicaset
```
* Replication controller (ReplicSet)

```yaml:replicaset.yml

apiVersion:
kind:
metadata:


spec:

```


### Deployment

```YML

apiVersion: apps/v1
kind: Deployment
metadata:
	name: myapp-deployment
	labels:
		app: myapp
		type: front-end
spec:
	template:
		metadata:
			name: myapp-pod
			lables:
				name: myapp
				type: front-end
			spec:
				containers:
				- name: nginx-container
				  image: nginx
replicas: 3
selectors:
	matchLabels:
		type: front-end
		
```

* A deployment automatically create a `replicaset`
* A `replicaset` ultimately create `pods`

### Namespaces

