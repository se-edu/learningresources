<div id="flex-body">
{% include "_markbind/layouts/sitenav.md" %}
<div id="content-wrapper" class="fixed-header-padding">

# <span class="text-dark">****A Student's Guide to Software Engineering Tools & Techniques »****</span>

{{ content }}
</div>
{% if page_nav %}
  <nav id="page-nav" class="fixed-header-padding">
  <div class="nav-component slim-scroll">
    <page-nav />
  </div>
  </nav>
{% endif %}
</div>
