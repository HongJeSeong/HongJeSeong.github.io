---
layout: page
permalink: /posts/
title: Posts
---

<div id="archives">
  <div class="archive-group">
    {% for post in site.categories.posts %}
        <article class="archive-item">
        <h4><a href="{{ site.baseurl }}{{ post.url }}">{{post.title}}</a></h4>
         {% include post_tags.html %}
           <span class="post-meta">
           <time class="post-date" datetime="{{ page.date | date:"%Y-%m-%d" }}">{{ post.date | date: "%b %-d, %Y" }}</time>
           <span class="post-author">by {{ site.author.name }}</span>
          </span>
        </article>
       <hr>
    {% endfor %}
  </div>
</div>

