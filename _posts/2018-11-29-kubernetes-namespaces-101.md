---
title: 'Kubernetes Namespace 101'
date: 2018-11-29T16:54:08+00:00
author: vishal 
layout: post
vantage_panels_no_legacy:
  - 'true'
siteorigin_page_settings:
  - 'a:6:{s:6:"layout";s:7:"default";s:10:"page_title";b:1;s:15:"masthead_margin";b:1;s:13:"footer_margin";b:1;s:13:"hide_masthead";b:0;s:19:"hide_footer_widgets";b:0;}'
categories:
  - containers
  - kubernetes
  - devops
tags:
  - kubernetes
image: namespace-101.jpg
description: Namespaces are Kubernetes objects which partition a single Kubernetes cluster into multiple virtual clusters.
published: true
---

![post-image]({{site.baseurl}}/images/blogs/namespace-101.jpg)

## Kubernetes Namespace
Namespaces are Kubernetes objects which partition a single Kubernetes cluster into multiple virtual clusters. Each Kubernetes namespace provides the scope for [`Kubernetes Names`](https://kubernetes.io/docs/concepts/overview/working-with-objects/names/) it contains; which means that using the combination of an object name and a Namespace, each object gets an unique identity across the cluster.

By default, a Kubernetes cluster is created with the following three namespaces:

- **default**: By default all the resource created in Kubernetes cluster are created in the `default namespace`. By default the `default namespace` can allow applications to run with unbounded CPU and memory requests/limits (Untill someone set resource quota for the `default namespace`).
- **kube-public**: Namespace for resources that are publicly readable by all users. This namespace is generally reserved for cluster usage. 
- **kube-system**:  It is the Namespace for objects created by Kubernetes systems/control plane.


## Prerequisites

- Basic experience with Linux/Unix system.
- Familiarity with Kubernetes.
- Kubernetes cluster to perform the demo.


### Create a Namespace.

- List the namespaces present in the Kubernetes cluster.

```
$ kubectl get namespaces

NAME          STATUS    AGE
default       Active    5m
kube-public   Active    5m
kube-system   Active    5m
```

We can create a Namespace in two ways either by using `kubectl create namespace` command or by using configuration file.

- Create a new namespace `my-namespace` with command.

```
$ kubectl create ns my-namespace
namespace/my-namespace created
```

- List the namespaces.

```
$ kubectl get namespace

NAME           STATUS    AGE
default        Active    8m
kube-public    Active    8m
kube-system    Active    8m
my-namespace   Active    3s
```

We can also create a namespace using the configuration file.

- Lets create a namespace from configuration file.

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: test-ns
```

- Deploy above file.

```
$ kubectl apply -f namespace.yaml 
namespace/test-ns created
```

- List the namespaces.

```
$ kubectl get namespace

NAME           STATUS    AGE
default        Active    18m
kube-public    Active    18m
kube-system    Active    18m
my-namespace   Active    9m
test-ns        Active    2m
```

## Use-cases of Namespaces

### Partition Cluster resources.
Kubernetes Namespaces can be used to divide a cluster into logical partitions allowing a single large Kubernetes cluster to be used by multiple users and teams, or a single user with multiple applications. Each user, team, or application running in a Namespace, is isolated from every other user, team, or application in other Namespaces and they operate as if they are the sole user of the cluster (note that Namespaces do not provide network segmentation). 

**Deploy application in specific namespace.**

Now we will see how to create a kubernetes object inside specific namespace.

- List the namespaces.

```
$ kubectl get namespace

NAME           STATUS    AGE
default        Active    18m
kube-public    Active    18m
kube-system    Active    18m
my-namespace   Active    9m
test-ns        Active    2m
```

- Lets create a pod from following configuration.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
  namespace: my-namespace
  labels:
     app: mypod
spec:
  containers:
  - name: mypod
    image: nginx:alpine
```
In above configuration, we have specified the namespace with field `namespace: my-namespace`. So this pod will be created inside the `my-namespace` namespace.

- Deploy a pod with above configuration.

```
$ kubectl apply -f mypopd.yaml
pod/mypod created
```

- List the pods present in `my-namespace` namespace.

```
$ kubectl get pod -n my-namespace

NAME      READY     STATUS             RESTARTS   AGE
mypod     1/1       Running            0          16s
```

**Bind user to specific namespace.**

By setting the context, we can bind a user to a specific namespace, so this namespace will be the default namespace for that user. You can do it either by modifying the `kubeconfig` file or using following command

- Create a Namespace for different teams like `dev, qa and production`

```
$ kubectl create ns dev   # Namespace for Developer team

$ kubectl create ns qa    # Namespace for QA team

$ kubectl create ns production  # Namespace for Production team
```

- Set the context for different user groups.

```
# Bind dev context to dev namespace
$ kubectl config set-context dev --namespace=dev --cluster=kubernetes --user=dev

# Bind qa context to QA namespace
kubectl config set-context qa --namespace=qa --cluster=kubernetes --user=qa

# Bind production context to production namespace
kubectl config set-context production --namespace=prod --cluster=kubernetes --user=production
```
Here we have binded Dev team to `dev` namespace, QA team to `qa` namespace and Production team to `production` namespace.
This specific namespaces will act as default namespace for paticular team assigned to it. For detailed information of Kubernetes context and kubeconfig follow the documentation given at [kubernetes.io](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/).


### Ability to specify resource consumption policies.
With [`Resource Quotas`](https://kubernetes.io/docs/concepts/policy/resource-quotas/) applied on namespaces, we can limit the cluster resources usage of a particular set of users.  A resource quota is responsible for limiting resource consumption per namespace. It also can limit the quantity of objects that can be created in a namespace by type, as well as the total amount of compute resources that may be consumed by resources in that namespace.

Now we will see how we can configure the resource quota for specific Namespace.

- Create a new `demo` namespace.
```
$ kubectl create namespace demo
namespace/demo created
```

- Create a resource qouta object for `demo` namespace.

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: mem-cpu
  namespace: demo
spec:
  hard:
    requests.cpu: "0.5"
    requests.memory: 512Mi
    limits.cpu: "1"
    limits.memory: 1Gi
```
Here, we are specifying that in demo namespace; we can not request more than 0.5 CPU and 512Mi RAM overall. So if we already have a pod running with 256 Mi request; we would not be able to schedule a new pod, if that requires more than remaining 256 Mi memory. Similarly, we are setting the hard limit of 1 CPU and 1GB RAM for the namespace.

- Deploy this resource qouta object.
```
$ kubectl apply -f qouta.yaml
resourcequota/mem-cpu created
```

- Get the detailed information about the above created resource quota.

```yaml
$ kubectl get resourcequota mem-cpu --namespace=demo --output=yaml

apiVersion: v1
kind: ResourceQuota
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","kind":"ResourceQuota","metadata":{"annotations":{},"name":"mem-cpu","namespace":"demo"},"spec":{"hard":{"limits.cpu":"1","limits.memory":"1Gi","requests.cpu":"0.5","requests.memory":"512Mi"}}}
  creationTimestamp: 2018-11-21T07:05:48Z
  name: mem-cpu
  namespace: demo
  resourceVersion: "6091"
  selfLink: /api/v1/namespaces/demo/resourcequotas/mem-cpu
  uid: dfd1002c-ed5b-11e8-ab3b-507b9d3bd0b6
spec:
  hard:
    limits.cpu: "1"
    limits.memory: 1Gi
    requests.cpu: 500m
    requests.memory: 512Mi
status:
  hard:
    limits.cpu: "1"
    limits.memory: 1Gi
    requests.cpu: 500m
    requests.memory: 512Mi
  used:
    limits.cpu: "0"
    limits.memory: "0"
    requests.cpu: "0"
    requests.memory: "0"
```
Here we can see the information about hard limit as well as used resources for given namespace.

- Deploy Demo1 application within this namespace. Whose resource request can be fulfilled by the resource quota set for this namespace.

```yaml
apiVersion: v1
kind: Pod
metadata:
  namespace: demo
  name: pod-demo1
  labels:
     app: demo1
spec:
  containers:
  - name: demo1
    image: nginx:alpine
    ports:
    - containerPort: 80
    resources:
      limits:
        memory: "400Mi"
        cpu: "0.3"      
      requests:
        memory: "300Mi"
        cpu: "0.2"
```

- Deploy this demo1 application.

```
$ kubectl apply -f demo1.yaml
pod/pod-demo1 created
```
This application resource request was within the resource quota's specification. So this pod got deployed.

- Now lets create another pod in `demo` namespace. Whose resource request is exceeding the resource quota set for the namespace. And let's see what happens.

```yaml
apiVersion: v1
kind: Pod
metadata:
  namespace: demo
  name: demo-pod2
  labels:
     app: demo
spec:
  containers:
  - name: demo
    image: nginx:alpine
    ports:
    - containerPort: 80
    resources:
      limits:
        memory: "700Mi"
        cpu: "0.6"      
      requests:
        memory: "600Mi"
        cpu: "0.6"
```

- Lets try to deploy this demo2 application.

```
$ kubectl apply -f demo2.yaml

Error from server (Forbidden): error when creating "demo2.yaml": pods "demo-pod2" is forbidden: exceeded quota: mem-cpu, requested: limits.memory=700Mi,requests.cpu=600m,requests.memory=600Mi, used: limits.memory=400Mi,requests.cpu=200m,requests.memory=300Mi, limited: limits.memory=1Gi,requests.cpu=500m,requests.memory=512Mi
```
This error occured because this pod is requesting the greater resources than specified in resource quota. 

- Lets delete the `demo` namespace. It will delete namespace along with all the object running inside it.

```
$ kubectl delete namespace demo
namespace "demo" deleted
```

- Delete earlier created namespace.

```
$ kubectl delete namespace my-namespace test-ns
namespace "my-namespace" deleted
namespace "test-ns" deleted
```


### Policies to run resources.
With the [Role-based access control (RBAC)](https://kubernetes.io/docs/reference/access-authn-authz/rbac/), we can control the access permissions within particular Namespace. Cluster Admin can controls the access permission granted to the user or groups of users in specific namespaces, this can be achieved by the help of Role Binding.

### A unique scope for Kubernetes Names.
Kubernetes namespace provides the scope for [`Kubernetes Names`](https://kubernetes.io/docs/concepts/overview/working-with-objects/names/). Within the Namespace an Object can be referred by a short name like `my-app` and the Namespace adds further scope to identify the object, e.g. `my-app.my-namespace`. Within the Namespace the name of the object must be unique and the combination of Name and Namespace (e.g. `my-app.my-namespace` ) must have to be unique at cluster level.


### Summary
Namespaces are essential objects for dividing and managing Kubernetes clusters. Namespaces allow us to logically segregate and assign resources to individual users, teams or applications. Namespaces provide the basic building blocks for resource usage allowance, access control and isolation for applications, users or groups of users. By using Namespaces, you can increase resource effeciencies as a single cluster can now be used for a diverse set of workloads.


### References
- **Kubernetes Namespaces** : https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
- **Kubernetes Names** : https://kubernetes.io/docs/concepts/overview/working-with-objects/names/
- **Role-based access control (RBAC)** : https://kubernetes.io/docs/reference/access-authn-authz/rbac/
- **Resource Quotas** : https://kubernetes.io/docs/concepts/policy/resource-quotas/)
- **Kubeconfig** : https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/
