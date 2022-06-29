# The Jahia Nextjs initiative : *Jahia the DXP to manage headless web project*

The aim of the Jahia Nextjs initiative is to explore and explain
the Jahia capabilities to easily create and manage headless web project.
Solutions we use are :
- [Jahia][jahia-website] : a Cloud / On-premise *DXP* solution to create and contribute
- [Vercel][vercel-website] a next-js Cloud platform provider to render the web project

### Table of Contents :

- [Initiative overview](#initiative-overview)
  - [Headless challenges : Edit and Preview](#headless-challenges--edit-and-preview)
- [Why we decide this initiative](#why-we-decide-this-initiative)
- [Modules involved](#modules-involved)
- [Set up the project](#set-up-the-project)
- [Architecture](#architecture)

## Initiative overview

To be the best, Jahia takes care of both side of a project, the development and the
content contribution.
One of the key factor success of a web project is the facility for each side to
execute their task. 

|Developers|Web Contributors|
|---|---|
|A web project is easy to create and maintain when they have an SDK in their preferred languages, and they can create, update, test and deploy component quickly.| A web project is easy to contribute and maintain when they understand the web site page structure and when they can create a content by clicking a button or just copy past a WYSIWYG content in the web page.|
| ![000] | ![001] |

### Headless challenges : Edit and Preview 

With headless CMS, editing and previewing HTML page is not trivial for developers and
contributors :
- from developer point of view, you have to add a specific button to your app, to enable
  or disable the preview.
- from contributor point of view, you cannot edit the content in the HTML page, sometime
worth the notion of page doesn't exist and the preview mechanism is not easy.

Even if Jahia can be used as an headless CMS, it is in the CMS market for a wild and
has a long experience in content management. Thus, it is especially recognized to provide a
good contributor experience.
Based in its experience, Jahia can offer the best of two worlds, offer to the contributor
the same experience to contribute and preview an headless website as it was for a traditional
website rendered directly by the CMS.

|Traditional Edit Experience|Headless Edit Experience|
|---|---|
|![traditionalExperience]|![headlessExperience]|

To obtain this result we have properly integrated next-js flow in Jahia.

## Why we decide this initiative

The aim of this Initiative is to help partner and developer to think `Jahia` for a full headless web project. In other word, as a developer which modules do I have to 
install and configure to set up a platform where my Web Contributors can create, contribute and personalize a website in Jahia and
delegate the live rendering to a next-js react application hosted by Vercel.

The website you will deploy is inspired by the Industrial template.

![003]

This template is not designed for React as it is a wordpress Html template. But, they are still a lot of company or digital agency
using templates like this one, and it could be interesting to show how we can handle components like [owl carousel][owl]
which is a very popular carousel but not a pure React component.

> We already have a [Jahia module available for this HTML template][store-industrial].This module uses the traditional rendering
capabilities of Jahia to deliver the website. Thus, what we want to achieve is to offer 
the same capabilities to contribute, personalize and deliver a website regardless of the live renderer.

## Modules involved
Three modules are required to contribute a web project in Jahia and to render it in Vercel.
Two modules are deployed in Jahia and one module is deployed in Vercel.

- [headless-templatesSet]: a Jahia Templates Set for headless projects.
This template set must be used with a proxy to enable headless features.
- [nextjs-proxy]: a jahia modules used to manage Nextjs website in Jahia. This module provides :
  - an HTTP proxy to redirect edit and preview rendering
  - a mixin to configure paths and token to interact with your Vercel next-js app
  - a mixin and its contentList Initializer to list and select an headless template for your pages 
- [jahia-industrial]: this module contains the content definitions to create components related to the industrial HTML template.
- [nextjs-industrial]: this module contains the page templates and content views built in React used to render the website.

> We also provide an archive which contains a pre-built website with page samples.

## Set up the project
[Read this dedicated page][setup.md]

## Architecture
[Read this dedicated page][architecture.md]



[000]: doc/images/000_DevPageTemplate.png
[001]: doc/images/001_ContribPageTempalte.png
[003]: doc/images/003_website.png
[traditionalExperience]: doc/images/005_traditionalExperience.png
[headlessExperience]: doc/images/004_headlessExperience.png

[owl]: https://owlcarousel2.github.io/OwlCarousel2
[store-industrial]: https://store.jahia.com/contents/modules-repository/org/jahia/se/modules/industrial.html
[jahia-website]: https://www.jahia.com
[vercel-website]: https://vercel.com

[headless-templatesSet]: https://github.com/Jahia/headless-templatesSet
[nextjs-proxy]: https://github.com/Jahia/nextjs-proxy
[jahia-industrial]: https://github.com/Jahia/jahia-industrial
[nextjs-industrial]: https://github.com/Jahia/nextjs-industrial

[setup.md]: doc/setup.md
[architecture.md]: doc/architecture.md

