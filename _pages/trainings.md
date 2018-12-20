---
layout: trainings
title: Trainings
permalink: /trainings/
# video-id: 222773086
# Video-id is the if of the video from Vimeo. Remove the # to see video in place of slider. 
sitemap: false
redirect_to:
    - /
---

<div class="col-md-12">
<div class="row carousel-holder">
<div class="col-md-12">
   {% if page.video-id %}
    <iframe src="https://player.vimeo.com/video/{{page.video-id}}" class="video-header" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>
   {% else %}
<div id="carousel-example-generic" class="carousel slide" data-ride="carousel">
<ol class="carousel-indicators">
<li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
<li data-target="#carousel-example-generic" data-slide-to="1"></li>
            <li data-target="#carousel-example-generic" data-slide-to="2"></li>
        </ol>
        <div class="carousel-inner">
            <div class="item active">
                <img class="slide-image" src="{{site.baseurl}}/images/trainings/training-1.jpg" alt="">
            </div>
            <div class="item">
                <img class="slide-image" src="{{site.baseurl}}/images/trainings/training-2.jpg" alt="">
            </div>
            <div class="item">
                <img class="slide-image" src="{{site.baseurl}}/images/trainings/training-3.jpg" alt="">
            </div>
        </div>
    <a class="left carousel-control" href="#carousel-example-generic" data-slide="prev">
        <span class="glyphicon glyphicon-chevron-left"></span>
        </a>
<a class="right carousel-control" href="#carousel-example-generic" data-slide="next">
            <span class="glyphicon glyphicon-chevron-right"></span>
        </a>
    </div>
    {% endif %}
</div>
</div>
<nav class="pills list-group list-group-horizontal">
    <li class="self active"><a href="/achive/#self" class="list-group-item">Online - Self Paced</a></li>
    <li class="instr"><a href="/achive/#instr" class="list-group-item">Online - Instructor Led</a></li>
    <li class="class"><a href="/achive/#class" class="list-group-item">Classroom</a></li>
</nav>
<div class="self archive plain-links">
    <div class="masonry-container">
    {% assign sp = site.trainings | where: "categories", "self-paced" | reverse %}
    {% for post in sp %}
       <a href="{{site.baseurl}}{{post.url}}">
        <div class="item">
            <div class="thumbnail">
      {% if post.image %}
        <img src="{{site.baseurl}}/images/trainings/{{post.image}}" alt="{{post.title}}">
    {% else %}
        <img src="{{site.baseurl}}/images/trainings/training3.jpg" alt="{{post.title}}">
    {% endif %}
                <div class="caption">
                    <h4>{{post.title}}</h4>
                    {% if post.description %}
                    <p>{{post.description | truncate: 160}}</p>
                    {% else %}
                    <p>{{post.excerpt | strip_html | strip_newlines | truncate: 160}}</p>
                    {% endif %}
                </div>

            </div>
        </div>
        </a>
    {% endfor %}
    </div>

</div>
     
     
     
<div class="instr archive plain-links hide">         
<div class="masonry-container">
    {% assign sp = site.trainings | where: "categories", "instructor-led" | reverse  %}
    {% for post in sp %}
       <a href="{{site.baseurl}}{{post.url}}">
        <div class="item">
            <div class="thumbnail">
      {% if post.image %}
        <img src="{{site.baseurl}}/images/trainings/{{post.image}}" alt="{{post.title}}">
    {% else %}
        <img src="{{site.baseurl}}/images/trainings/training3.jpg" alt="{{post.title}}">
    {% endif %}
                <div class="caption">
                    <h4>{{post.title}}</h4>
                    {% if post.description %}
                    <p>{{post.description | truncate: 160}}</p>
                    {% else %}
                    <p>{{post.excerpt | strip_html | strip_newlines | truncate: 160}}</p>
                    {% endif %}
                </div>

            </div>
        </div>
        </a>
    {% endfor %}
    </div>         
</div>


<div class="class archive plain-links hide">                   
<div class="masonry-container">
    {% assign sp = site.trainings | where: "categories", "classroom" | reverse  %}
    {% for post in sp  %}
       <a href="{{site.baseurl}}{{post.url}}">
        <div class="item">
            <div class="thumbnail">
      {% if post.image %}
        <img src="{{site.baseurl}}/images/trainings/{{post.image}}" alt="{{post.title}}">
    {% else %}
        <img src="{{site.baseurl}}/images/trainings/training3.jpg" alt="{{post.title}}">
    {% endif %}
                <div class="caption">
                    <h4>{{post.title}}</h4>
                    {% if post.description %}
                    <p>{{post.description | truncate: 160}}</p>
                    {% else %}
                    <p>{{post.excerpt | strip_html | strip_newlines | truncate: 160}}</p>
                    {% endif %}
                </div>

            </div>
        </div>
        </a>
    {% endfor %}
    </div>  
</div>
</div>
