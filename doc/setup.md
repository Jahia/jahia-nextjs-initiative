# The Jahia Nextjs initiative : *Setup*

Overview
What we want to achieve
The aim of this document is to present the path to install and configure modules used to create, contribute and personalize a website in Jahia and delegate the live rendering to a next-js react application hosted by Vercel (next-js cloud platform provider).

The website you will deploy is based on the Industrial template.


This template is not designed for React as it is a wordpress Html template. But, most of the customers are still using templates like this one and it could be interesting to show how we can handle components like owl carousel which is a very popular carousel but not a pure React component.

We already have a jahia module available for this HTML template.This module uses the traditional rendering capabilities of Jahia to deliver the website.

Thus, what we want to achieve is to offer the same capabilities to contribute, personalize and deliver a website regardless of the live renderer.
Modules involved
Three modules are required to contribute a website in Jahia and to render it in Vercel. Two modules are deployed in Jahia and one module is deployed in Vercel.

headless-templatesSet : a Jahia Templates Set for headless projects. This module provides :
an HTTP proxy to redirect edit and preview rendering
a mixin to configure paths and token to interact with your Vercel next-js app
a mixin and its contentList Initializer to list and select an headless template for your pages
a graphQL extension provider to create content areas (temporarily in this module)
jahia-industrial : this module contains the content definitions to create components related to the industrial HTML template.
nextjs-industrial : this module contains the page templates and content views built in React used to render the website.

Note : we also provide an archive which contains a pre-built website with page samples.

Provisioning
Cloud deployment
The instructions below are written for an architecture using vercel and an instance of Jahia accessible by vercel.

Jahia Personal API Token
To render a web project the application deployed on Vercel executes some graphQL calls on the backend side. To authenticate these calls you need to create a Personal API Token in Jahia.
Log into your Jahia server with the root user and from your dashboard click My API tokens then + CREATE TOKEN



Fill the form, for the name use headless and for the Scope click the graphql entry then click CREATE




Copy paste your token, you will use it later as an environment variable for the vercel app.




The next step is to deploy modules and web project into jahia.
Jahia provisioning
During this  step you will deploy all the jahia modules needed to manage the industrial headless web project. For this purpose you use the Jahia provisioning API available from Jahia 8.0.3.0.

To install modules and more with this API, you need to send a configuration file (json or yaml) through a REST call to the provisioning API endpoint. For this tutorial you use a yaml file and you send it with curl.

Note : you can also use Postman or other tools to send your REST call.
Yaml configuration
To install all the required components into jahia you use the following configuration :

- addMavenRepository: "https://store.jahia.com/nexus/content/repositories/jahia-public-app-store@id=JahiaStore"
- addMavenRepository: "https://devtools.jahia.com/nexus/content/groups/internal@id=jahia-internal@snapshots"

# New next.js and industrial modules
- installBundle:
    - "mvn:org.jahia.modules/popperjs"
    - "mvn:org.jahia.modules/bootstrap4-core"
    - "mvn:org.jahia.modules/bootstrap4-components"
    - "mvn:org.jahia.se.modules/codemirror-editor/1.1.3"
    - "mvn:org.jahia.se.modules/jahia-industrial"
    - "mvn:org.jahia.se.modules/industrial-media-component/1.0.0"
    - "mvn:org.jahia.se.modules/headless-templatesSet"
      autoStart: true
      uninstallPreviousVersion: true
- importSite: jar:https://github.com/Jahia/jahia-nextjs-initiative/raw/main/docker-jahia/headless-industrial.zip!/headless-industrial.zip
- import: jar:https://github.com/Jahia/jahia-nextjs-initiative/raw/main/docker-jahia/headless-industrial.zip!/users.zip

The configuration above do three main things:
configure the access to the nexus repositories where some archives are stored.
list the modules to install.
import a prepared web project named headless-industrial.

For convenience, we uploaded this provisioning script to this github repository. We will then use its URL in our curl request below.
Call the provisioning API
Now you have your configuration file, you will send it with CURL to your server. In addition to the yaml configuration file you have to configure the url and headers before the call.

The following url and user values are given as an example, please update with your host and your root user password.

curl --request POST \
--url https://headless-jahiapm.internal.cloud.jahia.com/modules/api/provisioning \
-u root:root123 \
--header 'Content-Type: application/yaml' \
--data '- include: https://raw.githubusercontent.com/Jahia/jahia-nextjs-initiative/main/provisioning/headless-industrial.yaml'


Adjust the parameter and execute the curl request. This takes around 30s and should not return any errors.
Provisioning check
If everything is done properly you should have a new web project available in your jahia server.
From your Jahia dashboard you should see the Headless Industrial project.



If you click on the project you should see the Site tree in page composer.



As you can see in the image above the website needs a post install configuration. But before doing it, you will deploy the frontend part on vercel.com

Vercel provisioning
You will deploy and configure the next-industrial web application on vercel.com.
Prerequisite
You must have a Github account or one of the others below and use this account to signup to vercel.


