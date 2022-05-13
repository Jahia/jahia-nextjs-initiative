# The Jahia Nextjs initiative : *Setup*

To know more about the Jahia Nextjs initiative [read this dedicated page][initiative.md].

## What we want to achieve
The aim of this document is to present the path to install and configure modules used
to create, contribute and personalize a website in [Jahia][jahia-website] and delegate
the live rendering to a next-js react application hosted by [Vercel][vercel-website].

The website you will deploy is inspired by the Industrial template.

![003]

## Modules involved
Three modules are required to contribute a website in Jahia and to render it in Vercel.
Two modules are deployed in Jahia and one module is deployed in Vercel.

- [headless-templatesSet] : a Jahia Templates Set for headless projects.
- [jahia-industrial] : this module contains the content definitions to create
components related to the industrial HTML template.
- [nextjs-industrial] : this module contains the page templates and content views
built in React used to render the website.

> Note: we also provide an archive which contains a pre-built website with page samples.

## OnePremise Setup

## Cloud Setup
The instructions below are written for an architecture using vercel and
an instance of Jahia accessible by vercel. Let's assume you have a jahia cloud
environnement up and runing ([ask for a demo cloud environnement][ask-demo-cloud])

### Jahia  

#### Personal API Token
To render a web project the application deployed on Vercel executes some graphQL calls
on the backend side. To authenticate these calls you need to create a
[Personal API Token][jahia-token] in Jahia.

1. Log into your Jahia server with the root user and from your dashboard
click **My API tokens** then **+ CREATE TOKEN**
   <img src="/doc/images/setup/200_token.png" width="700px"/>

2. Fill the form, for the name use `headless` and for the Scope click the `graphql`
entry then click **CREATE**
   <img src="/doc/images/setup/201_token.png" width="700px"/>

3. Copy paste your token, you will use it later as an environment variable forthe vercel app.
   <img src="/doc/images/setup/202_token.png" width="450px"/>


The next step is to deploy modules and web project into jahia.

#### Jahia provisioning
During this step you will deploy all the jahia modules needed to manage the
industrial headless web project. For this purpose you use the
[Jahia provisioning API][jahia-provisioning] available from Jahia 8.0.3.0.

To install modules and more with this API, you need to send a configuration file
(json or yaml) through a REST call to the provisioning API endpoint.
For this tutorial you use a yaml file and you send it with curl.

> Tip: you can also use Postman or other tools to send your REST call.

##### Yaml configuration
To install all the required components into jahia you use the following configuration :
```yml
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
```
The configuration above do three main things:
1. configure the access to the nexus repositories where some archives are stored.
2. list the modules to install.
3. import a prepared web project named headless-industrial.

This provisioning script is available [here][headless-industrial.yaml].
We will then use its URL in our curl request below.

##### Call the provisioning API
Now you have your configuration file, you will send it with CURL to your server.
In addition to the yaml configuration file you have to configure the url and headers
before the call.

The following url and user values are given as an example,
please **update** with your **host** and your **root user password**.

```shell
curl --request POST \
--url https://headless-jahiapm.internal.cloud.jahia.com/modules/api/provisioning \
-u root:root123 \
--header 'Content-Type: application/yaml' \
--data '- include: https://raw.githubusercontent.com/Jahia/jahia-nextjs-initiative/main/provisioning/headless-industrial.yaml'
```

Adjust the parameters and execute the curl request. This takes around 30s and should
not return any errors.

##### Provisioning check
If everything is done properly you should have a new web project available in your
jahia server. From your Jahia dashboard you should see the **Headless Industrial** project.

![203]

If you click on the project you should see the Site tree in page composer.

![204]

As you can see in the image above the website needs a post install configuration.
But before doing it, you will deploy the frontend part on [vercel.com][vercel-website]

### Vercel
Vercel is a next-js Cloud platform provider.

#### Vercel provisioning
You will deploy and configure the [next-industrial web application][nextjs-industrial]
on [vercel.com][vercel-website].

##### Prerequisite
You must have a Github account or one of the others below and use this account
to signup to vercel.

