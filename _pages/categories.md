---
layout: page
permalink: /categories/
title: Categories
---


<div id="archives">
{% for category in site.categories %}

  <div class="archive-group">
    {% capture category_name %}{{ category | first }}{% endcapture %}
      <a name="{{ category_name | slugize }}"></a>  
     <h2 class="category-head">{{ category_name | capitalize}}</h2>
       
       
    <ul>
    {% for post in site.categories[category_name] %}
    
    <li>
     <article class="archive-item">
      <a href="{{ site.baseurl }}{{ post.url }}">{{post.title}}</a>
    </article>
    </li>

    {% endfor %}
    </ul>
  </div>
{% endfor %}
</div>
<style>
.category-head {
    line-height: 2.5;
}
li {
    line-height: 2;
}

</style>