Next-industrial webapp deployment
After logged into vercel, click New Project from your vercel dashboard





Then click Import Third-Party Git Repository and copy paste the url of the next-industrial project available on the Jahia github repository :

https://github.com/Jahia/nextjs-industrial




Select the git account in which you want to clone the project and the project name, then click create.
Vercel will clone the repository to your account and start to build it. It will take about 1mn for the repository to be created and the build to fail. It is normal!
To have a successful build, you need to configure environment variables.


Note : the previous action has created a clone of the next-industrial project in your github. This clone will be used in another tutorial to update the rendering of the website.





Next-industrial webapp configuration
To work properly, the application needs to have some environment variables configured.

From your vercel dashboard click the next-industrial card, then click Settings then Environment Variables






From the Environment Variable screen you have to register four variables, two variables are public (NEXT_PUBLIC), that means they are exposed in the browser and the two others are used only by the back and are not exposed in the browser.


NEXT_PUBLIC_JAHIA_SITE : used to define the name of the web project you create in Jahia. For this tutorial the name is:

            headless-industrial



NEXT_PUBLIC_JAHIA_BASE_URL : used by the graphQL client (apollo) to request the content to the Jahia server. If you already have a Jahia server up and running write its URL, e.g. : https://jahia-server-instance.com

NEXT_PREVIEW_SECRET : The value is an arbitrary string used by jahia to authenticate API call requests to vercel. For example, this token is used to set up the preview. For this tutorial the value is:

        57e22073-2485-43fe-b9a1-9205d5310561



JAHIA_API_TOKEN : This is the value of the personal API token you have previously  configured in your Jahia instance. This token is used by vercel to authenticate the server side graphQL request to Jahia. Add the token value you copy pasted previously.





Rebuild next-js app
Now your environment variables are configured you can redo a build.

Note : be sure your Jahia is up and running.

Go to the Deployments tabs and click the three dots menu in the right and click Redeploy




Now the build must be successful and pages should have been created. If you look at the build log from Deployment Status > Building  you should see Page at the end of the log trace:





Note: there are some warnings in the log about React components and scss; it is normal.

And in the Deployments tabs you should see








To check your deployment you can try to load the home page :

<your-vercel-domain>/sites/headless-industrial/home

In this tutorial the url looks like this: https://nextjs-industrial-mu.vercel.app/sites/headless-industrial/home


Note : Vercel configures three domains. In the context of this tutorial and in accordance to the configuration I will use nextjs-industrial-hduchesn.vercel.app



Jahia Post-configuration
Configure access to vercel site in Jahia
To finalize the installation of your web project, you have to configure Jahia in the same way we set environment variables in vercel.

From Jahia backend go to Page Composer and right click on the site node : Headless Industrial and click Edit




The edit form appears, scroll down to the end of the form and turn on the switch button HEADLESS SITE




You have to fill four parameters :
The headless frontend server url, this is the stable domain of your vercel app. In our case : https://nextjs-industrial-hduchesn.vercel.app


The path for the preview API, use:

            /api/preview


The preview token (it is the same value as the one configure in the vercel env variable), use:

        57e22073-2485-43fe-b9a1-9205d5310561


The path to list the Page template available in the nextjs app, use:

            /api/jahia/templates




Click SAVE and return to Page Composer clicking the button

Now you should see the website with its contribution area in Page Composer.



In the Sample sites section of the site plan you can see sample pages and you can copy paste elements from Industrial or About sample pages into the Industrial home page.


Handle CORS issue
You have one last configuration to do in Jahia to configure CORS. This configuration allows the vercel webapp to execute graphQL calls to  jahia from the browser (frontend side). The current website is working properly without this configuration because the website does not have to contact Jahia from the front. But if you include a dynamic component like a paging element and you want to request content for the next page, in that case a graphQL will be performed.

So to prevent CORS issues, you have to allow the vercel domain in the api authorization file deployed by the headless-templateSet module.

To update the configuration file, you will again use the provisioning API. Don’t forget to use your own vercel domain. As a reminder the one display below is relative to the specific instance deployed as an example in the tutorial.

Yaml configuration file
- editConfiguration: "org.jahia.bundles.api.authorization"
  configIdentifier: "headless"
  properties:
  headless.auto_apply[1].origin: "https://nextjs-industrial-hduchesn.vercel.app"

Call the provisioning API
curl --request POST \
--url https://headless-jahiapm.internal.cloud.jahia.com/modules/api/provisioning \
-u root:root123 \
--header 'Content-Type: application/yaml' \
--data '- editConfiguration: "org.jahia.bundles.api.authorization"
configIdentifier: "headless"
properties:
headless.auto_apply[1].origin: "https://nextjs-industrial-hduchesn.vercel.app"'

Et voilà !


Note: if you have any issue with Page composer and you want to edit the system site node Headless Industrial you can do it from Repository Explorer.
