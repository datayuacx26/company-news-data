---
schema_version: "1.0.0"
document_id: "2b8fd67a7f192d834529add4dc4cacb0e1342625469466afa6006bfcc1e77653"
company_key: "certara-inc-common-stock"
company: "Certara Inc."
source_id: "certara-inc-common-stock-rss-c7049147c8d2"
canonical_url: "https://www.certara.com/on-demand-webinar/ml-in-python-with-chemaxon/"
published_at: "2026-08-11T15:20:18+00:00"
first_seen_at: "2026-08-11T17:21:16.300419+00:00"
fetched_at: "2026-08-11T17:21:18.380675+00:00"
content_hash: "sha256:46c69216917e88e38297f606ac9cebaaa360574dbbcf69d2cd4710f86d52799f"
---

# Trusted Cheminformatics, Zero Integration Headaches: ML in Python with Chemaxon

ShareShareShare


### View transcript


Good morning, everybody. Welcome to today’s webinar introducing the new Chemaxon on Python API. Before we get started, a quick overview of the webinar logistics.


All attendees will be muted throughout the session to ensure a clear audio experience.


Enter your questions in the Q and A panel and we’ll address them at the end of the session. If time does not allow for answering them today, we’ll make sure to follow-up by email in a couple of days.


Finally, the recording will be shared with you after the session so you can rewatch it or share it with your colleagues.


I would like to now introduce today’s speaker Jan Christopherson. Jan is a senior solutions consultant at Certara. He’s been with Chemaxon Portfolio since twenty nineteen and has since supported our customers across the whole application portfolio.


His combined scientific and product expertise makes him an excellent guy for today’s walkthrough of the new Python API.


I’m pleased to hand things over to him.


Well, thank you so much for the kind introduction, and, of course, thank you to our audience for being here. I’m excited to walk you through the first true public release of our Python APIs, and in doing so, demonstrate where our tools can help to improve your machine learning workflows.


In case any introductions are needed, you should know that in late twenty twenty four, Chemaxon joined the Certara family. The Certara Discovery division covers the former Chemaxon portfolio, as well as D360, and new workflows we’re working on to take advantage of Certara AI’s life science specialized GPT capabilities.


These are changes that we’re all really excited about, with long term goals to increase the collaboration between product groups and so strengthen our capabilities throughout the entire drug discovery process from discovery through to the clinic.


For those of you who know that we’ve historically been a Java shop, you might be asking yourselves, why Python and why now? The answer is that meeting our users where they are has always been a core part of our DNA. And we’ve seen this in the past when we pivoted from being largely a toolkit provider to releasing user ready enterprise grade applications. And then again when we added SaaS distributions as the industries began embracing cloud.


Now that we have a preponderance of users needing to model in Python, we’re here to meet you once again. So what we’ve done is use a Graal VM based tool to comp ile our Java code into Python libraries to ensure that you get the results with the same accuracy that you’ve always relied on, as well as an easy path to access.


Certara Discovery has a broad footprint supporting hit to lead and lead optimization efforts, and this is easily recognized by mapping some of our more recognizable applications onto the DMTA cycle as we’ve done here.


Today’s session heavily focuses on the JChem toolkit for structure representation highlighted in the center here, as well as our calculators and predictors as descriptor generations for you.


Diving into the agenda in a little bit more detail, my biggest goal today is to show you where Chemaxon APIs can provide a genuine upside to your work while also demonstrating that they should be interoperable with the existing libraries and approaches that you’ve come to know and love. Our goal here is by no means to build any sort of walled garden ecosystem. We’re gonna walk in more detail through each step of this process, with a heavy focus on the structure preparation and cleanup steps that I’ve shown here in blue.


While the steps in gray might classically be the central parts of modern training, they are steps that you honestly more likely know better than I do. And indeed, these are the steps that less heavily use our APIs. So while I will go through those steps for the sake of completeness, they won’t be the real focus of the session.


At the end, we’ll quickly touch on some of the other APIs that sit outside of the workflow that we’re presenting, and talk about the ways that you can distribute the fruit of your labor to end users.


