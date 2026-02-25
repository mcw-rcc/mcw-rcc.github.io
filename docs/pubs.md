# Publications supported by Research Computing

Acknowledging Research Computing is easy to do and helps everyone understand how important RCC resources are to you and MCW.

All you have to do is include the following statement in your publication:

!!! example "Acknowledgement"

    This research was completed in part with computational resources and technical support provided by the Research Computing Center at the Medical College of Wisconsin.

## List by year

<!-- markdownlint-disable MD033 -->
{# -------------------- Year Sections -------------------- #}
{# Render newest year first; sort by date descending within each year #}
{% for y in publications.years | sort(attribute='year', reverse=True) %}

### <a id="{{ y.year }}"></a>{{ y.year }}

{% set pubs_sorted = (y.pubs | default([]))
   | sort(attribute='date', reverse=True) %}

{% if pubs_sorted | length == 0 %}
_No publications listed for {{ y.year }}._
{% else %}

{% for p in pubs_sorted %}
<div class="pub">
  <div class="title">
    {% if p['doi'] %}
      <a href="{{ p['doi'] }}" target="_blank"><strong>{{ p.title }}</strong></a>
    {% else %}
      <strong>{{ p.title }}</strong>
    {% endif %}
  </div>
  {% if p['authors'] %}
  <div class="author">{{ p['authors'] | trim }}</div><br />
  {% endif %}
</div>
{% endfor %}
{% endif %}
{% endfor %}

## Summary

{# -------------------- Totals -------------------- #}
{# Sum counts instead of concatenating lists (faster and safer) #}
{% set all_pub_counts = publications.years
   | map(attribute='pubs')
   | map('default', [])
   | map('length')
   | sum %}

Research Computing has supported research resulting in **{{ all_pub_counts }}** publications over **{{ publications.years | count }}** years. 

{# -------------------- Data prep for xychart -------------------- #}
{% set years_sorted_asc = publications.years | sort(attribute='year') %}
{% set counts = years_sorted_asc
   | map(attribute='pubs')
   | map('default', [])
   | map('length')
   | list %}
{% set max_count = (counts | max) if counts else 0 %}
{# Add headroom so tallest bar doesn't hit the top; minimum top is 5 #}
{% set y_top = (max_count + 2) if max_count < 10 else (max_count + 5) %}
{% set y_top = 5 if y_top < 5 else y_top %}

<!-- markdownlint-disable MD046 -->
``` mermaid
xychart title "Publications per Year"
    x-axis [{% for y in years_sorted_asc %}{{ '"' ~ y.year ~ '"' }}{% if not loop.last %}, {% endif %}{% endfor %}]
    y-axis "Count" 0 --> {{ y_top }}
    bar [{% for c in counts %}{{ c }}{% if not loop.last %}, {% endif %}{% endfor %}]
```
<!-- markdownlint-enable MD046 -->
