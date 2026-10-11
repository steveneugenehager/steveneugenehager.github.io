---
layout: page
title: Technical Reading
permalink: /reading/
---

Breadth-first: early chapters across many books, deeper reads only where they pay off.
Source of truth: [`_data/reading.yml`](https://github.com/steveneugenehager/steveneugenehager.github.io/blob/main/_data/reading.yml).

<div class="reading-tracker" markdown="0">
<div class="rt-filters">
<span class="rt-label">Status</span>
<button type="button" class="rt-btn is-active" data-filter="all">All</button>
<button type="button" class="rt-btn" data-filter="complete">Complete</button>
<button type="button" class="rt-btn" data-filter="planned">Planned</button>
<button type="button" class="rt-btn" data-filter="not-planned">Not planned</button>
<label class="rt-label" for="rt-topic">Topic</label>
<select id="rt-topic">
<option value="all">All topics</option>
{%- assign topics = site.data.reading | map: "topic" | uniq | sort -%}
{%- for t in topics %}
<option value="{{ t | slugify }}">{{ t }}</option>
{%- endfor %}
</select>
</div>
{%- for book in site.data.reading %}
{%- assign total = book.chapters | size -%}
{%- assign done = book.chapters | where: "status", "complete" | size -%}
{%- assign planned = book.chapters | where: "status", "planned" | size %}
<section class="rt-book" data-topic="{{ book.topic | slugify }}">
<h2><a href="{{ book.url }}">{{ book.title }}</a> <small>{{ book.edition }}</small></h2>
<p class="rt-meta">{{ book.author }} &middot; {{ book.year }} &middot; {{ book.topic }} &middot; <strong>{{ done }} of {{ total }}</strong> complete, {{ planned }} planned</p>
<progress max="{{ total }}" value="{{ done }}"></progress>
<table class="rt-table">
<thead><tr><th>#</th><th>Chapter</th><th>Status</th></tr></thead>
<tbody>
{%- for ch in book.chapters %}
<tr data-status="{{ ch.status }}"><td>{{ ch.num }}</td><td>{{ ch.name }}</td><td><span class="rt-status rt-{{ ch.status }}">{{ ch.status | replace: "-", " " }}</span></td></tr>
{%- endfor %}
</tbody>
</table>
</section>
{%- endfor %}
</div>

<style>
.reading-tracker .rt-filters { display: flex; flex-wrap: wrap; gap: .5rem; align-items: center; margin: 1rem 0 1.5rem; }
.reading-tracker .rt-label { font-size: .85em; opacity: .7; margin-left: .5rem; }
.reading-tracker .rt-btn, .reading-tracker select { font: inherit; color: inherit; background: transparent; border: 1px solid currentColor; border-radius: 4px; padding: .25rem .7rem; cursor: pointer; opacity: .65; }
.reading-tracker .rt-btn.is-active, .reading-tracker .rt-btn:hover, .reading-tracker select { opacity: 1; }
.reading-tracker .rt-meta { margin: .25rem 0 .5rem; opacity: .8; }
.reading-tracker progress { width: 100%; height: .5rem; margin-bottom: .75rem; }
.reading-tracker .rt-table { width: 100%; border-collapse: collapse; }
.reading-tracker .rt-table th, .reading-tracker .rt-table td { text-align: left; padding: .35rem .5rem; border-bottom: 1px solid rgba(128,128,128,.25); }
.reading-tracker .rt-table td:first-child { width: 3rem; opacity: .7; }
.reading-tracker .rt-status { font-size: .8em; text-transform: uppercase; letter-spacing: .04em; padding: .1rem .45rem; border-radius: 3px; white-space: nowrap; }
.reading-tracker .rt-complete { background: rgba(46,160,67,.25); }
.reading-tracker .rt-planned { background: rgba(56,139,253,.25); }
.reading-tracker .rt-not-planned { background: rgba(128,128,128,.2); }
.reading-tracker .rt-book.is-hidden, .reading-tracker tr.is-hidden { display: none; }
</style>

<script>
(function () {
  var root = document.querySelector('.reading-tracker');
  if (!root) return;
  var status = 'all', topic = 'all';
  function apply() {
    root.querySelectorAll('.rt-book').forEach(function (book) {
      var topicOk = topic === 'all' || book.dataset.topic === topic;
      var visible = 0;
      book.querySelectorAll('tbody tr').forEach(function (row) {
        var show = topicOk && (status === 'all' || row.dataset.status === status);
        row.classList.toggle('is-hidden', !show);
        if (show) visible++;
      });
      book.classList.toggle('is-hidden', visible === 0);
    });
  }
  root.querySelectorAll('.rt-btn').forEach(function (btn) {
    btn.addEventListener('click', function () {
      root.querySelectorAll('.rt-btn').forEach(function (b) { b.classList.remove('is-active'); });
      btn.classList.add('is-active');
      status = btn.dataset.filter;
      apply();
    });
  });
  document.getElementById('rt-topic').addEventListener('change', function (e) {
    topic = e.target.value;
    apply();
  });
})();
</script>
