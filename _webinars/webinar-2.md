---
layout: page
title:  "Webinar 2"
description: "Open discussion on Cloud Technologies to learn from peers. In this webinar series we'll have  technical, non-technical disucssion on cloud related technologies ranging from IaaS, PaaS, CaaS, containers, serverless, ci/cd, automation etc." 
date:   2018-01-15 10:51:47 +0530
categories: 
    - upcoming
color: 4A148C
author: Neependra
image: /images/webinars/cloudsocial.png

webinars:
     - title: Some Title
       description: Lorem ipsum dolor sit amet, consectetur adipisicing elit. Reprehenderit reiciendis fugit inventore sit, aperiam sequi ratione explicabo accusantium minus odio quam sint, consectetur quidem! Alias voluptatibus quidem, vitae ullam facilis.
       video-id: XxxlmkKdw6M
       
published: false
---


<div>
{% for webinar in page.webinars %}
<div class="r no-gap border mb-20 mh-250">
<div class="c-5 no-padding">
    <iframe class="webinar-video" src="https://www.youtube.com/embed/{{webinar.video-id}}" frameborder="0"  allowfullscreen></iframe>
</div>
<div class="c-7 no-padding relative">
       <div class="p-20">
            <h4 class="bold">{{webinar.title}}</h4>
            <p>{{webinar.description | truncate: 160}}</p>
        </div>
        <div class="webinar-footer p-20 absolute">
            <a href="" class="btn btn-primary btn-sm">View</a>
            <a href="" class="btn btn-primary btn-sm">Something</a>
        </div>
</div>
</div>         
{% endfor %}
</div>