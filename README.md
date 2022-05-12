# The Jahia Nextjs initiative : *Jahia the best DXP to manage headless web project*

The aim of the Jahia Nextjs initiative is to explore and explain
the Jahia capabilities to easily create and manage headless web project.
Solutions we use are :
- [Jahia][jahia-website] : a Cloud / On-premise *DXP* solution to create and contribute
- [Vercel][vercel-website] a next-js Cloud platform provider to render the web project

### Table of Contents :

- [Initiative overview](#initiative-overview)
- [Why we decide this initiative](#why-we-decide-this-initiative)
- [Modules involved](#modules-involved)

## Initiative overview

To be the best, Jahia must take care of both side of a project, the development and the content contribution.
One of the key factor success of a web project is the facility for each side to execute their task. 

|Developers|Web Contributors|
|---|---|
|A web project is easy to create and maintain when they have an SDK in their preferred languages, and they can create, update, test and deploy component quickly.| A web project is easy to contribute and maintain when they understand the web site page structure and when they can create a content by clicking a button or just copy past a WYSIWYG content in the web page.|
| ![000] | ![001] |

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

- [headless-templatesSet] : a Jahia Templates Set for headless projects. This module provides :
  - an HTTP proxy to redirect edit and preview rendering
  - a mixin to configure paths and token to interact with your Vercel next-js app
  - a mixin and its contentList Initializer to list and select an headless template for your pages 
  - a graphQL extension provider to create content areas (temporarily in this module)
- [jahia-industrial] : this module contains the content definitions to create components related to the industrial HTML template.
- [nextjs-industrial] : this module contains the page templates and content views built in React used to render the website.

We also provide an archive which contains a pre-built website with page samples.

## Set up the project
[Read this dedicated page][setup.md]



[000]: doc/images/000_DevPageTemplate.png
[001]: doc/images/001_ContribPageTempalte.png
[003]: doc/images/003_website.png

[owl]: https://owlcarousel2.github.io/OwlCarousel2
[store-industrial]: https://store.jahia.com/contents/modules-repository/org/jahia/se/modules/industrial.html
[jahia-website]: https://www.jahia.com
[vercel-website]: https://vercel.com

[headless-templatesSet]: https://github.com/Jahia/headless-templatesSet
[jahia-industrial]: https://github.com/Jahia/jahia-industrial
[nextjs-industrial]: https://github.com/Jahia/nextjs-industrial

[setup.md]: doc/setup.md

