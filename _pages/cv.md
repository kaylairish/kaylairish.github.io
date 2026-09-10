---
layout: page
permalink: /cv/
title: cv
nav: true
nav_order: 5
description: <a href="/assets/pdf/Kayla_Irish_CV.pdf" target="_blank" rel="noopener noreferrer">Open in a new tab / download &rarr;</a>
---

{% assign cv_url = '/assets/pdf/Kayla_Irish_CV.pdf' | relative_url %}
<object data="{{ cv_url }}#view=FitH" type="application/pdf" class="cv-embed">
  <div class="cv-embed-fallback">
    <p>Your browser can&rsquo;t display the PDF here.</p>
    <p><a href="{{ cv_url }}" target="_blank" rel="noopener noreferrer">View Kayla Irish&rsquo;s CV (PDF) &rarr;</a></p>
  </div>
</object>

<style>
  .cv-embed {
    display: block;
    width: 100%;
    height: min(1100px, 85vh);
    border: 1px solid var(--global-divider-color);
    border-radius: 6px;
  }
  .cv-embed-fallback {
    padding: 2rem 1rem;
    text-align: center;
  }
</style>
