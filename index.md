---
layout: default
---
<div class="jumbotron">
      <div class="container">
        <h1>Write node APIs from your laptop</h1>
        <p>This is a template for a simple marketing or informational website. It includes a large callout called a jumbotron and three supporting pieces of content. Use it as a starting point to create something more unique.</p>
        <p><a class="btn btn-primary btn-lg" href="#" role="button">Learn more »</a></p>
      </div>
    </div>

## Overview

Apigee-127 is toolkit for modeling & building rich, enterprise-class APIs in Node.js on your laptop.  The focal point of Apigee-127 is the Swagger 2.0 specification for defining and describing an API model.  From the Swagger model you can generate clients, servers and interactive documentation for your API.

## Contents
{% for page in site.pages %}
  {% if page.title %}
    {% if page.root %}
  <a class="page-link" href="{{ page.url | prepend: site.baseurl }}">{{ page.title }}</a>
     {% endif %}
  {% endif %}
{% endfor %}