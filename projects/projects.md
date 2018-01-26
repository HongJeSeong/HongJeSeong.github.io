---
layout: page
permalink: /projects/
title: Projects
---

<div id="archives">
  <div class="archive-group">
    {% for post in site.categories.projects %}
        <article class="archive-item">
        <h4><a href="{{ site.baseurl }}{{ post.url }}">{{post.title}}</a></h4>
          {% include post_tags.html %}
             {% if post.image != '' %}
              <img class="thumbnail" src="{{ post.image }}"/>
               </br>
             {% endif %}
           <span class="post-meta">
           <time class="post-date" datetime="{{ page.date | date:"%Y-%m-%d" }}">{{ post.date | date: "%b %-d, %Y" }}</time>
           <span class="post-author">by {{ site.author.name }}</span>
          </span>
        </article>
       <hr>
    {% endfor %}
  </div>
</div>
<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=UA-92073995-2"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'UA-92073995-2');
</script>