To give this all a little bit of scientific rounding, let’s talk about our target. We scraped data from Kemble about this specific colon estrace here, and we’re going to try to model its pKemble value. For those of you who aren’t familiar, this is a value that the folks at the EMBL have come up with that allows the comparison of various disparate but roughly comparable measures. Just to keep things simple, let’s consider it as analogous to PIC fifty for today.


Well, with that, why don’t we go ahead and jump right in and get a grasp of the basics of importing and handling structures with the API.


I’m working here in a Jupyter notebook that will be made available to you, of course, after this webinar. And you have the typical installation and basic setup, installation, and import of the various libraries. And then, of course, you need to go ahead and provide a license key. I’ve hidden that part for obvious security reasons, but I have shown here that it’s very straightforward within something like a Jupyter Notebook to just set the environment variable of your license key and go ahead and get using that.


From here, all of our data is in a CSV, which we’ll go ahead and import with your probably familiar columns, such as a Campbell ID, a smile string, and your activity value. And the first thing we’ll do is go ahead and use our IO function to import that SMILES to create a Chemaxon molecule object. This I’ve stored again in a column in our data frame. There are a couple of different things that you can show here. And in this case, I’ve chosen to have a string preview of the smiles that’s associated with the structure as is, but there is an object type hiding behind this.


If you’re looking to import from an SDF, there’s a reference to the SDF importer here, but our data came in a CSV, and I feel like a lot of yours probably does too.


Then we’re working on some functions to help you convert between RDKit and Chemaxon .


Both of these libraries do have their own molecule type, and we don’t have a straight one to one conversion available just yet. With that being said, we have a couple of different helper functions that allow you to do this through what our current best practice is, which is using a cheminformatic string based representation as an intermediate. Intermediate. Of course, you might want to use different intermediates, so those are baked into this. Smiles is the most compact and usually the fastest and often enough. But in case you’re needing needing to retain more explicit features, you can choose to use a different intermediate, of course.


Next, let’s go ahead and look at our structures. And the first thing I want to do is make sure that there’s only one fragment present within each of the structures. There could be any number of reasons why there is a multi fragment structure object. The simplest being salts and solvents sometimes show up along with the structures. But, of course, it could also be that there is a true mixture of active components. So let’s go ahead and take a look at that.


The first thing we’re gonna do is use our evaluate function to get the fragment count. Now you’ll see later on that there are two different ways to apply calculations through our API.


Many of the most popular functions have their own function, and that will often give a more verbose output, with a number of different results that you might wish to interrogate. And then we have evaluate, which is our chemical terms. This is sort of a wrap around all, and it allows you to not only execute those functions that don’t have a dedicated endpoint yet, but also to make more complex statements. If you’re interested in more, please do reach out, and I’m happy to explain more about that. But for now, I’ve gone ahead and calculated the fragment count here, and let’s just verify that everything only has a single fragment. Excellent.


Now I did this through the generation of the value and then assessment of the result.


But as things get more complicated, we typically want to have a more comprehensive way of validating our structures.


To do that, we would want to use the libraries that come with standardizer and structure checker.


These enable you to perform this full validation that you need to to ensure that your chemical structures are ready for whatever process you’re going to put them through.


First of all, you want to standardize the library. Now in my case, although the data has come from a single data source, it is a public data source, which these often have different levels of curation.


In your cases, the data may come from multiple sources, multiple internal labs, CROs, literature sources, what have you. So it’s really important to curate, not only for normalization, but also to check for structure errors.


After this, you might also want to perform actions such as calculating pKas or from their major microspecies at the given biological pH that you’re interested in, as well as the major tautomers or verifying details around stereo isomers and resonance structures.


The way that this typically looks is that you collate your data together from your one or many sources. You use standardizer to form the canonicalization, and then you run it through structure checker where you go ahead and you look for errors.


And if there are errors, try to use the automated fixers to clean those up for you. And your end result is going to be clean structures that you know have all been fixed in place exactly how you want them, and are ready for further processing.


Again, let’s go ahead and see how that looks within the API.


Alright. So the first thing that we’ll look at is standardizer. At the simplest level, you can go ahead and configure a standardizer using a simple string that tells the standardizer object exactly what it needs to do, to normalize the input structure. So first, we’ll go ahead and take a look at the structure that we’ve imported here.


