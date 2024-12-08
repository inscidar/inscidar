---
layout: home
title: Home
---

<div class='title-panel'>
<div class='title'>
    Inclusive Science Data Accessibility Report
</div>
<div class='subtitle'>
    <span lang="en" aria-label="insider">INSCIDAR</span> (pronounced "insider")  is an open platform to share and communicate the digital accessibility states of life sciences data resources. We perform accessibility evaluation regularly and share evaluation results in the form of data, online reports, and research publications. Our mission is to increase the accessibility of life sciences data resources, lowering barriers for people with disabilities to join the life sciences workforce.

    This effort was initiated by the members of the HIDIVE Lab at Harvard Medical School (see <a href="https://hidivelab.org/research/teams/accessibility/our-accessibility-journey">Our Accessibility Journey</a>).
</div>
</div>

## Evaluation Results 

{% assign publications = site.publications %}
{% for publication in publications %}
{% include publication.html %}
{% endfor %}

## Learn More

<a href="https://hidivelab.org/research/teams/accessibility/">The Accessibility Team at the HIDIVE Lab of Harvard Medical School</a>


<a href="https://github.com/hms-dbmi/life-sciences-a11y-evaluation">The Analysis GitHub Repository</a>

<!--
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
-->
