---
schema_version: "1.0.0"
document_id: "0f997d0dc9b19a1ee4d3a6f40875935e724770549b4b63bc611a853ed35bc41a"
company_key: "yc-redbrick-ai"
company: "RedBrick AI"
source_id: "yc-redbrick-ai-news-import-4b943afcd495"
canonical_url: "https://www.redbrickai.com/blog/2024-01-20-segmenting-aorta"
published_at: "2024-01-20T00:00:00+00:00"
first_seen_at: "2026-07-24T11:23:16.380780+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:94c08c9a68a9b2f57ef67eba9151bcc36c50b529db4411351210e360b26a15f3"
---

# Guide: Segmenting the Aorta and Iliac Arteries in Abdominal CTA Scans

This document details how to use the best tools and workflows for segmenting the aorta in abdominal Computed Tomography Angiography scans on RedBrick AI.


## Project and data setup


Creating a project and importing data


### Create the Aorta CTA taxonomy


Let's start by creating a taxonomy to define what we will annotate. In this project, we're going to segment the Aorta and Iliac arteries:


1. Aortic Tree: Branching system of arteries originating from the aorta, distributing blood throughout the body.
2. Common Iliac Artery: Artery bifurcating from the abdominal aorta, dividing into external and internal iliac arteries for the pelvic region and lower limbs.
3. External Iliac Artery: Branch of the common iliac artery, continuing as the femoral artery, supplying blood to the lower limbs.
4. Internal Iliac Artery: Branch of the common iliac artery, supplying blood to the pelvic organs, gluteal region, and parts of the thigh.


