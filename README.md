{% extends "base.html" %}
{% block content %}
  <h1>All Publications</h1>

  {% if publications %}
    <table class="pub-table">
      <thead>
        <tr>
          <th>#</th>
          <th>Title</th>
          <th>Authors</th>
          <th>Journal / Conference</th>
          <th>Year</th>
          <th>DOI</th>
          <th>Abstract</th>
          <th>PDF</th>
        </tr>
      </thead>
      <tbody>
        {% for pub in publications %}
          <tr>
            <td>{{ loop.index }}</td>
            <td>{{ pub.title }}</td>
            <td>{{ pub.authors }}</td>
            <td>{{ pub.journal }}</td>
            <td>{{ pub.year }}</td>
            <td>
              {% if pub.doi %}
                <a href="https://doi.org/{{ pub.doi }}" target="_blank">{{ pub.doi }}</a>
              {% else %}
                N/A
              {% endif %}
            </td>
            <td>
              {% if pub.abstract %}
                <button class="btn-small" onclick="toggleAbstract({{ pub.id }})">View</button>
                <div id="abstract-{{ pub.id }}" class="abstract-popup hidden">
                  {{ pub.abstract }}
                  <button onclick="toggleAbstract({{ pub.id }})">Close</button>
                </div>
              {% else %}
                N/A
              {% endif %}
            </td>
            <td>
              {% if pub.pdf_filename %}
                <a href="{{ url_for('download_file', filename=pub.pdf_filename) }}" target="_blank">Download</a>
              {% else %}
                N/A
              {% endif %}
            </td>
          </tr>
        {% endfor %}
      </tbody>
    </table>
  {% else %}
    <p>No publications have been added yet. <a href="{{ url_for('upload_publication') }}">Upload one now.</a></p>
  {% endif %}

  <script>
    function toggleAbstract(id) {
      const el = document.getElementById("abstract-" + id);
      if (el.classList.contains("hidden")) {
        el.classList.remove("hidden");
      } else {
        el.classList.add("hidden");
      }
    }
  </script>
{% endblock %}
