{% extends "base.html" %}
{% block content %}
  <h1>Upload a New Publication</h1>
  <form method="POST" enctype="multipart/form-data">
    {{ form.hidden_tag() }}

    <div class="form-group">
      {{ form.title.label }}<br />
      {{ form.title(size=80) }}
      {% for err in form.title.errors %}
        <span class="text-error">{{ err }}</span>
      {% endfor %}
    </div>

    <div class="form-group">
      {{ form.authors.label }}<br />
      {{ form.authors(size=80, placeholder="e.g. Alice Smith, Bob Jones") }}
      {% for err in form.authors.errors %}
        <span class="text-error">{{ err }}</span>
      {% endfor %}
    </div>

    <div class="form-group">
      {{ form.journal.label }}<br />
      {{ form.journal(size=80) }}
      {% for err in form.journal.errors %}
        <span class="text-error">{{ err }}</span>
      {% endfor %}
    </div>

    <div class="form-group">
      {{ form.year.label }}<br />
      {{ form.year(min=1900, max=2100) }}
      {% for err in form.year.errors %}
        <span class="text-error">{{ err }}</span>
      {% endfor %}
    </div>

    <div class="form-group">
      {{ form.doi.label }}<br />
      {{ form.doi(size=80) }}
      {% for err in form.doi.errors %}
        <span class="text-error">{{ err }}</span>
      {% endfor %}
    </div>

    <div class="form-group">
      {{ form.abstract.label }}<br />
      {{ form.abstract(rows=5, cols=80) }}
      {% for err in form.abstract.errors %}
        <span class="text-error">{{ err }}</span>
      {% endfor %}
    </div>

    <div class="form-group">
      {{ form.pdf_file.label }}<br />
      {{ form.pdf_file() }}
      {% for err in form.pdf_file.errors %}
        <span class="text-error">{{ err }}</span>
      {% endfor %}
    </div>

    <div class="form-group">
      {{ form.submit(class="btn-primary") }}
    </div>
  </form>
{% endblock %}
