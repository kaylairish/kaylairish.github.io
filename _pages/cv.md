---
layout: page
permalink: /cv/
title: cv
nav: true
nav_order: 5
---

<p class="cv-actions">
  <a href="{{ '/assets/pdf/Kayla_Irish_CV.pdf' | relative_url }}" target="_blank" rel="noopener noreferrer">Download</a>
</p>

<div id="cv-viewer" class="cv-viewer" data-pdf="{{ '/assets/pdf/Kayla_Irish_CV.pdf' | relative_url }}">
  <p class="cv-viewer-status">Loading CV&hellip;</p>
</div>

<noscript>
  <p><a href="{{ '/assets/pdf/Kayla_Irish_CV.pdf' | relative_url }}" target="_blank" rel="noopener noreferrer">Download Kayla Irish&rsquo;s CV (PDF)</a></p>
</noscript>

<style>
  .cv-actions {
    margin-bottom: 1rem;
  }

  .cv-viewer {
    height: min(78vh, 880px);
    overflow: auto;
    resize: vertical;
    padding: 1rem;
    border: 1px solid var(--global-divider-color);
    border-radius: 6px;
    background: var(--global-code-bg-color);
  }

  .cv-viewer-status {
    padding: 2rem 1rem;
    text-align: center;
    color: var(--global-text-color-light);
  }

  .cv-viewer-page {
    position: relative;
    margin: 0 auto 1rem;
    background: #fff;
    box-shadow: 0 1px 6px rgba(0, 0, 0, 0.18);
  }

  .cv-viewer-page:last-child {
    margin-bottom: 0;
  }

  .cv-viewer-page canvas {
    display: block;
  }

  .cv-viewer .textLayer {
    position: absolute;
    inset: 0;
    overflow: hidden;
    line-height: 1;
    text-align: initial;
    forced-color-adjust: none;
    transform-origin: 0 0;
  }

  .cv-viewer .textLayer span,
  .cv-viewer .textLayer br {
    position: absolute;
    color: transparent;
    white-space: pre;
    cursor: text;
    transform-origin: 0 0;
  }

  .cv-viewer .textLayer ::selection {
    background: rgba(0, 90, 200, 0.28);
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
    var SCALE = 1.4;

    function showLink() {
      container.style.height = "auto";
      container.style.resize = "none";
      container.innerHTML =
        '<p class="cv-viewer-status"><a href="' + pdfUrl + '" target="_blank" rel="noopener noreferrer">Download Kayla Irish’s CV (PDF)</a></p>';
    }

    if (!window.pdfjsLib) {
      showLink();
      return;
    }

    pdfjsLib.GlobalWorkerOptions.workerSrc = "https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js";

    pdfjsLib
      .getDocument(pdfUrl)
      .promise.then(function (pdf) {
        container.innerHTML = "";
        var outputScale = window.devicePixelRatio || 1;
        var chain = Promise.resolve();
        for (var i = 1; i <= pdf.numPages; i++) {
          (function (pageNum) {
            var pageEl = document.createElement("div");
            pageEl.className = "cv-viewer-page";
            pageEl.style.setProperty("--scale-factor", SCALE);
            container.appendChild(pageEl);
            chain = chain.then(function () {
              return pdf.getPage(pageNum).then(function (page) {
                var viewport = page.getViewport({ scale: SCALE });
                pageEl.style.width = Math.floor(viewport.width) + "px";
                pageEl.style.height = Math.floor(viewport.height) + "px";

                var canvas = document.createElement("canvas");
                canvas.width = Math.floor(viewport.width * outputScale);
                canvas.height = Math.floor(viewport.height * outputScale);
                canvas.style.width = Math.floor(viewport.width) + "px";
                canvas.style.height = Math.floor(viewport.height) + "px";
                pageEl.appendChild(canvas);

                var textLayer = document.createElement("div");
                textLayer.className = "textLayer";
                pageEl.appendChild(textLayer);

                return page
                  .render({
                    canvasContext: canvas.getContext("2d"),
                    viewport: viewport,
                    transform: outputScale !== 1 ? [outputScale, 0, 0, outputScale, 0, 0] : null,
                  })
                  .promise.then(function () {
                    // Text layer enables selection/copy; its failure must not blank the preview.
                    if (typeof pdfjsLib.renderTextLayer !== "function") return;
                    return page
                      .getTextContent()
                      .then(function (textContent) {
                        return pdfjsLib.renderTextLayer({
                          textContent: textContent,
                          container: textLayer,
                          viewport: viewport,
                          textDivs: [],
                        }).promise;
                      })
                      .catch(function () {});
                  });
              });
            });
          })(i);
        }
        return chain;
      })
      .catch(showLink);
  })();
</script>
