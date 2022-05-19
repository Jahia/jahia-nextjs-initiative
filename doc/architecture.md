# The Jahia Nextjs initiative : *Architecture*

To know more about the Jahia Nextjs initiative [read this dedicated page][initiative.md].

- [Building process](#building-process)
- [Edit or Preview content](#edit-or-preview-content)
    - [First load](#first-load)
    - [Standard load](#standard-load)
- [Live rendering](#live-rendering)

## Building process
### Data fetching
At the time of writing this, the mechanisms Next offers for data fetching on the server
are at the page-level or above. This means that we cannot write a component
that gets its own data and have it rendered on the server in Next.js.
Instead, each component must get its data from the page it exists on.

This is not to say that we can’t fetch data at the component-level.
If you want a component to fetch its own data, you can still do that in the browser.
Unfortunately, client side rendering (CSR) in the browser is less ideal because
you lose the UX and SEO benefits of prerendering your app on the server (SSG)

The majority of headless CMS in the market using next-js for the frontend are subject
to this limitation.

The Jahia approach differs from others competitor by the capacity to do
component level data fetching on the server side.

The schema below is an overview of the build flow of a next-js react app using Jahia as
backend.
As you can see in the schema the magic is done in our SDK at the _app.jsx level. The SDK
uses the [apollo capabilities][apollo-ssr] to execute all the graphQL queries needed to fully render
a page and its component. Thus, the results of all queries are stored in the apolloState 
and use later at the component level to render it.

![build]
## Edit or Preview content
### Overview
With headless CMS editing and previewing HTML page is not trivial for developers and
contributors :
- from developer point of view you have to add a specific button to your app, to enable
or disable the preview.
- from contributor point of view, you cannot edit the content in the HTML page, sometime worth
the notion of page doesn't exist and the preview mechanism is not easy.

Even if Jahia can be used as an headless CMS it is in the CMS market for a wild and
as a long experience in content management, it is especially recognized to provide a
good contributor experience.
From its experience Jahia can offer the best of tow worlds, offert to the contributor 
the same experience to contribute and preview an headless website as it was in a traditional
website rendered directly by the CMS.

|Traditional Edit Experience|Headless Edit Experience|
|---|---|
|![traditionalExperience]|![headlessExperience]|

To obtain this result we have properly integrated next-js flow in Jahia.

### First load
![editpreviewfirst]
### Standard load
![editpreviewsecond]
## Live rendering
![live]

[build]: ./images/architecture/build.png
[editpreviewfirst]: ./images/architecture/editpreviewfirst.png
[editpreviewsecond]: ./images/architecture/editpreviewsecond.png
[live]: ./images/architecture/live_flow.png
[traditionalExperience]: ./images/architecture/traditionalExperience.png
[headlessExperience]: ./images/architecture/headlessExperience.png

[apollo-ssr]:https://www.apollographql.com/docs/react/api/react/ssr/#rendertostringwithdata
