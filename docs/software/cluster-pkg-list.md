# Cluster software list

Last updated: _{{ git_revision_date_localized }}_
<!-- 
// Copyright 2014-2022 Stanford Research Computing Center
//
// The following code is a derivative work of https://raw.githubusercontent.com/stanford-rc/www.sherlock.stanford.edu/main/src/docs/software/list.md, which is licensed GPLv3. This code therefore is also licensed under the terms GPLv3.
-->
## Categories

{% set all_packages =  software_modules.categories |
                       map(attribute='packages') | sum(start=[]) %}

{% set all_fields   =  all_packages |
                       unique(attribute='categories') | list %}

_We currently provide {{ all_packages | count }} software modules, in {{software_modules.categories | count }} categories, covering {{ all_fields | count }} fields of science:_

{% for c in software_modules.categories|sort(attribute='name') %}

<!-- markdownlint-disable MD033 -->
* [`{{ c.name }}`](#{{ c.name }}) <small>
    {{ c.packages | map(attribute='categories')
                  | map('replace', c.name ~ ', ', '')
                  | unique | sort | join(', ') }}
  </small>
{% endfor %}

{% macro module_line(p) -%}
    **{{ p.categories.split(',') | last | trim }}** | `{{- p.package }}` |
    {%- for v in p.versions | sort(attribute="markedDefault", reverse=true) -%}
      `{{ v.versionName }}`<br/>
    {%- endfor -%}
    | [Website]({{ p.url }}) | {{ p.description }}
{%- endmacro %}

{% for c in software_modules.categories|sort(attribute='name') %}

### **{{ c.name  }}**
<!-- markdownlint-disable MD045 MD056 MD058 -->
Field | Module Name<img style="min-width:110px"/> | Version(s)<img style="min-width:90px"/> | URL | Description
:---- | :----------- | :----------- | :-- | :----------
  {% for p in c.packages | sort(attribute="categories,package") -%}
    {{ module_line(p) }}
  {% endfor -%}

{% endfor %}