<img src="/doc/images/setup/100_vercel.png" width="375px"/>

##### Next-industrial webapp deployment
1. After logged into vercel, click **+ New Project** from your vercel dashboard
![101]
2. Then click **Import Third-Party Git Repository** and copy paste the url of the
next-industrial project available on the Jahia github repository : 
`https://github.com/Jahia/nextjs-industrial`
<img src="/doc/images/setup/102_project_import.png" width="300px"/>
<img src="/doc/images/setup/103_project_import.png" width="300px"/>
3. Select the git account in which you want to clone the project and the project name,
then click **create**. Vercel will clone the repository to your account and start
to build it. It will take about 1mn for the repository to be created and the **build
to fail. It is normal!** To have a successful build, you need to configure
environment variables.
   <img src="/doc/images/setup/104_project_import.png" width="300px"/>
   <img src="/doc/images/setup/105_project_import_failed.png" width="300px"/>


<u>Note</u> : the previous action has created a clone of the next-industrial project in your github. This clone will be used in another tutorial to update the rendering of the website.

![1041]

##### Next-industrial webapp configuration
To work properly, the application needs to have some environment variables configured.

1. From your vercel dashboard click the **next-industrial** card, then click **Settings**
then **Environment Variables**
![106]
![107]
2. From the Environment Variable screen you have to register four variables,
two variables are **public** (NEXT_PUBLIC), that means they are **exposed**
in the browser and the two others are used only by the **back** and are 
**not exposed** in the browser.
   - **NEXT_PUBLIC_JAHIA_SITE** : used to define the name of the web project you create
   in Jahia. For this tutorial the name is:
   `headless-industrial`
   
   - **NEXT_PUBLIC_JAHIA_BASE_URL** : used by the graphQL client (apollo) to reques
   the content to the Jahia server. If you already have a Jahia server up and running
   write its URL, e.g. : *https://jahia-server-instance.com*

   - **NEXT_PREVIEW_SECRET** : The value is an arbitrary string used by jahia to
   authenticate API call requests to vercel. For example, this token is used to set up
   the preview. For this tutorial the value is:`57e22073-2485-43fe-b9a1-9205d5310561`

   - **JAHIA_API_TOKEN** : This is the value of the [personal API token](#personal-api-token)
   you have previously  configured in your Jahia instance. This token is used by vercel
   to authenticate the server side graphQL request to Jahia. Add the token value
   you copy pasted previously.

   ![108]
![109]

##### Rebuild next-js app
Now your environment variables are configured you can redo a build.

>tip : be sure your Jahia is up and running.

Go to the **Deployments** tabs and click the **three dots menu** in the right and
click **Redeploy**

![110]

Now the build must be successful and pages should have been created. If you look at
the build log from **Deployment Status** > **Building**  you should see Page at the
end of the log trace:

![111]

>Note: there are some warnings in the log about React components and scss; it is normal.

And in the **Deployments** tabs you should see

![112]
![113]

To check your deployment you can try to load the home page :

**<your-vercel-domain>/sites/headless-industrial/home**

In this tutorial the url looks like this: 
*https://nextjs-industrial-mu.vercel.app/sites/headless-industrial/home*
![114]
>Note : Vercel configures three domains. In the context of this tutorial and in accordance to the configuration I will use nextjs-industrial-hduchesn.vercel.app
![115]

### Jahia Post-configuration
#### Configure access to vercel site in Jahia
To finalize the installation of your web project, you have to configure Jahia in 
the same way we set environment variables in vercel.

1. From Jahia backend go to **Page Composer** and right click on the site node :
**Headless Industrial** and click **Edit**
![205]
2. The edit form appears, scroll down to the end of the form and turn on the
switch button **HEADLESS SITE**
![206]
3. You have to fill four parameters :
   1. The headless frontend server url, this is the stable domain of your vercel app.
   In our case : *https://nextjs-industrial-hduchesn.vercel.app*
   2. The path for the preview API, use: `/api/preview`
   3. The preview token (it is the same value as the one configure in the vercel
   env variable), use:`57e22073-2485-43fe-b9a1-9205d5310561`
   4. The path to list the Page template available in the nextjs app, use:
   `/api/jahia/templates`
   ![207]
4. Click **SAVE** and return to Page Composer clicking the button 
<img src="/doc/images/setup/xxx_icon.png" width="20px"/>

Now you should see the website with its contribution area in Page Composer.

![208]

In the **Sample sites** section of the site plan you can see sample pages and
you can copy paste elements from **Industrial** or **About** sample pages into
the Industrial home page.
<img src="/doc/images/setup/209_jahia.png" width="300px"/>


### Handle CORS issue
You have one last configuration to do in Jahia to configure CORS. This configuration
allows the vercel webapp to execute graphQL calls to jahia from the browser
(frontend side). The current website is working properly without this configuration
because the website does not have to contact Jahia from the front. But if you include
a dynamic component like a paging element, and you want to request content for
the next page, in that case a graphQL will be performed.

So to prevent CORS issues, you have to allow the vercel domain in the api authorization
file deployed by the [headless-templatesSet] module.

To update the configuration file, you will again use the provisioning API.
Don’t forget to use your own vercel domain. As a reminder the one display
below is relative to the specific instance deployed as an example in the tutorial.

#### Yaml configuration file
```yaml
- editConfiguration: "org.jahia.bundles.api.authorization"
  configIdentifier: "headless"
  properties:
  headless.auto_apply[1].origin: "https://nextjs-industrial-hduchesn.vercel.app"
```
#### Call the provisioning API
```shell
curl --request POST \
--url https://headless-jahiapm.internal.cloud.jahia.com/modules/api/provisioning \
-u root:root123 \
--header 'Content-Type: application/yaml' \
--data '- editConfiguration: "org.jahia.bundles.api.authorization"
configIdentifier: "headless"
properties:
headless.auto_apply[1].origin: "https://nextjs-industrial-hduchesn.vercel.app"'
```
Et voilà !

>Tip: if you have any issue with Page composer and you want to edit the system
site node **Headless Industrial** you can do it from Repository Explorer.

[003]: ./images/003_website.png

[200]: ./images/setup/200_token.png
[201]: ./images/setup/201_token.png
[202]: ./images/setup/202_token.png
[203]: ./images/setup/203_provision.png
[204]: ./images/setup/204_provision.png
[205]: ./images/setup/205_jahia.png
[206]: ./images/setup/206_jahia.png
[207]: ./images/setup/207_jahia.png
[208]: ./images/setup/208_jahia.png
[209]: ./images/setup/209_jahia.png

[100]: ./images/setup/100_vercel.png
[101]: ./images/setup/101_vercel_createProject.png
[106]: ./images/setup/106_project_post_config.png
[107]: ./images/setup/107_post_config.png
[108]: ./images/setup/108_post_config.png
[109]: ./images/setup/109_post_config.png
[110]: ./images/setup/110_vercel.png
[111]: ./images/setup/111_vercel.png
[112]: ./images/setup/112_vercel.png
[113]: ./images/setup/113_vercel.png
[114]: ./images/setup/114_vercel.png
[115]: ./images/setup/115_vercel.png

[1041]: ./images/setup/1041_project_clone_github.png

[xxx]: ./images/setup/xxx_icon.png

[headless-templatesSet]: https://github.com/Jahia/headless-templatesSet
[jahia-industrial]: https://github.com/Jahia/jahia-industrial
[nextjs-industrial]: https://github.com/Jahia/nextjs-industrial
[jahia-website]: https://www.jahia.com
[vercel-website]: https://vercel.com
[ask-demo-cloud]: https://www.jahia.com
[jahia-token]: https://academy.jahia.com/home/documentation/developer/dx/8/working-with-our-apis/using-personal-api-tokens.html
[jahia-provisioning]:https://academy.jahia.com/documentation/system-administrator/dev-ops/provisioning/about-provisioning

[initiative.md]: ../README.md
[headless-industrial.yaml]: ../provisioning/headless-industrial.yaml
