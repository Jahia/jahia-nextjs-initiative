# The Jahia Nextjs initiative : *Architecture*

To know more about the Jahia Nextjs initiative [read this dedicated page][initiative.md].

- [Building process](#building-process)
- [Edit or Preview content](#edit-or-preview-content)
  - [Overview](#overview)
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
Unfortunately, client side rendering ([CSR][site-generation-mode]) in the browser is less ideal because
you lose the UX and SEO benefits of prerendering your app on the server
([SSG][site-generation-mode])

The majority of headless CMS in the market using next-js for the frontend are subject
to this limitation.

The Jahia approach differs from others competitor by the capacity to do
component level data fetching on the server side.

The schema below is an overview of the build flow of a next-js react app using Jahia as
backend.
As you can see in the schema the magic is done in our SDK at the _app.jsx level. The SDK
uses the [apollo capabilities][apollo-ssr] to execute all the graphQL queries needed to
fully render a page and its component. Thus, the results of all queries are stored in
the apolloState and use later at the component level to render it.

![build]
## Edit or Preview content
With headless CMS, editing and previewing HTML page is not trivial for developers and
contributors.

Based in its experience, Jahia can offer the best of two worlds, offer to the contributor
the same experience to contribute and preview an headless website as it was for a traditional
website rendered directly by the CMS.

To obtain this result we have properly integrated next-js flow in Jahia.
### First load
To properly edit or preview content with next-js you have to
[enable the preview mode][next-preview] first.
That mean sending a request to get back preview cookies used after for each call to the
next-js app.

![editpreviewfirst]
### Standard load
When you have enabled the preview mode, meaning the next-js preview cookies are available,
you can just call the webapp to see your changes. Indeed, in preview mode,
the next-js app build pages in [SSR][site-generation-mode] mode for each request showing 
immediately the update.
>Tip : with Jahia, contributors see the HMLT page they are editing and they can easily
> copy/past/update content directly from the generated HTML page.

![editpreviewsecond]
## Live rendering
The only interest using next-js as a frontend is to use the SSG mode to have a fast load
of your HTML pages. In SSG, HTML pages are served by Vercel CDN, content data are available
from the pages and the Hydration doesn't need to do any graphQL call. But of course,
if you have dynamic component like a search or paging, the app will then execute client side
call.

![live]

[build]: ./images/architecture/build.png
[editpreviewfirst]: ./images/architecture/editpreviewfirst.png
[editpreviewsecond]: ./images/architecture/editpreviewsecond.png
[live]: ./images/architecture/live_flow.png

[next-preview]: https://nextjs.org/docs/advanced-features/preview-mode#step-1-create-and-access-a-preview-api-route
[site-generation-mode]: https://nextjs.org/docs/basic-features/data-fetching/overview
[apollo-ssr]:https://www.apollographql.com/docs/react/api/react/ssr/#rendertostringwithdata
[initiative.md]: ../README.md
