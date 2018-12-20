---
id: 699
title: 'Docker Meetup Bangalore #33' 
date: 2017-07-18T13:39:03+00:00
author: Neependra
layout: post
#permalink: /2017/07/18/dockermeetup33/
vantage_panels_no_legacy:
  - 'true'
categories:
  - containers
  - community
  - bangalore
  - devops
  - docker
  - kubernetes
  - learnings
  - swarm
image: docker_meetup_33.jpg
description: "Bangalore Docker Meetup #33. In this meetup we covered the Moby Project, LinuxKit, state of Docker in Windows, common issues and troubleshooting networking issues and Docker multistage builds."
---

![]({{site.baseurl}}/images/blogs/docker_meetup_33.jpg){: style="width: 100%; margin-bottom: 1em"}

On July 1st, we hosted [Bangalore's Docker meetup at IBM's office](https://www.meetup.com/Docker-Bangalore/events/240413454/). Around 80 participants attended the meetup. Following was the agenda of the meetup :-

- What is Moby Project ?  -  [Neependra Khare](https://twitter.com/neependra), CloudYuga 
- Getting  started with LinuxKit ?  [Ajeet Raina](https://twitter.com/ajeetsraina), Dell-EMC
- State of Docker in Microsoft World  - [Girish Goudar](https://twitter.com/girishtweek)
- Docker networking - Common issues and troubleshooting techniques - [Sreenivas Makam](https://twitter.com/srmakam), Cisco 
- Docker Multistage Build -  [Rathneesh T M](https://www.linkedin.com/in/rathneesh-m-7b803a13/), HPE

I started the session by giving the overview of [the Moby Project](http://mobyproject.org). 

<iframe src="//www.slideshare.net/slideshow/embed_code/key/jZrvTII0lVVIxj" class="video" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/neependra/project-moby" title="Project Moby" target="_blank">Project Moby</a> </strong> from <strong><a target="_blank" href="https://www.slideshare.net/neependra">Neependra Khare</a></strong> </div>

I covered how the Docker and Moby projects are different and showed a demo create a bootable VM with [*LinuxKit*](https://github.com/linuxkit/linuxkit), using the *moby-tool*. 

<a data-flickr-embed="true"  href="https://www.flickr.com/photos/neependra/35879490411/in/dateposted-public/" title="Neependra_Docker_Meetup_33"><img src="https://farm5.staticflickr.com/4315/35879490411_bccd412e60_n.jpg" class="video"  alt="Neependra_Docker_Meetup_33"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script>

Other than mine all the sessions were live streamed and recorded. 

After my talk Ajeet took over and dived deep into the LinuxKit, where he covered the *Why* aspect of the LinuxKit and how he is using it. He has also written a [blog post about the event](http://collabnix.com/speaking-at-bangalore-docker-meetup-moby-linuxkit/), where you can find more information.  His presentation can be accessed [here](https://www.slideshare.net/ajeetraina/introduction-to-linuxkit-docker-bangalore-meetup?ref=https://www.slideshare.net/ajeetraina/slideshelf) and recorded video is available at following:-

<iframe class="video" src="https://www.youtube.com/embed/H1kMQRLNaq0?rel=0" frameborder="0" align="center" allowfullscreen></iframe>


After the break Girish talked about state of Docker in the Microsoft world and showed us how to deploy a .NET application using Docker on Kubernetes. His slides are available [here](https://www.slideshare.net/sharepointguy/windows-server-and-docker) and video above.  


Understanding and troubleshooting Docker networking is one of the interesting and challenging part, when we deploy Docker in production. Sreenivas covered common networking issues and shared troubleshooting tips. 
His presentation is available [here](https://www.slideshare.net/SreenivasMakam/docker-networking-common-issues-and-troubleshooting-techniques
) and video at following:-


<iframe class="video" src="https://www.youtube.com/embed/ChGBJysUAo8?rel=0" frameborder="0" allowfullscreen></iframe>

The last talk of the day was from our first time speaker Rathneesh on Docker multi stage builds. His session was very well received and we got good feedback about it. His presentation is available [here](https://www.slideshare.net/RathneeshM/docker-multi-stage-builds
) and video above. 

In just few hours lots of knowledge got shared. Thanks to all the speakers and our host [Sudipto](https://www.linkedin.com/in/sudiptoghos/) and [Pradipta](https://www.linkedin.com/in/pradipta-banerjee-58459430/) at IBM to make it happen. 
