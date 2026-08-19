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

  <details class="contact-menu">
    <summary class="contact-button">
      Contact Me
    </summary>

    <div class="contact-dialog">

      <a
        href="https://www.linkedin.com/in/yash-hosalli-70518a336/"
        target="_blank"
        rel="noopener noreferrer"
        class="contact-option"
      >
        <span class="contact-icon linkedin-icon">in</span>
        <span>LinkedIn</span>
      </a>

      <a
        href="https://github.com/Victowolf"
        target="_blank"
        rel="noopener noreferrer"
        class="contact-option"
      >
        <span class="contact-icon github-icon">↗</span>
        <span>GitHub</span>
      </a>

    </div>
  </details>

  <main class="readme-card">
    <div class="readme-content">
{% capture readme_content %}{% include_relative README.md %}{% endcapture %}
{{ readme_content | markdownify }}
    </div>
  </main>

</div>