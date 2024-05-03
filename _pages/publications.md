---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

<script src="https://bibbase.org/show?bib=https://bibbase.org/f/MkoKWbmd47BMF7EHK/citations.bib&jsonp=1"></script>         

{% if site.author.googlescholar %}
  <div class="wordwrap">You can also find my articles on <a href="{{site.author.googlescholar}}">my Google Scholar profile</a>.</div>
{% endif %}

{% include base_path %}

{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %}
