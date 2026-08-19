---
layout: default
---

<div class="website-shell">
  <aside class="site-sidebar" aria-label="Site navigation">
    <div class="menu-icon" aria-hidden="true">
      <span></span>
      <span></span>
      <span></span>
    </div>

    <div class="sidebar-brand">
      <span>Google Summer of Code</span>
      <img src="{{ '/GSoC.png' | relative_url }}" alt="Google Summer of Code">
    </div>
  </aside>

  <main class="readme-card">
    <div class="readme-content">
      {% include_relative README.md %}
    </div>
  </main>
</div>