And you’ll notice that in the bottom left here, there is this histidine group that is collapsed down into an s group. So I create our standardizer object.


And as part of that, I specify for it the standardization configuration that we want to run.


We provide that molecule as an input, and we go ahead and run it, and we see that it has been ungrouped.


Applying or providing the configuration as a string like this, is great once you are very well versed in the application and know how to form these strings and also when they are, you know, smaller, simpler configurations.


But for newer users or for more complex use cases, it can be more comfortable to perform this sort of configuration within a UI. And that’s available to you, where you have your UI. You have all the actions listed down here, many more shown further down the scroll bar, and you’ll go ahead and add these in the order that you want them to be executed, perform any sort of additional configuration. And this is a standalone desktop UI, but you can go ahead and export the configuration from here. That’ll save as an XML file that you can hold on to.


And And then what we’ll go ahead and show here is loading this XML and actually providing that XML to the standardizer as the configuration. So now if we have a more complex set of actions to perform on this different molecule that we’re showing here, we can go ahead and run that, and you’ll see that three things have actually occurred. The first is that there was an explicit hydrogen here up on this carboxylic acid, which I’m not a huge fan of. So within the configuration, I have told it to remove any explicit hydrogens. As with the prior example, there was an s group here, which we’ve gone ahead and ungrouped from a methyl to the c h three that you see.


And then we also have this nitro group here, which is shown in form the neutral form with a pentavalent nitrogen. Not a big fan again, so we’ve gone ahead and changed this to the charged form.


As I mentioned, there’s a great long list of standardizers, but also ways both within the user interface and through custom code to make additional ones should the default list be insufficient for whatever reason.


The other side of this, once we’ve gone ahead and standardized our entire set, is the structure checking.


Structure checker works much the same way. However, it is a two step process. The first is detection of errors, and the second is the fixing of errors. The detection is very simple. The fixing is a little bit more variable.


For many, but not quite all of the errors, we have automated fixers that you can choose to apply. We don’t have them for all because there are some things where you simply don’t know without further user input what the error is from. Take a pentavalent carbon, for example. Which bond is the unnecessary one? Which is the one that we should break? One great example.


So we’ll simply load in another structure as before, and again, I’m going to give you the simple example first where we are looking for, non stereo wedge bonds. The difference between the structure checker configuration here and the standardized one before is that we add this section at the back here, which tells it if you found this error, perform this action in an attempt to fix it.


And if we take a look at this, we’ll see that the wedge here, which was on an achiral center, has now flipped over, and I’m happy that this is an appropriate fix.


Occasionally, especially in cases where we cannot fix the issue automatically, we might wish to get further details about the results. Was the fix successful?


Which atom or bond was the error initially found at, etcetera, etcetera? And we can show some of these by performing further inspection of the results of the structure check and fix attempts.


Again, let’s go ahead and fix all of our input structures.


I’m gonna go ahead and take a quick look at the results and run this here. What we’ve got here is now a few things. We have our initial SMILES that we input. We have the initial, what I’ve called raw chemexon molecule object column. We’ve got information about our fragment count.


Then I standardized that raw to form this column, and you can already see a lot of the changes there from the SMILES preview. And then I went ahead and added another column of the fixed structures and verified that all the fixes were appropriate applied.


Next, I’m just gonna go ahead and clean things up a little bit. I don’t think that we need quite that many columns, so this is now a little bit cleaned up.


And then I also spoke about microspecies determination. Fortunately, we can do both tautomer generation, and protonation state in one. So what we’re going to do here is apply the major microspecies calculator and instruct it as part of its parameters to take the major tautomeric form.


This will go ahead and add an additional column towards the end, that will have the form that we’re interested in.


Excellent.


Now that the structures are in better shape, we should go ahead and take a look at our data. First, let’s take a look at the actual values that we have here. It’s always a good idea to inspect, of course. What are we looking for?


A reasonable distribution of values, a reasonable dynamic range. This dynamic range is a little wide. It’s also bimodal. Is that ideal?


Possibly not. However, I think for the sake of our simple demonstration here today, I’m happy with this.


