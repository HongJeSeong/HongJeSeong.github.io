---
layout: page
permalink: /tags/
title: Tags
---

<ul class="tag-cloud">
<h1>Tag Cloud</h1>
{% for tag in site.tags %}
  <span style="font-size: {{ tag | last | size | times: 100 | divided_by: site.tags.size | plus: 70  }}%">
    <a href="#{{ tag | first | slugize }}">
      {{ tag | first }}
    </a> &nbsp;&nbsp;
  </span>
{% endfor %}
</ul>

<div id="tags">
{% for tag in site.tags %}
  <div class="tag-group">
    {% capture tag_name %}{{ tag | first }}{% endcapture %}
    <h2 id="#{{ tag_name | slugize }}">{{ tag_name }}</h2>
    <a name="{{ tag_name | slugize }}"></a>
    {% for post in site.tags[tag_name] %}
    <article class="tag-item">
      <h4><a href="{{ root_url }}{{ post.url }}">{{post.title}}</a></h4>
    </article>
    {% endfor %}
  </div>
{% endfor %}
</div>
