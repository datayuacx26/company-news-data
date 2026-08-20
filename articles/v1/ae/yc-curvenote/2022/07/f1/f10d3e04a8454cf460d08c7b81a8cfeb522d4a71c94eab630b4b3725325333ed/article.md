---
schema_version: "1.0.0"
document_id: "f10d3e04a8454cf460d08c7b81a8cfeb522d4a71c94eab630b4b3725325333ed"
company_key: "yc-curvenote"
company: "Curvenote"
source_id: "yc-curvenote-news-import-2221551dc8c3"
canonical_url: "https://curvenote.com/blog/curvenote-on-the-command-line"
published_at: "2022-07-21T00:00:00+00:00"
first_seen_at: "2026-07-23T06:57:34.135292+00:00"
fetched_at: "2026-07-28T21:40:00.658555+00:00"
content_hash: "sha256:d595b9acf3da75003a1a67eb1c7d46ae6d165aa60cf9f4b537ea4c76ce962a33"
---

# Curvenote on the Command Line

# Curvenote on the Command Line


Steve Purves, Rowan Cockett · July 21, 2022


Over the past few months, we’ve been busy building an[open source Command Line Interface](https://github.com/curvenote/curvenote) (CLI) that sits on top of our content translation schemas and talks to the Curvenote API. We’ve put a lot of time and care into the design of the Curvenote CLI to make it easy to use and so you can get results quickly. In this post, we will dive into the main capabilities of the Curvenote CLI.


Figure 1: The Curvenote CLI after you run` curvenote init`


## Installing the Curvenote CLI


¶


The new Curvenote CLI runs in[NodeJS](https://curvenote.com/docs/cli/installing-prerequisites) . You can[install the Curvenote CLI](https://curvenote.com/docs/cli/installing) using node v14 or greater:


```text
npm install -g curvenote
```


You can[learn more about installing Node or NPM](https://curvenote.com/docs/cli/installing-prerequisites) if those tools aren’t familiar.


The CLI can work completely locally on your content and files. It can also connect and synchronize with the Curvenote API (learn[how to get an API key](https://curvenote.com/docs/cli/authorization) ), opening up collaborations between folks working online in the[Curvenote editor](https://curvenote.com/for/writing) , in the[Curvenote Jupyter extension](https://curvenote.com/for/jupyter) , or locally through the command line.


## Creating Websites


¶


The CLI provides a simple development experience for writing content and publishing websites designed for open science publications — whether that’s a[preprint or paper](https://www.stevejpurves.com/geoscience) ,[materials for a whole research project](https://www.stevejpurves.com/la-palma-earthquakes) or a[full Ph.D. thesis](https://phd.row1.ca/) . Other examples include:


-


[Courses](https://geosci-inversion.curve.space/) &[books](https://climasoma.curve.space/)


-


[Seminar](https://seminars.simpeg.xyz/) &[conference](https://transform.softwareunderground.org/) websites


-


[Blogs](https://curvenote.com/blog) &[technical websites](https://www.stevejpurves.com/blog)


-


[Papers](https://www.stevejpurves.com/la-palma-earthquakes) &[reports](https://www.stevejpurves.com/computational-finance)


-


[Documentation](http://curvenote.com/docs)


-


[Sharing Jupyter Notebooks](https://jarmitage.curve.space/)


Figure 2: A selection of websites that can be built using the Curvenote CLI.


### Getting Started


¶


Key commands for building websites locally are:


-


` curvenote init` - initialize your folder with a` curvenote.yml` file containing` project` and` site` configuration


-


` curvenote start` - build your website, start a local web server, and open a preview in your web browser (this preview will update in real time as you update content!)


-


` curvenote deploy` - deploy your website to Curvenote’s free hosting services


Provided you already have content in Markdown files or Juptyer Notebooks, you can get a website started, previewed, and deployed to the web in just a few minutes.


Here is a demo where Rowan got a full live demo done in 2 minutes and 54 seconds. 🚀


Tutorials


Check out our tutorials on different routes to get started:


-


[Start from a local folder](https://curvenote.com/docs/web/tutorial-deploy-local)


-


[Start from a Curvenote project](https://curvenote.com/docs/web/tutorial-deploy-project)


## Cloning Curvenote Projects


¶


The CLI works with both local content and content online in Curvenote projects. The latter is made possible by Curvenote’s API and underlying version control. You can pull in content from projects that you collaborate on with others, save new versions of your work, and update exiting projects with both content and website configuration information.


At the moment there are two commands that allow you to sync content. Those are` clone` and` pull` , which fetch new content to a local folder and fetch updates for an existing local folder respectively.


First, let’s take a look at` clone`


```text
curvenote clone <https://curvenote.com/@username/my-project>
```


For example, try` curvenote clone https://curvenote.com/@templates/web`


This command can behave in one of two ways:


1.


Run this in an empty folder and this will fetch the project’s existing` curvenote.yml` ,` site` configuration and content enabling you to work on this locally and deploy manually.


2.


Run this in an initialized local folder (i.e. one containing a` curvenote.yml` with your` site` configuration) and this will add the project as *an additional section in your existing website* , fetching the content and updating your website’s navigation controls in the process.


The first allows update and manual deployment of any existing website either by another user or by an automated system (e.g. through the[Curvenote GitHub Action](https://curvenote.com/docs/web/github-action) ), where the entire site configuration and latest versions of content is retrieved in full with a single command.


The second case enables powerful content aggregation workflows when publishing, allowing someone with the CLI to build documents or websites based on content from multiple collaborating users or groups, even when permission to access that content may be different on each project (of course, the CLI user must have access to all). An example of such a publication is the[SWUNG Transform Conference Website](https://transform.softwareunderground.org/) which was built from the Curvenote projects and GitHub repositories of many different groups of authors.


Next, let’s take a look at` pull` , which can only be run from within an initialized folder:


```text
curvenote pull [optional/content/path]
```


This command allows you to pull the latest updates for content in all projects, or a specific project or document if the optional path is provided.


### Roadmap


¶


We’re extending the CLI to expose Curvenote’s full version control capabilities (i.e.` merge` ,` diff` ,` push` ). This will enable:


-


a round trip of files and content with conflict resolution, allowing authors working locally to work with others concurrently no matter if they are working locally themselves or in the online editor


-


the ability to add and push new files including images and other supported files to Curvenote projects, enabling workflows where visualizations and figures are generated outside of Jupyter notebooks.


Stay tuned for[updates on Bluesky](https://bsky.app/profile/curvenote.com) !


## Exporting Content to Word, PDF, LaTeX, MyST and more


¶


The last CLI service we’ll talk about is` curvenote export` . This exposes functionality from Curvenote’s open source[schema](https://github.com/curvenote/schema) ,[nbtx](https://github.com/curvenote/nbtx) , and[prosemirror](https://prosemirror.net/) extensions which constitute a powerful document translation engine. It is used to build Markdown (MyST), HTML, DOCX, ipynb, jupyter-books, LaTeX and PDF files locally from a Curvenote article.


See the list of commands below:


Figure 3: Export to PDF, LaTeX, Markdown, Microsoft Word, JupyterBook or Sphinx with Curvenote.


Note


The commands in the export service will likely change over the coming months as we improve the functionality for local export/rendering.


When dealing with LaTeX and PDF exports you can also use[templates](https://curvenote.com/templates) from Curvenote’s[Public Template Repository](https://github.com/curvenote/templates) and create documents ready for submission to a number of preprint services and scientific journals.


Figure 4: A few of the[PDF templates](https://curvenote.com/templates) that you can export to using the CLI, allowing you to write your content once, then export to any template.


For more details on how to use the` export` service and dependencies that you may need to run some commands[see our documentation](https://curvenote.com/docs/export) .


### Roadmap


¶


Right now export commands only work with Curvenote projects, where content is pulled directly from the API into a temporary folder and built. We are currently changing this to support building from a local folder as well as support multi-article export (e.g. a PhD thesis or large report).


## Get Started


¶


We’ve built out detailed documentation at[https://curvenote.com/docs](https://curvenote.com/docs) , covering everything that we talked about in this post.


To begin your scientific writing and publishing journey with Curvenote, just[Install the CLI](https://curvenote.com/docs/cli) and[start building scientific content for the web](https://curvenote.com/docs/web) , syncing and[exporting to different document formats](https://curvenote.com/docs/export) .


### See the Curvenote CLI in action


¶


Watch our recent webinar on[Creating an Open Research Website](https://curvenote.com/blog/creating-an-open-research-website)


where we go from a folder of Markdown files and Jupyter Notebooks to a published website in a few minutes, before exploring the customizations we can make to change navigation, add more content, add licenses, DOIs, binder links and more.


Using Curvenote with GitHub


Have your content on GitHub? quickly try building a site directly from your repository using[try.curvenote.com](https://try.curvenote.com/) .


## Related Articles


- [Architecture of MyST and Curvenote Websites Curvenote and MyST websites create structured data, which can be rendered by any number of "theme servers", which are in charge of turning that structured data into a reading experience. myst cli](https://curvenote.com/blog/architecture-of-a-myst-website)
- [Writing a scientific paper faster with MyST Markdown Webinar - Learn how to write your next paper, report or even your thesis in MyST Markdown to create PDFs and interactive web articles. webinar myst cli editor](https://curvenote.com/blog/writing-a-scientific-paper-faster-myst-markdown)
- [How to use LaTeX with MyST Markdown The MyST command-line tools can now parse and render LaTeX documents, we explore some of the process behind creating this feature. latex tutorial myst editor](https://curvenote.com/blog/how-to-use-latex-with-myst-markdown)
- [How to use Paperpile with Curvenote Create BibTex files for your Curvenote project using Paperpile. This guide shows two ways to easily connect Paperpile to Curvenote to make reference management easy. tutorial editor partnerships](https://curvenote.com/blog/how-to-use-paperpile-with-curvenote)
- [One Click Publishing for Open Research Websites A Curvenote webinar taking attendees through publishing and updating research websites directly from the Curvenote visual editor webinar publishing editor](https://curvenote.com/blog/one-click-publishing-for-open-research)
- [Working Locally With MyST Markdown Export your Curvenote articles to MyST Markdown to locally edit the content. MyST Markdown is a new specification of Markdown that allows you to write professional documents, books and websites. myst cli editor](https://curvenote.com/blog/working-locally-with-myst-markdown)