The other thing we want to do is look for duplicate structures. It’s popular to perform string based comparisons here once the, for example, SMILES have been normalized. It’s also popular to run-in chi or in chi based comparisons.


I’m gonna use the Kymaxon approach here for two reasons. The first is it gives you the most control. In chi can be a little bit rigid when it comes to standardization, and I’ve just done all these steps to standardize according to my preferences, and I don’t want to go ahead and lose that. The other is obviously I’m here to demonstrate features to you. So let’s go ahead and do that.


First of all, duplicate searches. I’m going to go ahead and take an arbitrary entry from our data frame, and we’re going to run a duplicate search. Now this is going to use the chem axon search functionality, and this takes us input, the specification as to what type of search we want to run.


One other thing to note is that there are other optional parameters that you can apply, and that includes an on the fly standardization.


We’ve done the core standardization standardization already. However, there are a few reasons that you might wish to standardize on the fly only for the purpose of the search. Again, this doesn’t impact the in place representation of the structure.


The first reason is that you might just want to store the structure once and run searches with greater or fewer hits or flexibility. So to do that, you can run standardizations to be more or less strict in your search.


For example, you might wish to store the structure with all your salts, but then run a salt less duplicate search, which can be achieved by running salt and solvent removal on the fly during search.


The other is purely for accuracy.


We found, for example, that aromatizing all of your Kukule structures to an aromatic form for these searches is essential. Otherwise, you’re gonna have a lot of false negatives.


So we’ll go ahead and run this. I’m expecting to find exactly one hit, and we do find that. It’s a it’s a self hit to the structure within the data frame, so that’s all well and good.


If we go ahead and run this entire combinatorial search in our data frame, it’s going to take just a few minutes. We’re going to avoid that. The reason for this is that it’s a full atom by atom search, which can take a while. We could do a shortcut, though, to get ourselves in a better spot. And the way that we’ll do this is we’re gonna use the fingerprints functionality to generate the chemically hashed fingerprints for each of our structures.


This is a proprietary fingerprint that we have in house that’s optimized for duplicate and substructure searching.


The reason that we do this is when you generate a fingerprint for two structures to be identical, they must have the same fingerprint. And this quick text based comparison is enough to give us a list of potential matches based on their fingerprints. Now that we’ve done this, we can run that more exhaustive search, but we only have to run it for those forty one compounds, which as you see leads to a very, very fast search result.


I have included, an optional step here that you can run at your own leisure to run the full search whichever oh, on the full set of data if you’d like to.


Oops.


Now, something that we have here that’s a little bit out of scope is a step that you would probably wish to apply in your day to day work. This is further analysis at the data distribution, which I spoke briefly about earlier, but also on the chemical side, clustering, scaffold analysis, and not just reviewing duplicates, but reviewing near duplicates. Again, out of scope for today, I will note that I have a helper function up with the Chemaxon RDKit interconversion functions, that helps to convert the output of our fingerprint. I’ve been playing a little bit with using this in order to perform clustering on public using public libraries such as Bitbirch, and it’s been largely successful in early testing.


Next thing we want to look at is fingerprint generation. You probably already know that there are a large number of calculators and predictors that we have available, and we’re going to use those to generate the input for the machine learning model. Of course, we can use the fingerprints that we’ve just been discussing, but also features such as PKA, log p, log d, etcetera, that you see on the screen here.


I am gonna go ahead and import descriptors from RDKit as well, in part to show that this is very flexible and in part for completeness. There’s no reason not to borrow descriptors from other libraries, if they should be missing from one.


I’m going to largely be running the abbreviated evaluate method of running our calculators, but I did want to give an example of the more explicit or more verbose output that’s output by dedicated functions. So in this case, we’re going to be looking at pKa. If we were to run this through evaluate, we’d probably get just one result, the most acidic or the most basic pKa. For this example, though, I’m actually going to look at getting all of the information.


And what you see here is that there’s a number of pieces of information provided. There are multiple possible protonation sites on this structure. So we have access to the indices that the pKa was calculated for, whether it’s an acidic or a basic pKa, and its actual value.


So this is the sort of benefit that you can obtain from using the more verbose functions.


But back to our workflow.


