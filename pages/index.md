---
layout: page
title: Docsy Jekyll Theme
permalink: /
---

# Welcome to Docsy Jekyll

This is a starter template for a docsy jekyll theme.


## 1.	Gerar a oferta (POST /prospects – Short Form)

<p>Nessa etapa é enviado uma requisição para gerar uma oferta. As verificações feitas são análises de crédito e variam dependendo da política especificada para cada produto/serviço. No retorno da requisição, no header, terá um campo chamado Location que constará o id da oferta gerada. </p>
<p>Dados obrigatórios: CPF, nome, data de nascimento, CEP, celular, canal, user registration (número da matricula do usuário), metadados (dados do navegador/app para verificação). </p>

<b>Endpoints: </b>

+ Homologação: https://apihlg-portoseg.sensedia.com/dev/proposal/v1/prospects
+ Produção: https://api-portoseg.sensedia.com/proposal/v1/prospects

## Features

What are these features? You should see the {% include doc.html name="Getting Started" path="getting-started" %}
guide for a complete summary. Briefly:

 - *User interaction* including consistent permalinks, links to ask questions via GitHub issues, and edit the file on GitHub directly.
 - *Search* across posts, documentation, and other site pages, with an ability to exclude from search.
 - *External Search* meaning an ability to link any page tag to trigger an external search.
 - *Documentation* A documentation collection that was easy to organize on the filesystem, render with nested headings for the user, and refer to in markdown.
 - *Pages* A separate folder for more traditional pages (e.g, about).
 - *Navigation*: Control over the main navigation on the left of the page, and automatic generation of table of contents for each page on the right.
 - *News* A posts feed for news and updates, along with an archive (organized by year).
 - *Templates* or specifically, "includes" that make it easy to create an alert, documentation link, or other content.
 - *Continuous Integration* recipes to preview the site


For features, getting started with development, see the {% include doc.html name="Getting Started" path="getting-started" %} page. Would you like to request a feature or contribute?
[Open an issue]({{ site.repo }}/issues)
