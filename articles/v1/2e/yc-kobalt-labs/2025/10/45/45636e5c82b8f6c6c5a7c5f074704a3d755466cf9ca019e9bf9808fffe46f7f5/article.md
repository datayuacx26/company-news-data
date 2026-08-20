---
schema_version: "1.0.0"
document_id: "45636e5c82b8f6c6c5a7c5f074704a3d755466cf9ca019e9bf9808fffe46f7f5"
company_key: "yc-kobalt-labs"
company: "Kobalt Labs"
source_id: "yc-kobalt-labs-rss-c4f45a5ecda6"
canonical_url: "https://blog.kobaltlabs.com/echolocating-through-the-agi-reality-distortion-field-f4ccd2e12ffe"
published_at: "2025-10-09T17:02:20+00:00"
first_seen_at: "2026-07-20T23:21:08.518495+00:00"
fetched_at: "2026-07-28T20:55:41.466450+00:00"
content_hash: "sha256:cca11724cf870b6926a9ebe7d97850a7b66e8a5e4c26e0ec0585a47ab349210f"
---

# Echolocating through the AGI Reality Distortion Field

# Echolocating through the AGI Reality Distortion Field


[Braedon Villano](https://braedontask.medium.com/?source=post_page---byline--f4ccd2e12ffe---------------------------------------)


6 min read


·


Oct 9, 2025


--


Press enter or click to view image in full size


100% accurate rendering of me delivering muffins to my neighbors in 2026.


“Hark, Hark! At ease humans! For now is the time to stow yesterday’s labor in hunt of more bountiful and soul-fulfilling follies; to cherish loved ones with your refunded time and replenish your humanity. Today is the dawn of AI and dare I say AGI is around the corner! All jobs are now solved. By all means, go learn to bake so you can bring muffins to the new robot family that is set to move in down the street.”


Thank you, OpenAI, Anthropic, but can you help me with one small thing:


```text
Error code: 413 –   {    'error':       {        'type': 'request_too_large',        'message': 'Request exceeds the maximum size'      }  }
```


Why for the last two weeks have I been unable to understand why the request I’m sending to your model is “Too Large” and despite several hours searching am I rendered equally unable to find the true size limit anywhere in your documentation? Or, is this just a personality quirk of the AGI I’ve been assigned like how some humans don’t like green eggs?


## The Problem Statement


For a few weeks here at Kobalt Labs we had been plagued by an opaque “Request Too Large” exception on calls to Anthropic’s messages create endpoint. This error was a mounting reliability issue for our core services affecting several customers each week.


These particular requests were chat messages with Base64 encoded PDF files attached for the model to use as context. We looked for documentation on the error on Anthropic resources but there remained a core question:


What request size is too large? 1MB? 1000MB?


So, I decided to binary search the Anthropic messages create endpoint; fuzzing payload sizes as necessary. This is a method seen a lot in reverse engineering and hacking (sometimes called a[side-channel attack](https://en.wikipedia.org/wiki/Side-channel_attack#:~:text=Even%20though%20multiply%20operations%20draw,also%20immune%20to%20timing%20attacks) ).


I simply needed to construct valid PDF requests of arbitrary size and probe the endpoint for failures until I found the maximum size.


## **Echolocating with Bits and Bytes**


I’ll go through the steps here and include some screenshots along the way but if you want to just see for yourself I put all of the code in a GitHub Gist[here](https://gist.github.com/braedontask/efb08c3b871ccef23249f6024d2b5dd8) for public access. I should note that Cursor had a rough time one-shotting this experiment so much of it I wrote myself without my AI training wheels! Go me!


The path here is clear: we need to send requests to the Anthropic endpoint with various request sizes, binary searching until we don’t get a “Request Too Large” exception.


You will see that the meat of this side-quest is actually making valid PDF requests of variable size and not the binary search implementation. I’ll save you the trouble of re-learning binary search for the 204th time and just drop the branch predicate here:


```text
async def test_request_size(    size_mb: float,    model_id: str = "claude-3-5-sonnet-20241022"  ) -> bool:      try:          messages, pdf_size = create_test_message(size_mb)                    import json          request_json = json.dumps(messages)          total_request_size = len(request_json.encode('utf-8'))                    print(              f"Target: {size_mb:.2f} MB, PDF: {pdf_size / (1024*1024):.2f} MB\n"              f"Total request: {total_request_size / (1024*1024):.2f} MB"          )                    _ = await test_claude_client.messages.create(              model=model_id,              max_tokens=100,              messages=messages,              temperature=0.0,          )                    print(f"  -> ACCEPTED")          return True                except Exception as e:          error_msg = str(e).lower()          if (              "too large" in error_msg or              "size" in error_msg or              "payload" in error_msg          ):              print(f"  -> REJECTED (too large)")              return False          elif "rate limit" in error_msg:              print(f"  -> RATE LIMITED, retrying...")              await asyncio.sleep(1)              return await test_request_size(size_mb, model_id)          else:              print(f"  -> ERROR: {e}")              return False
```


### **PDF vs Other Base64 Data**


Why not just use random Base64 data? I tried. It turns out there are validation checks that Anthropic does to Base64 data upon reception. The Base64 payload you send over has to decode to some expected file type on their end.


Images could also work for this experiment. I avoided them here because after running into other validation problems I wanted to narrow and control for our specific use case: PDFs.


### **Stuffing PDFs with Images**


Why not just send in PDFs with a million pages? Once again, I tried. First, it turns out that PDFs can actually have so many pages with text before you start to make a dent in a single MB. And second, when I sent PDFs with >1000 pages to Anthropic the exception I got was a` 529 overloaded_error` . lol.


## Get Braedon Villano’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


So, I decided to put in 1MB images on each page using` fitz` .


```text
def generate_pdf_data(size_mb: float) -> tuple[str, int]:      doc = fitz.open()      current_size = 0      iterations = 0      target_bytes = size_mb * 1024 * 1024            while current_size < target_bytes:          image_bytes = generate_uncompressible_image()                    page = doc.new_page()          page.insert_image(fitz.Rect(0, 0, 1000, 750), stream=image_bytes)                    current_size = len(doc.write())          iterations += 1                pdf_bytes = doc.write()      base64_data = base64.b64encode(pdf_bytes).decode('utf-8')      base64_size = len(base64_data.encode('utf-8'))            print(f"Added {iterations} images")      print(f"Final PDF size: {len(pdf_bytes) / (1024*1024):.2f} MB")      print(f"Base64 encoded size: {base64_size / (1024*1024):.2f} MB")            return base64_data, len(pdf_bytes)
```


### **Avoiding PDF Compression**


At first I just downloaded some stock 1MB images from the internet. That was naive. I’m quite ready to say that I fully underestimated the power of PDF compression in 2025. When I say no matter how many pages of 1MB images I added, the PDF never climbed above 1.5MB… I mean it. I guess it’s pretty obvious in hindsight.


To get around this I used` numpy` to generate pixel matrices of 1024 x 1024 values with random pixel values. I then could convert these pixel matrices to images using the` PIL` library.


```text
def generate_uncompressible_image(target_size_bytes=1024 * 1024) -> bytes:      width, height = 256, 256      mode = "RGB"       while True:          arr = np.random.randint(0, 256, (height, width, 3), dtype=np.uint8)          img = Image.fromarray(arr, mode)           img_bytes = io.BytesIO()          img.save(img_bytes, format="PNG", compress_level=0)          img_bytes.seek(0)           size = len(img_bytes.getvalue())          if size >= target_size_bytes:              return img_bytes.getvalue()           width += 16          height += 16
```


### **Getting an Answer**


Finally, I had what I needed: ability to make arbitrary sized PDFs at 1MB denominations and a binary search built for understanding Anthropic’s request error codes.


```text
Testing 23.05 MB...  Added 23 images  Final PDF size: 23.07 MB  Base64 encoded size: 30.76 MB   Total request: 30.76 MB      -> ACCEPTED      -> ACCEPTED: 30.76 MB works
```


30.76MB. Ah, the sweet sweet pleasure of over-engineering. Scoreline: 1 Braedon to 326 Anthropic. I’m still in the game!


### **Fixing Prod**


With the constraint identified, we took this moment to properly install file size validation:


1. ****Data Model Updates**** : Added a` size_bytes` column to file models, backfilled using S3 metadata


2. ****Request Validation**** : Check combined file sizes before sending requests to Anthropic


3. ****UI Constraints**** : Added file size validation to upload UI flows


If you haven’t set up file size constraints in your product I would consider it. I’ve dodged some long-tail backend work using them.


## **A Note to Engineers**


Being an engineer right now in the AI revolution is an uncomfortable place. We are the closest to ground zero, the first to feel our once stable foundation shifting and breaking beneath us like tectonic plates, the first to be disrupted… I have felt a number of times a smallness building next to the new generation of intelligence that one-shots much of what I used to offer the world in a workday. Even on the startup front there is new dogma: “Everyone is an engineer” or “Startup is now just sales”.


It is the small problems and solution finding like the one outlined here that should inspire some confidence. The CS skillset is not yet dead or useless. Be reminded that the narrative being broadcast for the AI boom is not yet reality. AGI is not here but needs to be heavily marketed to fuel consumer appetite and buy-in for the future to come.


This is not Kobalt’s first run-in with problems of this variety. I’ve recently had a row with the OpenAI vector store (perhaps a problem for another time). Although it seems trivial, keep in mind that a non-engineer would have struggled much longer here.


In an explosion of vision perhaps the people to bet on are still those that can see the land shift beneath them and maintain their footing.
