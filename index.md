---
layout: default
---

<div class="website-shell">

  <aside class="site-sidebar">
    <div class="sidebar-image-wrapper">
      <img
        src="{{ '/GSoC.png' | relative_url }}"
        alt="Google Summer of Code"
      >
    </div>
  </aside>

  <main class="readme-card">
    <div class="readme-content">
      {% capture readme_content %}
      {% include_relative README.md %}
      {% endcapture %}

      {{ readme_content | markdownify }}
    </div>
  </main>

</div>