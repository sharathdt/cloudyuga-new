---
title: Search
layout: page
desc: Search webjeda website for Jekyll tutorials and github tutorials from here. 
permalink: /search/
---


<!-- Html Elements for Search -->
<div id="search-container">
<svg id="i-search" viewBox="0 0 32 32" width="26" height="26" fill="none" stroke="currentcolor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"><circle cx="14" cy="14" r="12" /><path d="M23 23 L30 30"  /></svg>
<input type="text" id="search-input" placeholder="" autofocus>

<ul id="results-container"></ul>
</div>

<!-- Script pointing to search-script.js -->
<script src="{{site.baseurl}}/js/jekyll-search.min.js" type="text/javascript"></script>

<!-- Configuration -->
<script>
SimpleJekyllSearch({
  searchInput: document.getElementById('search-input'),
  resultsContainer: document.getElementById('results-container'),
  json: '{{site.baseurl}}/search.json'
})
</script>


<style>
    
#search-container {
        min-height: 600px;
        width: 600px;
        margin: 3em auto;
        position: relative;
    }

#i-search {
        position: absolute;
        z-index: 999;
        top: 21px;
        right: 14px;
        stroke: #aaa;
    } 
#result-container li{
        line-height: 2.2;
    }
input[type=text] {

    outline: none;
    padding: 15px 25px;
    margin: 5px 1px 3px 0px;
    border: none;
    width: 100%;
    border-radius: 1px;
    box-shadow: 0 1px 0px 0 rgba(0,0,0,0.16), 0 0 0 1px rgba(0,0,0,0.08);

}
    
input[type=text]:hover {
    outline: none;
    border: none;
    margin: 5px 1px 3px 0px;
    padding: 15px 25px;
    -webkit-transition: all 0.30s ease-in-out;
    -moz-transition: all 0.30s ease-in-out;
    -ms-transition: all 0.30s ease-in-out;
    -o-transition: all 0.30s ease-in-out;
     box-shadow: 0 2px 2px 0 rgba(0,0,0,0.16), 0 0 0 1px rgba(0,0,0,0.08);
}
    
input[type=text]:focus {
    box-shadow: 0 2px 2px 0 rgba(0,0,0,0.16), 0 0 0 1px rgba(0,0,0,0.08);
    margin: 5px 1px 3px 0px;
    padding: 15px 25px;
    border: none;
    outline: none;
        -webkit-transition: all 0.30s ease-in-out;
    -moz-transition: all 0.30s ease-in-out;
    -ms-transition: all 0.30s ease-in-out;
    -o-transition: all 0.30s ease-in-out;
}
    
@media screen and (max-width: 600px) {
    
    #search-container {
        width: 100%;
        margin: 1em auto;

    }
    input[type=text]{
            width: 100%;
            box-sizing: border-box;
    } 
 }
</style>