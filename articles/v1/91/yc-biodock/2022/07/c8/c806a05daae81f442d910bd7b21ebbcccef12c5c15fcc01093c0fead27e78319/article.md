---
schema_version: "1.0.0"
document_id: "c806a05daae81f442d910bd7b21ebbcccef12c5c15fcc01093c0fead27e78319"
company_key: "yc-biodock"
company: "Biodock"
source_id: "yc-biodock-rss-567b1dbd4991"
canonical_url: "https://blog.biodock.ai/semantic-vs-instance-segmentation/"
published_at: "2022-07-27T14:41:35+00:00"
first_seen_at: "2026-07-24T19:19:16.186046+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:a8414765c004bead3f96551e13f60e2e433369e0242e9490e9c516a475d1c517"
---

# Semantic vs. Instance Segmentation: Applications for Biology

Anyone with moderate to severe nearsightedness (myopia) will tell you that losing their glasses is devastating. I learned this the hard way in Costa Rica when the Pacific Ocean swallowed my pair, and I was left with my brutal myopia for the rest of the trip. I recall sitting at restaurants and looking confusedly at menus. I could make out where words were written but I did not know where one word started and another ended. Other times, when looking at people in the distance, I could make out the presence of a crowd but was unable to distinguish individuals and ascertain how many people were present. It was not until I looked closer at the menus and walked nearer to the crowds that individual words and people came into view. This silly albeit tragic tale evinces a key aspect of visual discrimination and classification: that is that there is a difference between distinguishing between two things and identifying the constitutive units of a thing among its group.


This discriminative ability has become a primary imperative of image segmentation models and can be distinguished by the names semantic and instance segmentation. Such models have also become useful in the field of biology where instance and semantic distinctions are made on histological images across many subfields. Here, I will discuss the differences between semantic and instance segmentation approaches for images of biological tissue by underscoring how extant models are being used to solve real biological problems with image analysis.


## Table of contents:


**1. Semantic vs. Instance segmentation: The basics**


**2. Semantic Segmentation**


a. Continuous distinctions: Boundary Detection


b. Categorical distinctions: Disease detection


**3. Instance Segmentation**


a. Cell count and nuclear organization with Nuclei detection


b. Bacterial colonies


c. ROI extraction for Calcium Imaging


**4. Semantic vs. instance segmentation: Which to choose and why?**


### Semantic segmentation finds the crowd whereas instance segmentation finds the individuals


Consider a crowded room of your friends. In a snapshot of the image, semantic and instance segmentation models can be used to pick out people from the background. In the semantic model the output would be a silhouette of the whole group against the background (Figure 1a). In the instance model, the output would be the silhouettes of each person against each other and the background (Figure 1b). The fundamental reason behind these outputs is that each model is being asked to perform different tasks. The semantic model is often performing a binary classification for each pixel of the image, that is: is this pixel part of the person category or the background category? Meanwhile, the instance model is being asked to perform a binary classification and object distinction, that is: is this pixel part of a person category or the background category? And… which person does this pixel belong to?