In this cell here, I’m simply defining a function in order to apply a large number of property predictions, and a lot of these are going to be used as input again for that evaluate function that we saw before. Before.


I also have a large list of RDKit descriptors that are stored in an accompanying file, in order to add those to our list.


And then here, we’re going to go ahead and calculate all of those descriptors based on the SMILES column. I’m doing this for simplicity. I know that seems a little silly knowing that we’ve just worked through all of these different steps. However, I’m concerned about, you know, compatibility, intercompatibility with the RDKit. So, again, for simplicity, we’ll just go ahead and use that column.


And, actually, I’m going to skip that. I have pre calculated these. They don’t take forever to run, but in the interest of time, we will simply go ahead and load them from a CSV that I’ve already saved.


I a little bit more information here, about going through and running near identical or similarity scores, that you can also take a look at. In this case, I’m actually using a variety of other fingerprints that you might be interested in testing that we have available.


Now we’ll go ahead and jump to, I guess, what we could be called the more routine parts of your machine learning modeling pipeline. First, we’re going to go ahead and remove highly correlated features to make sure that we’re not too redundant here.


And the next thing that we want to do is make sure that we go ahead and perform a proper train test split.


And then after removing redundant features, we also want to remove features that are not relevant to our particular model.


Now I ‘ve gone ahead and used an algorithm called the Boruta algorithm. And this is an excellent sequence of steps that you can follow for whenever you’re building any sort of tree based models. I’m just gonna be using random forest and I think a gradient boosted tree later on. So this is an appropriate one to run here. Again, it can take a little bit of time, so I’ll leave you to try this out on your own.


I’m just going to go ahead and read these features in, and you can see that there’s been a significant reduction in the number of columns that we have.


We’ll go ahead and apply that same filtering to our test set.


And from there, we can go ahead and train our model.


Again, I’m using fairly lightweight models here for the purposes of speed and demonstration. They seem to do quite well on our initial testing. Indeed, our charts show the same thing as our statistics.


Simple comparison between the models, and we choose to continue with the gradient boosted model. Again, here, out of scope for today are additional steps. Right? You’re more than likely using more sophisticated models than this. You’ll want to tune hyperparameters. You’ll want to perform proper cross validation in order to verify which model or which set of hyperparameters were the optimum. But I’ll leave you to do that as you do in your regular daily work.


The last but not least, we want to go ahead and actually run some of these predictions on the held out test set.


So let’s go ahead and start these cells here executing. We see that, again, our statistical values are fairly good for this held out test set, so we’re happy with the model.


Now what comes after this? How do we distribute this to our users?


Something that’s very common that we see is wrapping the Python generated models into a REST API. The reason for this is that many applications that end users, like medicinal chemists, use are typical enterprise deployed graphical user interfaces that can operate via REST calls.


An example that works for our application and is probably very similar to whatever your favorite design management tool is, is to wrap it using Flask or FastAPI in a simple set of APIs where you provide information about this plug plugin or model that you’ve created to the application, and it will then go ahead and using post calls, send the structure over to your model, which will then spin its wheels, generate a value, and return that to your application.


What this then ends up with is this interface or whichever interfaces your users are in, where their structures are there, their designs are managed, and their your machine learning model prediction is served to them here. Of course, if your model outputs something more verbose or more exciting than a simple value, then the charts or other associated images can be displayed as well.


I will distribute, along with this Jupyter notebook, a simple example of the output of this model wrapped in an API, in case you’re interested in seeing that.


I also promised to talk a little bit about other parts of the API, and these six boxes here are showing the progress that we’ve made so far in exposing our toolkits through Python. And we focus today on the top three boxes here, although the bottom three are also available. Those are our ADME tox predictors, reaction numeration, and a couple of other quality of life features.


You’ve probably actually already seen one of these, the integrated name to structure, that I showed through my import where aspirin, ******, warfarin, and a few other compounds were imported directly by name.


Let’s go ahead and take a look at these. The built in ADMETox predictors are further examples of the calculations that we have available. You might use these as a baseline to compare your models to, or you might also use these as descriptors if you think that it’s relevant to your particular situation.


