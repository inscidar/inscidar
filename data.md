---
title: Data
permalink: /data/
layout: home
---

<link rel="stylesheet" href="/assets/css/data.css">

# Accessibility Data

This page showcases the data of the accessibility analyses and allows for easy downloading.

All data is also available on [our GitHub repository for analysis](https://github.com/hms-dbmi/life-sciences-a11y-evaluation). 


<!-- Retrieve all unique dates -->
{% assign csv_files_all = site.static_files | where_exp: "file", "file.path contains '/assets/csv' and file.extname == '.csv'" %}
{% assign file_paths = csv_files_all | map: "path" %}
{% assign unique_dates = "" %}
{% for file_path in file_paths %}
  {% assign file_path_array = file_path | split: "/" %}
  {% assign date = file_path_array[3] %}
  {% assign unique_dates = unique_dates | append: " + " | append: date %}
{% endfor %}
{% assign unique_dates = unique_dates | split: " + " | shift | uniq | reverse %}

<!-- Retrieve all unique file names -->
{% assign unique_filenames = "" %}
{% for file_path in file_paths %}
  {% assign file_path_array = file_path | split: "/" %}
  {% assign filename_with_extension = file_path_array[4] | split: ".csv" %}
  {% assign filename = filename_with_extension[0] %}
  {% assign unique_filenames = unique_filenames | append: " + " | append: filename %}
{% endfor %}
{% assign unique_filenames = unique_filenames | split: " + " | shift | uniq | sort %}


## Download in batch
<ul class="download-list">
  <li class="download-item">
    <div class="download-content">
      <span>Download all files</span>
      <a href="{{ "/assets/csv/zips/all/a11y-evaluations.zip" | relative_url }}" download class="download-link" tabindex=0>
        <span class="visually-hidden">Download all files as zip</span>
        <img src="{{ '/assets/icons/download.svg' | relative_url }}" alt="Download all files" width="24" height="24">
      </a>
    </div>
  </li>
  <li class="download-item">
    <div class="download-content">
      <span>Download all files with filename</span>
      <label for="filename">Choose a filename:</label>
      <select name="filename" id="filename">
        <option value="" disabled selected>Select your option</option>
        {% for filename in unique_filenames %}
        <option value="{{ filename }}">{{ filename }}</option>
        {% endfor %}
      </select>
      <a href="#" download class="download-link" id="download-filename" tabindex=0>
        <span class="visually-hidden">Download all files with selected filename as zip</span>
        <img src="{{ '/assets/icons/download.svg' | relative_url }}" alt="Download all files with selected name" width="24" height="24">
      </a>
    </div>
  </li>
  <li class="download-item">
    <div class="download-content">
      <span>Download all files with date</span>
      <label for="date">Choose a date:</label>
      <select name="date" id="date">
        <option value="" disabled selected>Select your option</option>
        {% for date in unique_dates %}
        <option value="{{ date }}">{{ date }}</option>
        {% endfor %}
      </select>
      <a href="#" download class="download-link" id="download-date" tabindex=0>
        <span class="visually-hidden">Download all files with selected date as zip</span>
        <img src="{{ '/assets/icons/download.svg' | relative_url }}" alt="Download all files with selected date" width="24" height="24">
      </a>
    </div>
  </li>
</ul>


## View single analyses
<!-- list files for each unique date -->
{% for date in unique_dates %}
  <h3>Analysis {{ date }}</h3>
  <ul class="file-list">
    {% assign csv_files = csv_files_all | where_exp: "file", "file.path contains date" %}

    {% for file in csv_files %}
      <li class="file-item">
        <div class="file-header">
          <button aria-expanded="false" aria-controls="file-content-{{ date | slugify }}-{{ file.name | slugify }}" class="toggle-button" onclick="toggleContent('{{ date | slugify }}-{{ file.name | slugify }}', '{{ file.path | relative_url }}')">
            <span class="visually-hidden">Expand section for {{ file.name }}</span>
            <img id="icon-{{ date | slugify }}-{{ file.name | slugify }}" src="{{ '/assets/icons/triangle-right.svg' | relative_url }}" alt="Expand section for {{ file.name }}" width="16" height="16">
          </button>
          <span class="file-title">{{ file.name }}</span>
          <a href="{{ file.path | relative_url }}" download class="download-link" tabindex=0>
            <span class="visually-hidden">Download {{ file.name }}</span>
            <img src="{{ '/assets/icons/download.svg' | relative_url }}" alt="Download {{ file.name }}" width="24" height="24">
          </a>
        </div>
        <div id="file-content-{{ date | slugify }}-{{ file.name | slugify }}" class="file-content" hidden>
          <table class="file-preview">
          </table>
        </div>
      </li>
    {% endfor %}
  </ul>
{% endfor %}

## Summary Visualizations

<div class='plots'>
  {% assign plots = site.plots %}
  {% for plot in plots %}
  <a href='{{ plot.url | relative_url }}'>
    {{ plot.title }}
  </a>
  {% endfor %}
</div>


## Publications

Sehi L'Yi, Harrison G Zhang, Andrew P Mar, Thomas C Smits, Lawrence Weru, Sofía Rojas, Alexander Lex, Nils Gehlenborg. A comprehensive evaluation of life sciences data resources reveals significant accessibility barriers, OSF Preprints, [10.31219/osf.io/5v98j](https://doi.org/10.31219/osf.io/5v98j)

<script>
  const filenameDownload = document.getElementById('download-filename');
  const filenameSelect = document.getElementById('filename');

  filenameSelect.addEventListener('change', function() {
    const selectedFilename = filenameSelect.value;
    filenameDownload.href = `{{ '/assets/csv/zips/by_name/' | relative_url }}${selectedFilename}.zip`;
  });

  const dateDownload = document.getElementById('download-date');
  const dateSelect = document.getElementById('date');

  dateSelect.addEventListener('change', function() {
    const selectedDate = dateSelect.value;
    dateDownload.href = `{{ '/assets/csv/zips/by_date/' | relative_url }}${selectedDate}.zip`;
  });

  function toggleContent(id, filePath) {
    const content = document.getElementById(`file-content-${id}`);
    const button = document.querySelector(`[aria-controls="file-content-${id}"]`);
    const icon = document.getElementById(`icon-${id}`);
    
    const isExpanded = button.getAttribute("aria-expanded") === "true";
    
    // change aria state
    button.setAttribute("aria-expanded", !isExpanded);
    content.hidden = isExpanded;

    // Update hidden button text for screen readers
    const srText = button.querySelector('.visually-hidden');
    srText.textContent = isExpanded ? `Expand section for File${id}` : `Collapse section for File${id}`;

    // change icons
    icon.src = isExpanded 
      ? "{{ '/assets/icons/triangle-right.svg' | relative_url }}" 
      : "{{ '/assets/icons/triangle-down.svg' | relative_url }}";
    
    // check if expanded, else, fetch content
    if (!isExpanded && !content.getAttribute('data-loaded')) {
      fetch(filePath)
        .then(response => response.text())
        .then(csvText => {
          const rows = csvText.split('\n').slice(0, 5); // max 5 rows
          const table = content.querySelector('table');
          let tableHTML = '';
          
          rows.forEach((row, rowIndex) => {
            const columns = row.split(',');
            tableHTML += '<tr>';
            columns.forEach(column => {
              if (rowIndex === 0) {
                tableHTML += `<th>${column}</th>`;
              } else {
                tableHTML += `<td>${column}</td>`;
              }
            });
            tableHTML += '</tr>';
          });
          
          table.innerHTML = tableHTML;
          content.setAttribute('data-loaded', 'true');
        })
        .catch(error => console.error('Error fetching CSV file:', error));
    }
  }
</script>
