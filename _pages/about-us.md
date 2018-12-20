---
title: About Us
date: 2016-11-22T05:57:29+00:00
author: Neependra
layout: page
permalink: /about/
---




 <div class="row">
   <div class="col-md-8 col-md-offset-2">
        <p><span style="color: #007ee6;"><strong>Yuga</strong></span> means <span style="color: #007ee6;"><strong>epoch or era</strong></span> and this is definitely a Cloud era. And <span style="color: #007ee6;"><strong>Guru</strong> </span>means <span style="color: #007ee6;"><strong>Teacher</strong></span>.</p><p>We provide Training and Consulting on latest technologies like <strong>Docker, Kubernetes,  CI/CD cycle, GO programming</strong>. We are also proficient in delivering trainings on <strong>Advance Linux, System Programming, Linux Kernel, Python, Debugging and Performance Tuning.&#8221;</strong></p> 
    </div>
</div>
<div class="pb-50"></div>


<div class="about-us">    
    <div class="row">
        <div class="col-md-12">
            <div class="heading-box">
                <h2>Our Team</h2>
                </div>
        </div>
        <div class="col-md-10 col-md-offset-1">
        {% for item in site.data.data.about %}
        
        <div class="col-md-4">
            <div class="icon-box-1 border-blue">
                <div class="icon">
                    <img style="display: inline-block;" width="80" class="img-circle" src="{{site.baseurl}}{{item.image}}" alt="{{item.name}}">
                </div>
                <h4>{{item.name}}</h4>
                <p class="title"><small>{{item.title}}</small></p>
                   <ul class="social about-social-links inline-block">
                   {% if item.facebook %}
                    <li><a href="{{item.facebook}}" target="_blank" ><i class="ion-social-facebook"></i></a></li>
                   {% endif %}
                   {% if item.twitter %}
                    <li><a href="{{item.twitter}}" target="_blank"><i class="ion-social-twitter"></i></a></li>
                    {% endif %}
                    {% if item.linkedin %}
                    <li><a href="{{item.linkedin}}" target="_blank"><i class="ion-social-linkedin"></i></a></li>
                    {% endif %}
                    {% if item.github %}
                    <li><a href="{{item.github}}" target="_blank"><i class="ion-social-github"></i></a></li>
                    {% endif %}
                    {% if item.website %}
                    <li><a href="{{item.website}}" target="_blank"><i class="ion-android-globe"></i></a></li>
                    {% endif %}
                </ul>
                
                <a style="display:block" href="" class="btn btn-primary btn-sm" data-toggle="modal" data-target="#project-{{forloop.index}}">Know More</a>
            </div>
        </div>
     
        {% endfor %}
        </div>
    </div>
</div>

{% for item in site.data.data.about %}
<div id="project-{{forloop.index}}" class="modal fade" role="dialog" style="display: none;">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header text-center">
                <button type="button" class="close" data-dismiss="modal">×</button>
                <img style="display: inline-block;" width="80" class="img-circle" src="{{site.baseurl}}{{item.image}}" alt="{{item.name}}">
                <h4 class="modal-title">{{item.name}}</h4>
                 <p>{{item.title}}</p>
                </div>
            <div class="modal-body">
               <p><small>{{item.about}}</small></p>
            </div>
            <div class="modal-footer">
               <ul class="social about-social-links">
                   {% if item.facebook %}
                    <li><a href="{{item.facebook}}"><i class="ion-social-facebook"></i></a></li>
                   {% endif %}
                   {% if item.twitter %}
                    <li><a href="{{item.twitter}}"><i class="ion-social-twitter"></i></a></li>
                    {% endif %}
                    {% if item.linkedin %}
                    <li><a href="{{item.linkedin}}"><i class="ion-social-linkedin"></i></a></li>
                    {% endif %}
                    {% if item.github %}
                    <li><a href="{{item.github}}"><i class="ion-social-github"></i></a></li>
                    {% endif %}
                    {% if item.website %}
                    <li><a href="{{item.website}}"><i class="ion-android-globe"></i></a></li>
                    {% endif %}
                </ul>
                <button type="button" class="btn btn-default btn-sm" data-dismiss="modal">Close</button>
            </div>
        </div>
    </div>
</div>
{% endfor %}


<div class="container-fluid pt-80">
<div class="col-md-12">
    <div class="heading-box">
        <a class="typeform-share button" href="https://cloudyuga.typeform.com/to/pUY7fb" data-mode="popup" style="display:inline-block;text-decoration:none;background-color:#267DDD;color:white;cursor:pointer;font-family:Helvetica,Arial,sans-serif;font-size:20px;line-height:50px;text-align:center;margin:0;height:50px;padding:0px 33px;border-radius:25px;max-width:100%;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;font-weight:bold;-webkit-font-smoothing:antialiased;-moz-osx-font-smoothing:grayscale;" data-submit-close-delay="5" target="_blank">Contact Us </a> <script> (function() { var qs,js,q,s,d=document, gi=d.getElementById, ce=d.createElement, gt=d.getElementsByTagName, id="typef_orm_share", b="https://embed.typeform.com/"; if(!gi.call(d,id)){ js=ce.call(d,"script"); js.id=id; js.src=b+"embed.js"; q=gt.call(d,"script")[0]; q.parentNode.insertBefore(js,q) } })() </script>
    </div>
</div>


<!-- 
<div class="typeform-widget" data-url="https://cloudyuga.typeform.com/to/pUY7fb" style=" height: 400px; margin-bottom: 100px" > </div> <script> (function() { var qs,js,q,s,d=document, gi=d.getElementById, ce=d.createElement, gt=d.getElementsByTagName, id="typef_orm", b="https://embed.typeform.com/"; if(!gi.call(d,id)) { js=ce.call(d,"script"); js.id=id; js.src=b+"embed.js"; q=gt.call(d,"script")[0]; q.parentNode.insertBefore(js,q) } })() </script> <div style="font-family: Sans-Serif;font-size: 12px;color: #999;opacity: 0.5; padding-top: 5px;" ></div>
 -->

<div class="container-fluid pt-80">

<div class="col-md-12">
    <div class="heading-box">
        <h2>Find Us</h2>
    </div>
</div>
<div class="row">
    <div class="col-md-7">
       <!-- <iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d687.4775030809581!2d77.59076991349004!3d12.910290222055757!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x3bae1511fe717de1%3A0x8a45569b1804a6d0!2zMTLCsDU0JzM3LjUiTiA3N8KwMzUnMjcuMyJF!5e0!3m2!1sen!2sin!4v1534244258361" class="map" frameborder="0" allowfullscreen></iframe> -->
       <iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d124446.69305426338!2d77.52096625820154!3d12.91034657688894!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x3bae15a7ccd6d50b%3A0x3790b8e432be14fa!2sCloudYuga+Technologies+Pvt.+Ltd.!5e0!3m2!1sen!2sin!4v1536063355558" class="map" frameborder="0" style="border:0" allowfullscreen></iframe>
    </div>
    <div class="col-md-5">
        <h4 class=""><b>CloudYuga Technologies Pvt. Ltd.</b></h4>
        <p class=""><small>#1746 <br> 9<sup>th</sup> Cross Road Marenahalli <br> JP Nagar 2<sup>nd</sup> Phase <br>Bengaluru Karnataka<br>India - 560078</small></p>
    </div>
</div>
</div>