The next is library enumeration. I don’t have a fully fleshed out example here, although examples from our public GitHub repo where we have actually examples for all of these different use cases, is linked for you. In short, this allows you to take a set of input compounds, your reactants, and a defined list of reactions that you are comfortable performing, have the means to perform. Maybe you filtered by how reliably they generate the desired output, and you can go ahead and virtually run those reactions. This allows you to quickly say, hey. I I have these compounds, but what compounds could I have in two or three days physically in my lab knowing just that I can run these through the reactions that we have on file?


And then last but not least, there’s a few helper functions, the first of which is this display option here. This works very well if you’re actually running your Jupyter Notebook in the browser. What it allows you to do is rather than the molecule object preview or the smile string preview that I had in my data frame, you can generate an image based preview there instead, which is sometimes, more comfortable for people and easier to look at and parse the parse.


With that, we’ve already reached the end of our time together, and I’d like to thank you, of course, for your attention. We’ve gone through a number of different functions of our Python API. We really focused on structure manipulation as well as descriptor generation as the lead up to our modeling steps and hopefully demonstrated that this should slot right into the workflows that you already have. I’m really excited to hear more about how you feel about this, any further expansions that you’d like to see to our Python API. And again, thank you for your attention.


Thanks, Jan, for the insightful demonstration.


We received several excellent questions. To ensure they get the complete answer they deserve, a member of our team will follow-up via email in a couple of days.


If you’d like to execute any of the examples shown today, please contact our colleague for a license key. His email address is shown on the slide.


Thanks for spending your morning with us. Bye bye.


### Speaker:


[Jan Christopherson](https://www.certara.com/teams/jan-c-christopherson/) Senior Solutions Consultant


Jan Christopherson is a Senior Solutions Consultant working in Certara’s Discovery Portfolio. Following a stint at Mettler Toledo focused on laboratory instrument software interfacing and 21CFR compliant validation, he joined Chemaxon – acquired by Certara in 2025 – in 2019, where he has since supported customers through the full application portfolio and lifecycle.


## Learn more about Chemaxon Discovery Toolkit


The Discovery Toolkit provides you with top notch chemical intelligence including all our toolkit bundles; all calculators, predictors and structure preparation tools through all available programmatic interfaces for easy integration.


[Learn more](https://www.certara.com/discovery-toolkit/)


## You May Also Like


AllCheminformatics


[Cumyl‑PeGaClone: Emerging Synthetic Cannabinoids and the UK’s Evolving Legal Response](https://www.certara.com/blog/cumyl-pegaclone-emerging-synthetic-cannabinoids-and-the-uks-evolving-legal-response/)


[Cumyl‑PeGaClone: Emerging Synthetic Cannabinoids and the UK’s Evolving Legal Response](https://www.certara.com/blog/cumyl-pegaclone-emerging-synthetic-cannabinoids-and-the-uks-evolving-legal-response/)[Blog](https://www.certara.com/category/blog/)


### [Cumyl‑PeGaClone: Emerging Synthetic Cannabinoids and the UK’s Evolving Legal Response](https://www.certara.com/blog/cumyl-pegaclone-emerging-synthetic-cannabinoids-and-the-uks-evolving-legal-response/)


[Why speed and scale matter in your discovery pipeline – A scalable compound registration](https://www.certara.com/blog/why-speed-and-scale-matter-in-your-discovery-pipeline-a-scalable-compound-registration/)


[Why speed and scale matter in your discovery pipeline – A scalable compound registration](https://www.certara.com/blog/why-speed-and-scale-matter-in-your-discovery-pipeline-a-scalable-compound-registration/)[Blog](https://www.certara.com/category/blog/)


### [Why speed and scale matter in your discovery pipeline – A scalable compound registration](https://www.certara.com/blog/why-speed-and-scale-matter-in-your-discovery-pipeline-a-scalable-compound-registration/)


[Precursor chemicals – Sine qua non](https://www.certara.com/blog/precursor-chemicals-sine-qua-non/)


[Precursor chemicals – Sine qua non](https://www.certara.com/blog/precursor-chemicals-sine-qua-non/)[Blog](https://www.certara.com/category/blog/)


### [Precursor chemicals – Sine qua non](https://www.certara.com/blog/precursor-chemicals-sine-qua-non/)
