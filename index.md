---
layout: home
title: Home
---

<div class='title-panel'>
<div class='title'>
    Interactive Science Data Accessibility Report
</div>
<div class='subtitle'>
    <dfn>INSCIDAR</dfn> (pronounced "insider") is an open platform to share and communicate the digital accessibility states of life sciences data resources. We perform accessibility evaluation regularly and share evaluation results in the form of data, online reports, and research publications. Our mission is to increase the accessibility of life sciences data resources, lowering barriers for people with disabilities to join the life sciences workforce.

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

## Funding

This study was in part funded by NIH grants <a href="https://reporter.nih.gov/project-details/10452031">R01HG011773</a> and <a href="https://reporter.nih.gov/project-details/10984200">K99HG013348</a>.

{% include cite.html %}
