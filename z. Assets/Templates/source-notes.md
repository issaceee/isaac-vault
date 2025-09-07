---
category: literaturenote  
tags: {% if allTags %}{{allTags}}{% endif %} 
citekey: {{citekey}}  
status: unread  
dateread:  
---
  
> [!Cite]  
> {{bibliography}}  
  
>[!Synth]  
>**Contribution**::  
>  
>**Related**:: {% for relation in relations | selectattr("citekey") %} [[@{{relation.citekey}}]]{% if not loop.last %}, {% endif%} {% endfor %}  
>  
  
>[!md]  
{% for type, creators in creators | groupby("creatorType") -%}  
{%- for creator in creators -%}  
> **{{"First" if loop.first}}{{type | capitalize}}**::  
{%- if creator.name %} {{creator.name}}  
{%- else %} {{creator.lastName}}, {{creator.firstName}}  
{%- endif %}  
{% endfor %}~  
{%- endfor %}  
> **Title**:: {{title}}  
> **Year**:: {{date | format("YYYY")}}  
> **Citekey**:: {{citekey}} {%- if itemType %}  
> **itemType**:: {{itemType}}{%- endif %}{%- if itemType == "journalArticle" %}  
> **Journal**:: *{{publicationTitle}}* {%- endif %}{%- if volume %}  
> **Volume**:: {{volume}} {%- endif %}{%- if issue %}  
> **Issue**:: {{issue}} {%- endif %}{%- if itemType == "bookSection" %}  
> **Book**:: {{publicationTitle}} {%- endif %}{%- if publisher %}  
> **Publisher**:: {{publisher}} {%- endif %}{%- if place %}  
> **Location**:: {{place}} {%- endif %}{%- if pages %}  
> **Pages**:: {{pages}} {%- endif %}{%- if DOI %}  
> **DOI**:: {{DOI}} {%- endif %}{%- if ISBN %}  
> **ISBN**:: {{ISBN}} {%- endif %}  
  
> [!LINK]  
> {%- for attachment in attachments | filterby("path", "endswith", ".pdf") %}  
> [{{attachment.title}}](file://{{attachment.path | replace(" ", "%20")}}) {%- endfor -%}.  
  
> [!Abstract]  
> {%- if abstractNote %}  
> {{abstractNote}}  
> {%- endif -%}.  
>
# Notes  
{% persist "notes" %}{% if isFirstImport %}
Write notes here!
{% endif %} 
{% endpersist %}
  
# Annotations
{%- macro calloutHeader(type, color) -%}  
  {%- if type == "highlight" -%}  
    {%- if color == "#ff6666" -%}  
      <mark style="background-color: {{color}}">Disagree</mark>  
    {%- elif color == "#ffd400" -%}  
      <mark style="background-color: {{color}}">Important / Interesting</mark>  
    {%- elif color == "#5fb236" -%}  
      <mark style="background-color: {{color}}">Agree</mark>  
    {%- elif color == "#a28ae5" -%}  
      <mark style="background-color: {{color}}">Chapter / Section</mark>  
    {%- elif color == "#2ea8e5" -%}  
      <mark style="background-color: {{color}}">Related Sources</mark>  
    {%- elif color == "#ff8ebf" -%}  
      <mark style="background-color: {{color}}">?? Confused</mark>  
    {%- elif color == "#ffb86c" -%}  
      <mark style="background-color: {{color}}">Definition</mark>  
    {%- else -%}  
      <mark style="background-color: {{color}}">Highlight</mark>  
    {%- endif -%}  
  {%- endif -%}  

  {%- if type == "text" -%}  
    Note  
  {%- endif -%}  
{%- endmacro -%}  

{% persist "annotations" %}
{% set newAnnotations = annotations | filterby("date", "dateafter", lastImportDate) %}
{% if newAnnotations.length > 0 %}

### Imported: {{importDate | format("YYYY-MM-DD h:mm a")}}

{% for a in newAnnotations %}
{{calloutHeader(a.type, a.color)}}
> {{a.annotatedText}}

{% if a.comment %}
>  
> Comment: {{a.comment | replace("\n", "\n> ") }}
{% endif %}

{% if a.imageRelativePath %}
![[{{a.imageRelativePath}}]]
{% endif %}

<br>
{% endfor %}

{% endif %}
{% endpersist %}