[Figure 1: Semantic vs. instance segmentation applied to the same image](https://www.analyticsvidhya.com/blog/2019/07/computer-vision-implementing-mask-r-cnn-image-segmentation/?ref=blog.biodock.ai) (a) segmentation of people against the background (b) segmentation of each individual against the background


Both questions have vital relevance in biology where images of tissues are routinely analyzed to distinguish qualities and individuals. What sorts of images are being analyzed and what classifications are such models being tasked with making?


## Semantic Segmentation in Biological Samples


### Semantic segmentation finds the boundaries of cells and multicellular structures to quantify continuous morphological features.


One powerful application for semantic models is the classification of cellular membranes against extracellular and intracellular space. This classification is performed routinely across fields and histological samples and is often used as a base to iterate upon for instance segmentation. At a basic level, a machine learning model is tasked with classifying pixels as being a member of a cellular boundary or not. The prediction can be read out in an image where each pixel value represents the confidence of the model in making that prediction (Figure 2).


Figure 2: Semantic model segments boundaries of muscle fibers (a) fluorescently stained muscle fibers in a section of muscle where green is laminin (b) output image segmenting the fiber boundaries from 'a'


In muscle biology, a program I developed called[Myosoft](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0229041&ref=blog.biodock.ai) , performs this classification on sections of muscle tissue. The output of this classified image enables the program to ultimately perform instance segmentation to extract morphological information on individual muscle fibers across a section of the whole muscle. This allows you to quantify the distribution of muscle fiber morphology such as cross-sectional area which is a crucial metric influenced by physiological changes and disease states. Although this is an example of boundary detection applied to muscle tissue, similar models have been developed for a[dipose tissue](https://imagej.net/plugins/adiposoft?ref=blog.biodock.ai#:~:text=Adiposoft%20is%20an%20automated%20Open,under%20Windows%2C%20Linux%20or%20macOS.) ,[cerebral tissue](https://www.nature.com/articles/s41592-021-01330-0?ref=blog.biodock.ai) , and[plant cell walls](https://pubmed.ncbi.nlm.nih.gov/32723478/?ref=blog.biodock.ai) .


Semantic models are also predicting the boundaries of more complex multicellular structures such as veins.[The lab of Jose Rouco in Spain developed](https://www.sciencedirect.com/science/article/pii/S0933365721001093?via%3Dihub&ref=blog.biodock.ai) a model to segment retinal arteries and veins. They implemented a convolutional neural network-based model to perform a binary classification on processed retinographs. The model outputs predictions for whether a pixel belongs to an artery/vein or extra-retinal space (vitreous humor) (Figure 3). From this prediction, scientists can extract relevant morphological features of the segmented vasculature such as length, branches, and surface area covered. This information can be usefully integrated into diagnostic approaches for retinal diseases such as[Macular Degeneration](https://www.hopkinsmedicine.org/health/conditions-and-diseases/agerelated-macular-degeneration-amd?ref=blog.biodock.ai#:~:text=Age%2Drelated%20macular%20degeneration%20(AMD)%20is%20a%20disease%20that,diet%20high%20in%20saturated%20fat.) ,[glaucoma](https://www.nei.nih.gov/learn-about-eye-health/eye-conditions-and-diseases/glaucoma?ref=blog.biodock.ai#:~:text=Glaucoma%20is%20a%20group%20of,a%20comprehensive%20dilated%20eye%20exam.) , and diabetes all alter the architecture of retinal vasculature.


[Figure 3: Segmentation of retinal arteries from a retinograph image](https://www.sciencedirect.com/science/article/pii/S0933365721001093?via%3Dihub&ref=blog.biodock.ai)


### Semantic Segmentation makes categorical classifications to automatically identify distinct regions and disease states


Although semantic segmentation can be used to define ROIs that can be quantified to extract continuous variables such as cross-sectional area, it also has widespread potential for making categorical classifications. In the field of biology, this is especially useful for identifying disease states and histological features/regions of interest.


[Sickle cell anemia](https://www.mayoclinic.org/diseases-conditions/sickle-cell-anemia/symptoms-causes/syc-20355876?ref=blog.biodock.ai) is caused by a point mutation which results in red blood cells assuming a deformed sickle shape which makes them inefficient oxygen carriers. Due to the distinct morphological profile of sickle cell red blood cells,[one group developed](https://ieeexplore.ieee.org/document/9122550?ref=blog.biodock.ai) a deep learning-based model to perform semantic segmentation on red blood cells where the model would find red blood cells and then classify them as either sickled or healthy. They demonstrated that their model could predict red blood cell type with high accuracy. Incorporating multiple classes of red blood cells beyond sickle vs healthy was also successful. This presents potential as a powerful diagnostic tool and demonstrates how semantic segmentation can be applied to discriminate disease states when such states are associated with morphological differences. Beyond discriminating cell types to distinguish a disease state, semantic models can also be used to classify images of whole tissue and identify regions of interest.


Specifically,[one group aimed](https://ieeexplore.ieee.org/abstract/document/9453850?ref=blog.biodock.ai) to automate the detection and segmentation of the fetal cerebellum. The cerebellum is a brain region comprised of a dense network of neurons involved in motor coordination and motion calibration. This structure is also used as a benchmark for brain development during fetal ultrasound scans as structural changes report neurodevelopmental stages and might report neurodevelopmental outcomes. The authors developed a deep learning algorithm to automatically segment the cerebellum in two-dimensional ultrasound images (Figure 4b). Their model was highly accurate with an 86% overlap compared to manual annotation, making it a reliable and efficient alternative.


[Figure 4: Automated semantic segmentation of fetal cerebellum](https://ieeexplore.ieee.org/abstract/document/9453850?ref=blog.biodock.ai) (a) diagram of angle of sagittal section of fetal ultrasound scans (b) unsegmented ultrasound image (c) segmented ultrasound image with cerebellum ROI in green


## Instance segmentation in Biological Samples


### Instance segmentation reliably reveals the distribution, quantity, and structural profile of individual cells and cellular components over space.


One nearly ubiquitously imaged and analyzed cellular feature is the nucleus. Not only does the nucleus offer a direct measure of cell count and location but it is also used as a biomarker for certain cellular states. For example, In muscle fibers, translocation of the nucleus to the center of the fiber, termed[centrally nucleated myofibers](https://pubmed.ncbi.nlm.nih.gov/10574566/?ref=blog.biodock.ai#:~:text=Centrally%20nucleated%20fibers%20(CNFs)%20are,dystrophies%20and%20their%20animal%20models.) , is a biomarker for damage and typically occurs following muscle stress. This analysis, howsoever important, is typically performed manually which is labor-intensive and unreliable in large sections that can contain thousands of nuclei. Therefore, many groups have aimed to develop automated ways of segmenting individual nuclei.


This is a prime task for an instance segmentation model. One group used a Mask R-convolutional neural network as their segmentation framework to develop a deep learning-based model to segment nuclei in H&E-stained images (Figure 5). What’s impressive about their program is that it was able to perform accurate instance segmentation of nuclei over a variety of tissue types such as liver, prostate, and colon. Furthermore, accuracy was consistent across patients. This demonstrates the model’s ability to extract instances of nuclei in cases of image variability.


[Figure 5: instance segmentation of nuclei from H&E stain](https://bmcbiomedeng.biomedcentral.com/articles/10.1186/s42490-019-0026-8/figures/1?ref=blog.biodock.ai)


Models for instance segmentation can also go beyond identifying homogenous cellular features but can be generalized to segmenting whole individual cells. In[one such example](https://elifesciences.org/articles/65151?ref=blog.biodock.ai) , a group developed a U-Net based framework to perform instance segmentation of bacteria. Quantifying the distribution and morphology of bacterial colonies is an essential metric for understanding bacterial growth and microbiome diversity. In their study, they were able to apply their network to segment different strains of bacteria over different microscopy techniques (Figure 6).


[Figure 6: Instance segmentation of different bacterial strains](https://elifesciences.org/articles/65151?ref=blog.biodock.ai)


These examples demonstrate the application of instance segmentation to define the quantity and distribution of cellular components over ***space*** . How, though, might instance segmentation be used to define the distribution of cells to ultimately determine their physiological state over ***time*** ?


### Utilizing instance segmentation to define cell location and physiological state over time


In neuroscience,[optical readouts of neuronal activity](https://www.youtube.com/watch?v=SQaueBgLknI&ref=blog.biodock.ai) have become ubiquitous tools for understanding neural circuit function. This method fundamentally works by impregnating neurons with a protein that reports calcium binding with light. In this way, the concentration of calcium can be read out optically since calcium is directly tied to neural activity. Neural activity can be read out in turn. For more information on calcium imaging, see[my article on holographic optogenetics](https://blog.biodock.ai/semantic-vs-instance-segmentation/blog.biodock.ai/holographic-optogenetics/) ! Obtaining the activity of individual neurons necessitates defining where that neuron is located in space and extracting the light intensity of that region over time. Traditionally this was performed manually which is tedious and does not permit the[demixing of neural activity](https://elifesciences.org/articles/10989?ref=blog.biodock.ai) of overlapping cells. Recently, however,[a group implemented](https://elifesciences.org/articles/28728?ref=blog.biodock.ai) constrained[non-negative matrix factorization](https://blog.acolyer.org/2019/02/18/the-why-and-how-of-nonnegative-matrix-factorization/?ref=blog.biodock.ai) , an unsupervised learning algorithm, to perform instance segmentation on raw imaging data. This segments individual neurons within the population and allows for the extraction of intensity from individual cells (Figure 7).


Figure 7: Instance segmentation of neurons from live calcium imaging with denoised neural traces


### The advantages and disadvantages of semantic and instance segmentation are complementary


Both semantic and instance segmentation have their drawbacks and advantages. For semantic segmentation, the primary advantage is that it is easily implementable and highly accurate. Since the model is performing binary classification, additional steps to segment instances do not have to be taken into consideration. As a direct effect, however, this limits your analysis to quantifying qualities of the whole morphology of what you are studying. Conversely, Instance segmentation suffers from being harder to implement and achieve high accuracy from. As a result, many methods depend on some user supervision to revise and check the automated output. The benefit, however, is that with access to individual units, the dimensionality of data is much higher. The decision between implementing semantic or instance segmentation depends on the nature of the question you are asking as a scientist. Are you interested in characterizing the qualities of one structure or many structures? Do you need individual data or will population data suffice to address your hypothesis? With whichever segmentation framework you chose, you can use Biodock to design a custom AI module to meet your needs![Reach out](https://www.biodock.ai/contact/?ref=blog.biodock.ai) to examine all the services Biodock has to offer.
