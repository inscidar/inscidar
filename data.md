---
title: Data
permalink: /data/
layout: home
---


# Accessibility Data

This page showcases the data of the accessibility analyses and allows for easy downloading.

All data is also available on [our GitHub repository for analysis](https://github.com/hms-dbmi/life-sciences-a11y-evaluation). 

## Summary Visualizations

<ul class='plots'>
  {% assign plots = site.plots %}
  {% for plot in plots %}
  <li><a href='{{ plot.url | relative_url }}'>
    {{ plot.title }}
  </a></li>
  {% endfor %}
</ul>

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


{% assign analysis-files = site.data.analysis-files %}

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
          {% assign file-descs = analysis-files | where: 'file', file.name %}
          {% if file-descs.size > 0 %}
          {% assign file-desc = file-descs[0] %}
          {% else %}
          {% assign file-desc = '' %}
          {% endif %}
          <span class="file-title">{{ file-desc.name | default: file.name }}</span>
          <a href="{{ file.path | relative_url }}" download class="download-link" tabindex=0>
            <span class="visually-hidden">Download {{ file.name }}</span>
            <img src="{{ '/assets/icons/download.svg' | relative_url }}" alt="Download {{ file.name }}" width="24" height="24">
          </a>
        </div>
        <div id="file-content-{{ date | slugify }}-{{ file.name | slugify }}" class="file-content" hidden>
          <p>{{ file-desc.desc }}</p>
          <div class="file-preview-header">File Preview (up to 4 rows):</div>
          <table class="file-preview">
          </table>
          <div class="file-name"><code>{{ file.name }}</code></div>
        </div>
      </li>
    {% endfor %}
  </ul>
{% endfor %}

{% include cite.html %}

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
   
    // format values of each cell in the table
    const format = (str) => {
      if(+str && +str % 1 !== 0) {
        // float, e.g., str is "3.1429"
        return (+str).toFixed(3);
      } else {
        return str;
      }
    }

    // check if expanded, else, fetch content
    if (!isExpanded && !content.getAttribute('data-loaded')) {
      fetch(filePath)
        .then(response => response.text())
        .then(csvText => {
          const rows = csvText.split('\n').slice(0, 5); // max 5 rows
          const table = content.querySelector('table');
          let tableHTML = '';
          // we do not want to show the index column to save space
          const firstColNull = rows[0]?.split(',')?.[0] === "";
          rows.forEach((row, rowIndex) => {
            // to safely parse string values containing commas, replace commas with a predefined string
            const COMMA_WITHIN_DOUBLE_QUOTE = '###COMMA###';
            row = row.replace(/"(.*?)"/g, (str) => str.replaceAll(',', COMMA_WITHIN_DOUBLE_QUOTE));
            const columns = row.split(',');
            tableHTML += '<tr>';
            columns.forEach((column, colIndex) => {
              // recover the commas within each column
              column = column.replaceAll(COMMA_WITHIN_DOUBLE_QUOTE, ',');
              if(firstColNull && colIndex === 0) return;
              if (rowIndex === 0) {
                tableHTML += `<th>${column}</th>`;
              } else {
                tableHTML += `<td>${format(column)}</td>`;
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


<style>
  .visually-hidden {
    position: absolute;
    width: 1px;
    height: 1px;
    margin: -1px;
    padding: 0;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
    border: 0;
  }

  .download-list {
    padding: 0;
    margin: 20px 0;
  }

  .download-item {
    background-color: #f9f9f9;
    border: 1px solid #ddd;
    border-radius: 5px;
    padding: 15px;
    margin-bottom: 15px;
    display: flex;
    flex-direction: column;
  }

  .download-content {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  select {
    border: 1px solid #ddd;
    font-size: 14px;
  }

  .toggle-button, .download-link {
    background-color: transparent;
    border: none;
    cursor: pointer;
    display: inline-flex;
    padding: 10px;
  }

  .toggle-button:hover, .toggle-button:focus, 
  .download-link:hover, .download-link:focus {
    outline: 3px solid #005fcc;
  }

  .file-name {
    font-style: italic;
    display: inline-block;
    text-align: right;
    width: 100%;
    color: grey;
  }

  .file-header {
    display: flex;
    align-items: center;
  }

  .file-preview-header {
    color: grey;
  }

  .file-preview {
    width: calc(100% - 20px);
    max-width: calc(100% - 20px);
    border-collapse: collapse;
    margin: 20px;
    margin-bottom: 8px;
    margin-right: 0px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
    position: relative;
    overflow: hidden;
    border-radius: 10px;
    background-color: #ffffff;
    border: 2px solid #cc79a7;
    font-size: 12px;
  }

  .file-preview th, .file-preview td {
    padding: 8px;
    text-align: left;
    border: 1px solid #cc79a7;
  }

  .file-preview th {
    color: #333333;
    font-weight: bold;
  }

  .file-preview tr:nth-child(even) {
    background-color: #f9f9f9;
  }

  .file-preview tr:hover {
    background-color: #f1f1f1;
  }

  .file-preview td {
    color: #555555;
  }

  .file-preview::after {
    content: '';
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    height: 100px;
    background: linear-gradient(to bottom, rgba(255, 255, 255, 0) 80%, rgba(255, 255, 255, 1) 100%);
    pointer-events: none;
  }

  .download-button {
    display: inline-block;
    padding: 10px 15px;
    margin-top: 10px;
    background-color: #4caf50;
    color: white;
    border: none;
    border-radius: 5px;
    text-decoration: none;
    font-size: 14px;
  }

  .download-button:hover {
    background-color: #45a049;
  }
</style>
