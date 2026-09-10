---
layout: page
permalink: /cv/
title: cv
nav: true
nav_order: 5
description: <a href="/assets/pdf/Kayla_Irish_CV.pdf" target="_blank" rel="noopener noreferrer">Open in a new tab / download &rarr;</a>
---

<div id="cv-viewer" class="cv-viewer" data-pdf="{{ '/assets/pdf/Kayla_Irish_CV.pdf' | relative_url }}">
  <p class="cv-viewer-status">Loading CV&hellip;</p>
</div>

<noscript>
  <p><a href="{{ '/assets/pdf/Kayla_Irish_CV.pdf' | relative_url }}" target="_blank" rel="noopener noreferrer">View Kayla Irish&rsquo;s CV (PDF) &rarr;</a></p>
</noscript>

<style>
  .cv-viewer {
    max-width: 100%;
  }

  .cv-viewer-status {
    padding: 2rem 1rem;
    text-align: center;
    color: var(--global-text-color-light);
  }

  .cv-viewer-page {
    margin: 0 auto 1.25rem;
    max-width: 850px;
    border: 1px solid var(--global-divider-color);
    border-radius: 4px;
    overflow: hidden;
    box-shadow: 0 1px 6px rgba(0, 0, 0, 0.08);
  }

  .cv-viewer-page canvas {
    display: block;
    width: 100%;
    height: auto;
  }
</style>

<script
  src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"
  integrity="sha512-q+4liFwdPC/bNdhUpZx6aXDx/h77yEQtn4I1slHydcbZK34nLaR3cAeYSJshoxIOq3mjEf7xJE8YWIUHMn+oCQ=="
  crossorigin="anonymous"
  referrerpolicy="no-referrer"
></script>
<script>
  (function () {
    var container = document.getElementById("cv-viewer");
    if (!container) return;
    var pdfUrl = container.getAttribute("data-pdf");

    function showLink() {
      container.innerHTML =
        '<p class="cv-viewer-status"><a href="' +
        pdfUrl +
        '" target="_blank" rel="noopener noreferrer">View Kayla Irish’s CV (PDF) →</a></p>';
    }

    if (!window.pdfjsLib) {
      showLink();
      return;
    }

    pdfjsLib.GlobalWorkerOptions.workerSrc =
      "https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js";

    pdfjsLib
      .getDocument(pdfUrl)
      .promise.then(function (pdf) {
        container.innerHTML = "";
        var dpr = window.devicePixelRatio || 1;
        var chain = Promise.resolve();
        for (var i = 1; i <= pdf.numPages; i++) {
          (function (pageNum) {
            var pageEl = document.createElement("div");
            pageEl.className = "cv-viewer-page";
            container.appendChild(pageEl);
            chain = chain.then(function () {
              return pdf.getPage(pageNum).then(function (page) {
                var viewport = page.getViewport({ scale: 1.6 });
                var canvas = document.createElement("canvas");
                canvas.width = Math.floor(viewport.width * dpr);
                canvas.height = Math.floor(viewport.height * dpr);
                pageEl.appendChild(canvas);
                return page.render({
                  canvasContext: canvas.getContext("2d"),
                  viewport: viewport,
                  transform: dpr !== 1 ? [dpr, 0, 0, dpr, 0, 0] : null,
                }).promise;
              });
            });
          })(i);
        }
        return chain;
      })
      .catch(showLink);
  })();
</script>
