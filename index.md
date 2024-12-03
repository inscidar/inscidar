---
layout: home
title: Home
---

<div class='title-panel'>
<div class='title'>
    Inclusive Science Data Accessibility Report
</div>
<div class='subtitle'>
    An open database and data portal for storing and keeping track of the digital accessibility of life science data resources.
</div>
</div>

<div id='visual-statistics'>
    <div id='statistics-num-pages'></div>
    <div></div>
</div>

## Publications

Sehi L’Yi, Harrison G Zhang, Andrew P Mar, Thomas C Smits, Lawrence Weru, Sofía Rojas, Alexander Lex, Nils Gehlenborg.
A comprehensive evaluation of life sciences data resources reveals significant accessibility barriers,
OSF Preprints, 10.31219/osf.io/5v98j

<script type="text/javascript">

    const numPages = "{{ "/assets/plots/statistics-num-pages.json" | relative_url }}";
    
    const vegaOptions = {
        renderer: "canvas",
        actions: true,
    };

    window.onload = () => {
        vegaEmbed(
            `#statistics-num-pages`,
            numPages,
            vegaOptions
        );
    }
</script>
