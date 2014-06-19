---
layout: page
title: Resumé
---

<div id="slides">
  <div>
    <p>Mon texte</p>
    <img alt="Mon image" src="/public/git.png" />
  </div>
</div>

<div id="projects">
  {% for collection in site.data.openbadges %}
     
       <div class="badgesCollection">
         <span class="badges">
           <img src="{{ collection.image }}" alt="{{ collection.collection }}">
         </span>
         {% for badge in collection.badges %}
           <span class="badges">
             <div class="badgeWarper">
               <a href="{{ badge.url }}"><img class="badge" src="{{ badge.image }}" alt="{{ badge.title }}"></a>
             </div>
           </span>
         {% endfor %}
       </div>
       <hr/>
  {% endfor %}
</div>