*Please[visit our documentation](https://docs.redbrickai.com/projects/taxonomies) for instructions on creating taxonomies.*


### Create an Aorta CTA project using taxonomy


Now, lets create a project with a label and review workflow using the taxonomy we just created.


*Please[visit our documentation](https://docs.redbrickai.com/projects/get-started-with-a-project) for instructions on creating projects.*


### Upload CTA NRRD data within your project


Now, let's import a few CTA studies into our project. For this use case, we will use data from the study "[AVT: Multicenter aortic vessel tree CTA dataset collection with ground truth segmentation masks](https://pdf.sciencedirectassets.com/311593/1-s2.0-S2352340921X00078/1-s2.0-S2352340922000130/main.pdf?X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLWVhc3QtMSJGMEQCIADXffJtwQb%2BMnnQOfahSWipJs4JnFgujTnN%2F%2FpVPqlvAiAliFEMfIWKwad%2FKMdUgtYUqPLy6JJoVWEAW9a%2B559Tmyq8BQjo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAUaDDA1OTAwMzU0Njg2NSIMIguDL38VY8VW8iS1KpAFiVgR056SCtGXpQXuzRv23uOvQPgiqX0r5K%2BIgW3NplZYu1ptOrIeP42zhH3uFm%2FLdx9ZGaODhYG1XeYVh3SfSCRFMjClUArj15idxh44YW%2Fw%2BZUyvA7KGITP9uPBGlNeyvQ6UpZaQ3MT%2F355JbgXqbP6zxXtqvVVr6EcLo8B70AOg3hFROg%2Faj3vKlcl%2FYNthlgWEEARfEvQZcJ5sf19efA8Z6u%2FktmhcNcxInDqn9CQXvwFwM7V7WHe6iqz3N6x2tOn%2FtLnTv7Rt8gqA7t0QkeNXgtGeq4GRE2d8tzgv6yLDGD9jnPy8JulvQigez8MDw4sr4%2B2%2FUj8GsGL3HVEt%2FjYuN62tEI2YaG09xPvMxULknMvwHbWxuy%2FjXLyuc11eY724mUlEneo%2Fb33fWkC4G5fZlQ9ermqtvExB4fk9XiiP3a1vEkpb8uiJQbVTDCqmRzOEH7VUsNUQS9z4NA%2BGAlfK2etdd0oeB2SskrzBcyX5irjdveJlrXlho%2B%2BbD8WpBzaKihYEAbRnrdMtX0oGetK79mZf%2FbTQoZQhqWpQpXcw7qXTe3FWnjsutFxLaRTDCucM8kO6j2BMsDR0xut1HbiafcLuIYu5Q3D6viiW2%2FA6E%2BZtKFjvwryr9Zg9ywNrDs%2B6aGa8%2B9%2FHBt1fiUhr6bYONiqVcWB%2FwiK%2Bzh6XgVVATjfIJAWndmrENJx73zvELhJ2SMj7atO0nHp4OMMbPkaSOCiMrwxApRN3yhW4bGjW9mha%2BmnRxA%2BWHekEQ%2FZcMz0AakQwnNeD%2BVm08tITlRhBEZTx%2B8poeH2Ut5tEgiiaw42p3JMHdWipnYxa8s20qCL9IEBzNX0WO%2BUwzMoOgowGA9prQG%2FvIQb2oQ1%2Bcww8uTzrAY6sgFQ%2ByoY9W3CDB%2FZkWHa7sG7DbRB%2F%2Fc%2BEj2OKAG1EG%2FPk4vvEe%2BLDNc4J11ZGVU943FNuAxCD4YXDfS9v16P4K65KsVVj6TdN8nXxPNPN3SS%2Bk69XHXK9Ur3P8gDsa6ROBV5PA%2FC8DstygHTjoZ%2FWkuiQvHmePbFKlUNeA%2FQmXz8JfrYshN4BGvSc3IoIQTCYFhmwZUukq7ziIPf8TL3ZK1tZz2wgmoDwjCXq0J9%2FSensA67&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240109T082229Z&X-Amz-SignedHeaders=host&X-Amz-Expires=300&X-Amz-Credential=ASIAQ3PHCVTYW2P45QSP%2F20240109%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=869fbc5dbba757a1a43b4ea24e99fb6da80712fc02f609127d5c220c73037902&hash=4f9c2d89531a4372eefff62e4b1bfbef772157f37474bb465ca32d84248e5454&host=68042c943591013ac2b2430a89b270f6af2c76d8dfd086a07176afe7c76c2c61&pii=S2352340922000130&tid=spdf-b4c50e98-eaa3-4dcc-a864-052a359f6d87&sid=0543106a35df274abe784e25dc52280bea17gxrqa&type=client&tsoh=d3d3LnNjaWVuY2VkaXJlY3QuY29t&ua=0b0d5d555403525b00&rr=842b45cf7cfbf490&cc=in) ".


*You can find the[subset of data we're working with here](https://drive.google.com/drive/folders/1MeyLak24sJuZpgKK8I8ph8dI17Po-4H5) .*


## Setting up the viewer for CTA


Please[visit our documentation](https://docs.redbrickai.com/annotation-and-viewer/viewer-basics) to understand the basics of our viewer.


### Set up windowing using hanging protocols


When you first open up a case, you'll notice the windowing settings may not be optimal for viewing and annotating the aorta. Use the windowing shortcut` ctrl` +` left mouse` drag to adjust the windowing settings.


After you find your optimal window width & level, you can set it as the default for your project using[RedBrick AI's hanging protocols](https://docs.redbrickai.com/annotation/layout-and-multiple-volumes/custom-hanging-protocol) . Hanging protocols allows you to pre-configure your viewer with custom scripts; to set windowing, we will use the` setWindowing` function.


## Segmenting the aortic tree and arteries


### Region growing for aortic tree base segmentation


The aorta is a well-contrasted 3D structure. Therefore, the 3D region growing tool will perform well in segmenting the entire aorta. We'll first segment the entire aorta as a base segmentation and then use boolean operations to further classify parts of the aorta as iliac arteries.


### Cleaning up using 3D scissors, hole-fill, and more


Now that the base segmentation is done, we can perform basic cleanup by using:


1. **3D hole filling** to fill all small holes in the segmentation.
2. **3D scissors** on the 3D view to remove large chunks of overflowing segmentations.
3. **Island removal** to only keep the main segmentation structure.
4. **Dilation** (left-click to dilate, right-click to erode) increases our segmentation by 1 pixel in all directions.


### Overwrite aorta using boolean operations & masking


Next, we'll modify the Masking Panel to overwrite only a portion of the aorta segmentation as common, internal, and external iliac arteries.


1. **Unlock** the base aorta segmentation to allow it to be overwritten.
2. **Create a new common iliac** segmentation entity.
3. Adjust the **masking panel** to make Editable area: Inside all segments, and Modify other segments: Overwrite unlocked segments.
4. Use the **pen tool in the 3D** view to overwrite the aorta segmentation with common iliac.
