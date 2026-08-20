---
schema_version: "1.0.0"
document_id: "b441ddbde705cb8dc3cc859967bde1c14cbf1830434dc5044be7d9428a41a6e2"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/in-silico-pcr-primer-design-and-gene-amplification"
published_at: "2019-04-10T16:00:00+00:00"
first_seen_at: "2026-07-20T03:30:03.260200+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:82d18021813b36c8c34d9dcb25fd9b09d0d0eca5bc95a4729f7ac3577f3e901b"
---

# In Silico PCR Primer Design and Gene Amplification

DNA amplification with polymerase chain reaction (PCR) is a ubiquitous protocol that scientists across academia and the industry know well. (You may even have been introduced to[PCR in song](https://www.youtube.com/watch?v=x5yPkxCLads) once upon a time.) In a typical PCR protocol, the first step is denaturation, to separate the template DNA strands. Then by annealing, or decreasing the temperature, the DNA primers are able to bind to the respective DNA template strands. At this point, polymerase, the active enzyme, builds off of the primer to form the new DNA strand. Then this process is repeated to the nth degree until there are enough new copies of DNA to execute on downstream experiments.


PCR is a crucial step in many in vitro research pathways, and now modern software tools, like the Benchling platform, empower scientists to prepare for their experiments with in silico modeling. The[Benchling Life Sciences R&D Cloud](https://www.benchling.com/life-sciences-cloud) includes tools to model a PCR product, and design and visualize primers for PCR. Beyond its software capabilities for primer design, Benchling also allows you to store your primers- novel or extant- to your workspace and project; you can link them in[Notebook](https://www.benchling.com/notebook) entries or save them for a later date.


## Setting Up with Existing Primers


First select the sequence you wish to amplify. To do this, navigate to your project inventory in the left side panel and click on your target gene. For this tutorial, we will be using the BRCA2 gene. Your target sequence will show in the Molecular Biology application, which will offer a host of insights, including a linear map and any relevant metadata.


Now, navigate to the primer button on the far right panel and select *Attach Existing* to include pre-made primers from your inventory.


In the *Find Binding Sites* window, make sure to find binding sites *for all primers in specified folders* , which you will select from the first drop down window. Next, select the folder or folders that contain your saved primers. For this post, we have selected the “PCR Example Inventory” folder that is within the project “Example Project.”


Be sure that your settings match the default settings shown in the image below. Then select the *Find Binding Sites* button.


Benchling will then retrieve any suitable primers from your selected folder and display them in the search results. You can sort these results by location, melting temperature, primer name, and strand. Select your preferred forward and reverse primers, then select *Attach Selected Primers.*


### Primer Design: Method 1


If you haven’t already imported primers into your inventory, navigate to the sequence map of your target gene. From there, left click and drag your cursor to highlight the region where you want your primer to bind. At the top right of the sequence map, select *Create,* then *Primer* , and then *forward* or *reverse.*


You will find your primer design tab in the right-hand window – from there, click *Set From Selection.* In this window, you can also design, verify, and save your new primer. You can adjust the number of bases in your primer by clicking and dragging the black bars that border the highlighted region of your sequence. Be sure to give your new primer a name. In this window, you can also set the 3’ location, designate the overhang length, and verify the melting temperature and GC content.


Once you have selected your primers, click the button *Attach Selected Primers* in the top right of your window.


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


### Primer Design: Method 2


To create a pair of primers simultaneously, first navigate to the *Primers* button on the far right panel, and select *Create Primers,* then *Manual* . From the small dropdown in the top left corner of the right side window, select *Primer Pair* .


From there, similar to the first method, you will click and drag your cursor on the sequence map to highlight the region where you wish to attach your forward primer. Click *Set from Selection* ***,*** and then select *Forward* for your forward primer. Then, for the reverse primer, do the same at the 3 prime end of the target sequence you want to amplify: highlight, *Set from Selection,Reverse.*


In the right side window, you can assess your primers for GC content, melting temperature, and length. If you find these metrics need to be adjusted, simply re-highlight a different region. In manual primer design, you can also set restriction enzyme cut sites, overhangs, and check secondary structure to see Gibbs Free Energy values for monomers or homodimers.


When all is set, name your primer, choose a folder, and select *Save Primer Pair.*


## Creating Your PCR Product


Now that you have completed your PCR primer design- whether you selected primers from your inventory or you just created a new primer pair in Benchling- navigate back to your sequence map to see the primers highlighted along your sequence. Next click on the *Primers* button in the right-most panel. Shift-click both of your primers and select *link primers.*


In the next series of pop-up windows, select *create PCR product,* select the features you wish to have copied with your DNA, and then select the folder in which you like to save your new PCR product.


And there you have it, your in silico PCR product! Modeling PCR in silico empowers you to know more about your sequence before executing protocols in the lab. From here, you can use your amplified gene of interest to model downstream procedures in Benchling, such as digestion and ligation, plasmid design, and transfection.


## Accessing Your Primers and PCR Product


Now that you have designed your primers and amplified your target gene, you can access and share this data in a few ways. Navigating to the Notebook, Benchling’s ELN, you can @mention the file name to include a direct link to that file in your lab notebook entry. With access to the Registry, you can register your target sequence, primers, PCR product to your Benchling account’s data warehouse. With access to Inventory, you can link these in silicomodel outcomes to the in vitroproducts you construct in the lab.


Editor's note: Header image["Experiment"](https://www.flickr.com/photos/140436993@N06/26772706097) by[OIST (Okinawa Institute of Science and Technology)](https://www.flickr.com/photos/140436993@N06) is licensed under[CC BY 2.0](https://creativecommons.org/licenses/by/2.0)
