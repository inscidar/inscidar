---
title: Issue Criticality
permalink: /issues/
layout: home
---

<link rel="stylesheet" href="/assets/css/data.css">

# Criticality and Impact of AXE Issues
For each AXE issue, we classified the impact and criticality based on it's WCAG level and difficulty to fix. 


<span class="file-title">issues.csv</span>
<a href="{{ '/assets/issues.csv' | relative_url }}" download class="download-link" tabindex="0">
  <span class="visually-hidden">Download issues.csv</span>
  <img src="{{ '/assets/icons/download.svg' | relative_url }}" alt="Download issues.csv" width="24" height="24">
</a>

<div id="issues-table" class="file-preview-container">
  <table class="file-preview"></table>
</div>


<script>
  fetch('/assets/issues.csv')
    .then(response => response.text())
    .then(csvText => {
      const rows = csvText.trim().split('\n')
      const table = document.createElement('table');

      let tableHTML = '';

      table.innerHTML = tableHTML;

      document.getElementById('issues-table').appendChild(table);
    }).catch(error => console.error('Error fetching CSV file:', error));
</script>
