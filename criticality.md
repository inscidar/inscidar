---
title: Issue Criticality
permalink: /issues/
layout: home
---

<link rel="stylesheet" href="/assets/css/data.css">

# Criticality of Accessibility Issues

In our accessibility analysis, we used [Axe](https://github.com/dequelabs/axe-core) to collect the accessibility states of target web pages. To improve the interpretability of the issues defined by Axe for our study result analysis, we reviewed individual issues and assigned more informative categories, such as criticalities and potential impacts.


<span class="file-title">issues.csv</span>
<a href="{{ '/assets/issues.csv' | relative_url }}" download class="download-link" tabindex="0">
  <span class="visually-hidden">Download issues.csv</span>
  <img src="{{ '/assets/icons/download.svg' | relative_url }}" alt="Download issues.csv" width="24" height="24">
</a>

<div id="issues-table" tabindex="-1">
  <table class="file-preview"></table>
</div>

<script src="https://cdn.jsdelivr.net/npm/papaparse@5.5.2/papaparse.min.js"></script>

<script>
  fetch('/assets/issues.csv')
    .then(response => response.text())
    .then(csvText => {
      const parsed = Papa.parse(csvText.trim(), { header: false });
      const rows = parsed.data;
      const table = document.querySelector('table');

      const caption = document.createElement('caption');
      caption.textContent = 'AXE Issues Table: Criticality and Impact';
      table.prepend(caption);

      // not using innerHTML because some of the file cells have HTML tags that are not escaped
      // header
      const thead = table.createTHead();
      const headerRow = thead.insertRow();
      rows[0].forEach(header => {
        const th = document.createElement('th');
        th.textContent = header;
        th.scope = "col";
        headerRow.appendChild(th);
      });

      // body
      const tbody = table.createTBody();
      rows.slice(1).forEach(row => {
        const tr = tbody.insertRow();
        row.forEach((cell, columnIndex) => {
          const td = document.createElement('td');

          if (columnIndex === 10 && cell.startsWith('http')) {
            const a = document.createElement('a');
            a.href = cell;
            a.setAttribute('aria-label', 'Visit Deque University explanation');
            a.tabIndex = 0
            a.textContent = cell;
            td.appendChild(a);
          } else {
            td.textContent = cell;
          }

          tr.appendChild(td);
        });
      });
    }).catch(error => console.error('Error fetching CSV file:', error));

    document.getElementById('issues-table').focus();
</script>
