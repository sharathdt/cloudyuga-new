---
id: 700
title: 'Understanding Volumes in Kubernetes'
date: 2017-12-20T13:39:03+00:00
author: Neependra
layout: post
vantage_panels_no_legacy:
  - 'true'
categories:
  - containers
  - devops
  - docker
  - kubernetes
  - learnings
  - docker
  - storage 
image: k8s-volume-blog.png
description: "In this blog post we'll see different ways to attach external storage to the Pods. The focus of the blog is to cover fundamentals and not to do a deep dive. "
---

![]({{site.baseurl}}/images/blogs/k8s-volume-blog.png)

The first thing one does after learning about containers, is to run some stateless containers, something like following :- 

```
$ docker container run -i -t alpine sh 

$ kubectl run mynginx --image nginx:alpine --replicas=3
```

As one gets confidence, he/she tries some stateful applications(may be a database) to convince himself/herself, that containers can really be used for production. To run a stateful application, we need to make sure that we store the persistent data outside the container. To achieve this Docker uses `Docker Volumes` and `Volume Plugins`. Similarly Kubernetes has concept of `Volumes`, by which we can attach external storage to the Pods. 

In this blog post we'll see different ways to attach external storage to the Pods. The focus of the blog is to cover fundamentals and not to do a deep dive. 

With the help of `volumes` section in the `Pod's Defintion`, we can attach externel storage to a Pod. The external storage can be coming from NFS, GlutserFS, Cloud, Host etc. More details about the `Volumes` can be found [here](https://kubernetes.io/docs/concepts/storage/volumes/). 

![]({{site.baseurl}}/images/blogs/k8s_vol_dec_1.gif)

<script src="https://gist.github.com/cloudyuga/f4892acb6418f812a5d905194460f7a8.js"></script>

In above, the management of `Volumes` is offloaded to individual user but as a developer, I don't like it. As a developer, I won't like to take responsibility of managing storage. I should just say that I want 10GB storage and it should be allocated from `somewhere`. But in reality, that `somewhere` would be backed by some physical storage. 

To de-couple the storage provisioning and its use, Kubernete created two objects - [`Persistent Volume (PV)`](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) and `Persistent Volume Claim`. `Persistent Volumes` are created via the Kubernetes Adminitrator and they are backed by different Physical storage like AWS EBS, GCE Disk, NFS, iSCSI etc. The Adminitrator can create a pool with more than one PVs, which can be backed by different physical storage. 

![]({{site.baseurl}}/images/blogs/k8s_vol_dec_pv.png)

<script src="https://gist.github.com/cloudyuga/441a04810979def2d6b4141c7ca30fbf.js"></script>

Now coming to back the problem I mentioned earlier that as a developer I shouldn't be worried about the underlying storage management and it should be automatically allocated from `somewhere`. Kubernetes provides a special [volume](https://kubernetes.io/docs/concepts/storage/volumes/) type called `Persistent Volume Claim (PVC)`, in which we (developers) define the storage requirement like `I want 10 GB storage`. 

![]({{site.baseurl}}/images/blogs/k8s_vol_dec_2.gif)


Once we define our requirement in `PVC`, Kubernetes searches in the existing pool of PVs and attaches the best possible match. If there is no match then it would just keep looking.  

![]({{site.baseurl}}/images/blogs/k8s_vol_dec_3.gif)

<script src="https://gist.github.com/cloudyuga/f9926cd706df0d26fd19c9add15314fb.js"></script>

In above, we requested for `10 GB` worth of storage but a `PV` with `12 GB` got attached, because that was the best match. This is good but we allocated 2 GB extra, which might get wasted. What we saw is an example of `Static Volume Provisioning`, in which such fragmentation would be common. 

To overcome this Kubernetes provides `Dynamic Volume Provisioning` storage using `StorageClass` object. An administrator can create a `StorageClass` based on his/her setup. 
For example an admin can create a `StorageClass` with name `platinum`, which would create `SSD` backed disk on GCE. A user/developer would just need to mention  `platinum` as `StorageClass` in the `PVC`. 

Following is an example of in which we are using `rook-block` storage class, which would create a `PV` of `exact 10 GB` on `Rook` storage.  [Rook](https://rook.io) is a `CloudNative` storage solution, built on top on `Ceph`.  

![]({{site.baseurl}}/images/blogs/k8s_vol_dec_4.gif)

<script src="https://gist.github.com/cloudyuga/8ed97dafbbc80dd204e980cd6f0189f8.js"></script>

In above we have seen, how we can attach external storage to Kubernetes Pods using `Volumes`. It is very specific to Kubernetes. On other container orchestatrors,  we have to follow different methods and use other plugins to use external storage. This is a nightmare for any storage vendor as they have to make sure that their storage solution is compatible with different orchestrators. To solve this, different storage vendors and container community is trying to build a common interface, [`Container Storage Interface`](https://github.com/container-storage-interface/spec), so that once a storage plugin is written for one orchestrator, it should work with others as well.  Do check it [out](https://github.com/container-storage-interface/spec). 

Happy Learning !!!



