---
layout: page
permalink: /projects/
title: Projects
---

<div class="post-list">
    {% for post in site.categories.projects %}
      {% include post.html %}
    {% endfor %}
</div>

<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=UA-92073995-2"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'UA-92073995-2');
</script>

