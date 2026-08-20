---
schema_version: "1.0.0"
document_id: "afd7b88851e08147e1888aa6337739b198fb5f90fc3411acb9953d463f60a4a4"
company_key: "yc-biodock"
company: "Biodock"
source_id: "yc-biodock-rss-567b1dbd4991"
canonical_url: "https://blog.biodock.ai/free-microscopy-software/"
published_at: "2025-09-19T14:08:04+00:00"
first_seen_at: "2026-07-24T19:19:16.186046+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:97f032f864ef9f2cde2adfc1833a1513676c04ecda25d1aaed5bd4ca132bb82f"
---

# Open-source image analysis software for microscopy

Microscopy provides a rare opportunity for biologists—visualization of their science. Many biologists spend their day pipetting small volumes of liquids between tubes. Any reprieve from the mundane is welcome and microscopy provides this in abundance. Indeed, the proverb “seeing is believing” holds true even amongst scientists. However, microscopy isn’t just the process of capturing an image it also requires processing and quantification. Herein we will cover open-source software options for image analysis and the features they provide.


The first software to become familiar with is[ImageJ](https://imagej.nih.gov/ij/?ref=blog.biodock.ai) . ImageJ is a Java-based image processing program that was initially developed by the National Institutes of Health (NIH) and the Laboratory for Optical and Computational Instrumentation at the University of Wisconsin. The original version of ImageJ is developed on the public domain (i.e., no exclusive intellectual property rights). Current and related ImageJ projects are all licensed with a permissive[BSD-2 license](https://imagej.net/licensing/bsd?ref=blog.biodock.ai) , which is to say there are minimal restrictions on the usage and distribution of the software. I introduce licensing here intentionally, as this pertains to all open-source software discussed herein.


ImageJ can run as a downloadable application, on any computer with Windows, Mac OS, or Ubuntu Linux operating systems. Although the desktop application may not be visually stunning, ImageJ is a robust software. ImageJ can do simple things such as crop, label, and modify an images brightness and contrast. It can handle Z-stack confocal images and perform quantitative analysis. ImageJ can display edit, analyze, process, save, and print 8-bit color and grayscale, 16-bit integer, and 32-bit floating point images. It can read many images file formats, including TIFF, PNG, GIF, JPEG, BMP, DICOM and FITS, as well as raw formats. One nice feature of ImageJ is it can come as a predownloaded package with a variety of useful plugins in a package called Fiji. Some utilities included in Fiji include 3D viewing, video editing, and measuring colocalization. The design of ImageJ is done in such a way that users can extend functionality of the software by simply downloading Java plugins or recording custom macros. The software can run custom acquisition, analysis and processing plugins designed by users either through ImageJ’s built-in editor or a Java compiler. These features, specifically the plugin architecture and built-in development have made ImageJ the popular platform for image processing. However, be aware as one issue and caveat with ImageJ is you need an understanding of Java to write custom macros for image batch analysis or other more advanced processes.


Graphical user interface and tool icons for ImageJ. Source: Nono64; Creative Commons.


The second open-source software is[CellProfiler](https://cellprofiler.org/?ref=blog.biodock.ai) , a cell image analysis software and its companion software CellProfiler Analyst, a data exploration software. CellProfiler was released in 2005 from the Whitehead Institute for Biomedical Research and MIT. CellProfiler is designed with biologists in mind and is hosted by the Broad Institute. Originally CellProfiler was developed in MATLAB but it was re-written in Python and released as CellProfiler 2.0 in 2010 Version 3.0 as the open-source available today.


CellProfiler, unlike ImageJ can run only on Mac OS and Windows operating systems as a desktop application. In my opinion the user interface of CellProfiler is more intuitive than ImageJ with helpful icons and online tutorials to walk you through an image processing task. CellProfiler can do the basics of image analysis but is meant to be used as a series of image-processing modules which can read and analyze most common microscopy image formats. In terms of managing workflow, one does not have to be an expert in batch scripting in Python. Rather there are graphical instructions (i.e., PowerPoint slides) guiding the user through each step or extensive help resources online (i.e., blog forums, YouTube).


[Icy](http://icy.bioimageanalysis.org/about/?ref=blog.biodock.ai) is another open-source software option founded by the Institut Pastuer and France-BioImaging. Similar to both ImageJ and CellProfiler with basic image processing capabilities but can also run pre-writeen modules and run custom macros to fit the users’ specifications. Icy has an extensive user manual with[training material](http://icy.bioimageanalysis.org/trainings/?ref=blog.biodock.ai) designed for anyone wanting to run the software, regardless of experience.


Beyond ImageJ, CellProfiler and Icy, which are “stand alone” open-source software for image processing and analysis, the remaining open-source software are specialized for specific image processing tasks (i.e., object identification) or image type (i.e., whole slide image)


[Ilastik](https://www.ilastik.org/?ref=blog.biodock.ai) is an interactive learning and segmentation toolkit. It can run on any operating system and includes an extensive user manual. It allows the user to leverage machine learning algorithms to easily segment, classify, track, and count your cells or other experimental data. Operations are interactive where the user draws the labels and immediately see the results.


[QuPath](https://qupath.github.io/?ref=blog.biodock.ai) is another open-source software for bioimage analysis. QuPath is often used for whole slide images common in pathology, but it can be used for other microscopy images too. Indeed, QuPath was[published](https://www.nature.com/articles/s41598-017-17204-5?ref=blog.biodock.ai) and designed under the merits of being user-friendly and specific for visualization and computational challenges posed by whole slide images. Furthermore, it includes features with batch-processing and scripting functionality. The software runs as a desktop software application and can integrate with existing software and platforms such as ImageJ and MATLAB.


QuPath image cell detection example with red, cell border and purple, cell nuclei. Source:


[QuPath 3.0 Documentation](https://qupath.readthedocs.io/en/stable/docs/starting/first_steps.html?ref=blog.biodock.ai) .


[Neuronstudio](https://en.wikipedia.org/wiki/NeuronStudio?ref=blog.biodock.ai) is a highly specialized software package for reconstructing neurons from 3D confocal images. The software was developed at the Computational Neurobiology and Imaging Center at the Neuroscience Department of the Mount Sinai School of Medicine in New York. Reconstruction of neurons can range from fully automated to fully manual. The images as well as the detected objects are rendered in 3D—2D is also supported. The software includes neuronal dendritic spine detection and a classification function. Development of this software appears to have ceased back in 2009, but Windows source code is still available.


Additional open-source resources worth mentioning are[VLC](https://www.videolan.org/?ref=blog.biodock.ai) ,[Imaris Viewer](https://imaris.oxinst.com/imaris-viewer?ref=blog.biodock.ai) ,[ZEN Lite](https://www.zeiss.com/microscopy/us/products/microscope-software/zen-lite.html?ref=blog.biodock.ai) and[LAS X](https://www.leica-microsystems.com/products/microscope-software/p/leica-las-x-ls/?ref=blog.biodock.ai) . VLC is a free and open-source cross-platform multimedia player and framework that plays most multimedia files. Imaris Viewer is a free version of Imaris with limited scope; it is useful for viewing Imaris files or raw data. ZEN Lite is a free software from Zeiss to open .czi files but only runs on Windows. Lastly, LAS X is Leica’s free software to open images acquired on Leica microscope controlled by LAS X--LAS X software only runs on Windows.


Now if you want to process an image through your own pipeline both Python and MATLAB also include image analysis packages. However, MATLAB is proprietary, closed-source software so I won’t cover it here, but is worth mentioning in case you have access to a license. Python packages[scikit-image](https://scikit-image.org/?ref=blog.biodock.ai) and[OpenCV](https://opencv.org/?ref=blog.biodock.ai) are designed for machine learning and computer vision/image processing, respectively.


Finally, in 2020 the Center for Open Bioimage Analysis (COBA) was established with a P41 grant from the NIH National Institute of General Medical Sciences to Co-Principal Investigators Carpenter (Broad) and Eliceiri (UW Madison). As a National Biomedical Technology Research Resource, the purpose is to develop and disseminate novel technologies through the biomedical research community. COBA will continue to develop and maintain the open-source software CellProfiler and ImageJ while making new deep learning tools and workflow solutions for bioimaging. Together all this computational work will be developed in the context of driving biological projects for the community.
