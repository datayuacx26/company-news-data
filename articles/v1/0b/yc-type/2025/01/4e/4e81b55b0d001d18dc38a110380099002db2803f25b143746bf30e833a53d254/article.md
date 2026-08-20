---
schema_version: "1.0.0"
document_id: "4e81b55b0d001d18dc38a110380099002db2803f25b143746bf30e833a53d254"
company_key: "yc-type"
company: "Type"
source_id: "yc-type-news-import-4fd34dd6ac78"
canonical_url: "https://blog.type.ai/post/type-ai-overview"
published_at: "2025-01-31T00:00:00+00:00"
first_seen_at: "2026-07-24T05:16:16.827244+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:c0c3b1a23c0ece4b3335a0e56f30fb1fe428cfd3ec606047c0acfdac0932be1d"
---

# Type Overview

## **What is Type?**


[Type](https://type.ai/) is a new kind of document editor. It embeds powerful AI writing and editing features inside of a fast, performant editor. It's built for writers who need greater control and precision in their work.


## **Getting started: navigating the document editor**


### **The document**


The left side of Type contains your document. This is where you compose and edit your writing.


#### Blocks


Each piece of content in the editor is contained in a block.


Blocks are just individual pieces of content, such as body text, headers, images, code, and tables. Your document is the sum total of its blocks.


You can create your first block in an empty document by just typing. When you do, you'll have created a text block.


You can change any type of block into another by tapping the floating arrow menu to the right of a block and selecting "Change block type."


**Pro tip:** You can also use markdown shortcuts at the beginning of a new block to create headers or list items.


There are a few different types of blocks in Type:


- **Headers** : come in three different header sizes
- **Text** : basic body text
- **Code** : format your code snippet in over 100 programming languages, including Python, Javascript, and more
- **Math** : format a math formula; use brackets, curly brackets/braces, and other[LaTeX punctuation](https://en.wikibooks.org/wiki/LaTeX/Mathematics) to write your formulas
- **Quote** : give a statement extra oomph
- **Tables:** organize information in rows and columns


To create a new block, just press the Enter key and create a new line in your document.


### **The sidebar**


On the right side of the app, you’ll see the sidebar.


The sidebar contains a number of writing and editing features, including Type Chat, content ideas, and Reviews.


You can adjust the width of the sidebar by dragging the border between it and the document.


You can also dismiss it for a more focused writing experience – just tap the sidebar icon in the top right-hand corner.


### **The toolbar**


At the top of your document is the toolbar. In the toolbar, you'll find options for formatting your document and blocks.


You can access more advanced settings and features by tapping the` ...` menu.


## **Core features: how to write in Type**


You can just start typing in an empty document if you'd prefer to write something from scratch yourself.


Alternatively, you can import a document into Type's editor or have the AI generate a draft.


### **Importing a file or URL**


You can import a file or URL you'd like to edit inside of Type by tapping the "Import doc" button in an empty document.


Type currently supports importing URLs, PDFs, Word documents, and images that contain text.


Type will do its best to maintain the formatting and content in your original file or URL, but always be sure to double-check the imported content for completeness.


You can always copy and paste content into the editor, too. Type will always try to keep all of your formatting, images, and content intact when you do.


### **Generating a draft**


Type can generate entire documents that contain your knowledge and follow a particular style template you provide.


You can open Generate Draft by tapping on the Generate Draft button in the middle of an empty document.


When you open Generate Draft, you'll be asked to provide a description of what you're writing. You can also (optionally) provide knowledge attachments like PDFs, word documents, images, or Type documents that contain useful information Type should be aware of.


And you can attach a template, which Type will attempt to guide the general format and voice of the draft it generates. If you write a weekly newsletter, for example, you might attach last week's newsletter as a template so Type learns your preferred layout and voice.


You can save custom prompts inside Generate Draft by tapping the` +` icon in the prompt field. Generate Draft will also save your prompt history automatically.


You can access both your saved prompts and history by tapping the dropdown icon in the prompt field.


### **Type Chat**


Type Chat is a chat-based writing assistant that integrates directly with your document.


Type Chat understands nuances in conversation and can interpret complex instructions, so you can talk to it just like you would another person. It’s also aware of what’s inside your document and can provide help brainstorming, writing, and editing as you work on a draft.


Open Chat by selecting the sidebar icon next to the **Publish** button. Make sure the panel has “Type chat” at the top, and the **speech bubble icon** is indicated.


Chat is useful at any point in the writing process. For example, if you don’t know where to start, you can tell Type Chat to ask you a few[questions](https://blog.type.ai/post/non-obvious-prompting-advice-for-writers) to help you refine your starting point. On the other hand, when you’re done writing, you can ask Type to give you feedback on your work with a certain goal in mind, such as optimizing for sign-ups.


If you ask Type to generate text to include in your document, it will place its output in a block that you can drag and drop into your document. Just tap the six dots in the top left hand corner of the block and drag it into your document.


Alternatively, you can tap the “Replace selected text in editor” to insert the text at your current cursor position. Or copy the text to your clipboard by tapping “Copy to clipboard.”


You can also use Chat to edit and refine text that’s already in your document. When you highlight text in your document, the selected text will appear in Type Chat. You can ask the chatbot to edit your work, no matter the block type. This means you can even ask Type to help out with writing or reviewing Code or Math blocks.


Highlighting text adds to Type Chat’s context window. A context window refers to the set amount of text or characters the model can interpret when generating an output.


Note that Type’s context window is roughly 128k tokens (about 500k characters or 110k words) if you’re on GPT, and 200k tokens (about 800k characters or 180k words) if you’re using Claude.


For longer documents, Type Chat works best when you highlight and edit smaller chunks of text one at a time, rather than trying to revise thousands of words at once.


To ensure Type Chat has all of the context it needs to provide useful assistance, you can give it access to your document content by tapping the document icon in the message field.


When you enable document access, Chat will take your entire document into context when it generates its responses. This means you can ask it for help across any part of your document without needing to highlight it. Turning off document access means Chat won’t have the current content of your document in memory, but it will respond to your messages more quickly.


Highlight text and ask Type Chat to transform it in any way you need. Type will generate output that fits in the location of the selected text.


### **Content Ideas**


Type can make relevant suggestions on what to write about next based on what’s in your document. These ideas grow and evolve as your document changes.


To view Type’s content ideas in the sidebar, tap on the lightbulb icon. Scroll through Type’s suggestions. If you don’t see any you like, you can either ask it to refresh by selecting the cycle icon in the upper right corner or "More suggestions" at the end of the suggestions list.


Once you find an interesting idea, Type can write it for you. Tap the three dots to reveal the "Write it for me" button. Type’s AI will write a new section wherever you last placed your cursor.


### **The command palette**


You can summon AI writing help at your cursor as you write in Type – just press the` /` key anywhere in your document and Type's command palette will open.


The command palette contains a number of AI writing features that can generate text directly at your cursor:


- **Generate content** : create custom instructions to receive more specific output; it can produce up to 1,000 words, but defaults to a few hundred, depending on your prompt
- **Write sentence** : finish an incomplete sentence or add a new one
- **Write paragraph** : add a new paragraph or finish an incomplete sentence with a few more sentences
- **Write list** : create a bulleted list or add 3-5 new items to an existing one
- **Continue writing** : ask Type to continue writing with free form text based on what’s already in the document
- **Generate section headline** : inserts a header that describes the content below it
- **Generate document headline** : creates an H1 header at the top of your document


### **Rewriting text inline**


Rewriting with AI in Type takes just a few clicks.


When you highlight text in the document editor, a floating toolbar automatically appears. Open the **AI dropdown menu** or **use slash (“/”)** to reveal Type’s rewrite menu. You can either provide your own custom or saved rewrite instructions, or use a pre-built “Brush.”


A Brush is a shortcut for common editing tasks. Here’s what they do.


- **Improve** : Enhances the overall quality of your text by refining word choice, improving sentence structure, and ensuring clarity, all while maintaining your original meaning and tone. **‍**
- **Shorten** : Reduces the length of your text while maintaining its core message, making it more concise and to the point. **‍**
- **Lengthen** : Expands your text by adding relevant details and elaboration to ensure comprehensive coverage of the topic. **‍**
- **Fix grammar** : Corrects grammatical errors in your text, ensuring that it adheres to proper grammar rules and standards **‍**
- **Simplify language** : Makes your text easier to understand by using simpler words and sentence structures **‍**
- **Enhance readability** : Separates chunkier text into smaller paragraphs and uses formatting such as bullet points or numbered lists to keep content interesting


When you select a Brush, the change applies to all the highlighted text you’ve selected but uses the entire context of the document when generating its revision.


After you run a transformation, Type makes suggestions throughout your selected text. New and rewritten content is represented by green text, while red text indicates deletions.


You can review these suggestions individually by hovering over the colored text or using the **Shift-Command-. (period)** keyboard shortcut. To accept a suggestion, press **A** . To reject a suggestion, press **R.** Once you've accepted or rejected an edit, Type automatically moves you to the next suggestion to review.


At any point, you can accept or dismiss the remaining suggestions. To accept all, press **Command-Enter** . To dismiss all, press **the Escape key** . Dismissing the remaining suggestions does not undo any edits you've made.


If none of these shortcuts give you exactly what you need, you can save your own custom rewrite instructions as a custom Brush for easy reuse. Just select **Custom Edit** to add a new transformation. Once you save your own, they’ll always be easily accessible in your **Saved Prompts** .


You can run an edit command multiple times on the same text to get different results by selecting **Retry transform** .


When selecting text to rewrite, it’s important to consider how much or little to include. Though most Brushes will automatically take your entire document into account, the Type’s AI will pay special attention to the text you highlight.


For best results, it’s generally better to include more rather than less, but also keep in mind that selecting large amounts of text (especially the entire document) may slow down generation times or produce more changes than you may want. We recommend breaking up revision work into independent chunks of text that are directly related (ex. An entire subsection within a blog post).


### Reviews


You can review your entire document for errors, opportunities for improvement, or any other changes using the Reviews feature in the sidebar.


Reviews process your entire document and provide suggested edits that you can accept or reject.


You can start a review by tapping the Reviews icon in the sidebar and selecting a Review to run.


After you run a Review, you can accept or reject Type's suggested edits by hovering over them in your document or tapping "Accept" or "Dismiss" in the sidebar.


### **AI models, modes, and rate limits**


You can write with both Claude and GPT in Type. We go into more about their differences, but here are the 3 most important things you should know:


1. Claude is more creative, while GPT tends to be more analytical and factual.
2. Claude has a bigger context window, which means it can process longer inputs and generate longer outputs.
3. However, GPT can write in 200+ languages.


When you write, you use both sides of your brain—the artistic left side and the technical right side. Writing with AI is sort of like that, but if each half of your brain was a super processor trained on massive amounts of data.


Change the AI model you’re using in the toolbar when you’re in the document editor. Try alternating between the models while you’re writing!


Type offers two AI modes to help you write: Speed and Power.


**In Speed mode, your selected AI will respond and generate almost instantly, but the output quality may be lower.** Today, these include OpenAI's GPT-3.5 Turbo and Anthropic's Claude Haiku.


**In Power mode, your selected AI may take a moment longer to respond, but it can handle more complex prompts and generate higher quality outputs.** Today, these include OpenAI's GPT-4o and Anthropic's Claude 3.5 Sonnet.


Select the lightning bolt icon to toggle between Speed and Power mode.


Note that there users are subject to some basic rate limiting while leveraging the AI models in Power mode. Setting rate limits is a[common](https://docs.anthropic.com/en/api/rate-limits)[practice](https://platform.openai.com/docs/guides/rate-limits) for APIs, as they protect the service, its infrastructure, and other users.


It’s unlikely you’ll reach these limits. However if you do, these limits reset roughly every 30 days alongside your billing cycle automatically. We also offer one-time resets if you email “limit reset” tohello@type.ai .


There are no limits while in Speed mode, which leverages faster models like GPT-3.5-Turbo and Claude Instant.


You can also update your preferred model and mode in your settings.


### Keyboard shortcuts


Type comes loaded with keyboard shortcuts to help access key writing and editing features faster.


If you’re on a[Windows (PC) computer](https://support.microsoft.com/en-us/topic/keyboard-mappings-using-a-pc-keyboard-on-a-macintosh-d4fd87ca-8762-30ee-fcde-08ffe95faea3) , you’ll need to use your Control (CTRL) or Windows key, depending on your settings.


General:


- **Show keyboard shortcuts:** Control-H
- **Toggle power mode:** Command-Shift-P
- **Toggle AI model** : Command-Shift-M
- **Undo** : Command-Z
- **Redo** : Command-Shift-Z


AI editing:


- **Next suggestion** : Shift-Command-. (Period)
- **Previous suggestion** : Shift-Command-, (Comma)
- **Accept suggestion** : A
- **Reject suggestion** : R
- **Accept all** : Command-Enter **‍**
- **Dismiss all** : Escape key


AI generation:


- **Open Command Palette:** Command-K or slash (/) **‍**
- **Generate content** : Command-Semicolon (;) **‍**
- **Write sentence** : Command-Period (.) **‍**
- **Write paragraph** : Command-Slash (/) **‍**
- **Continue writing** : Option-Command-Slash (/) **‍**
- **Generate section headline** : Command-Shift-U **‍**
- **Generate document headline** : Command-Shift-Y


Block controls:


- **Change block type:** Control-T
- **Create block below current** : Control-Enter
- **Create block above current** : Control-Shift-Enter
- **Move current block down** : Control-Down Arrow
- **Move current block up** : Control-Up Arrow
- **Turn into header 1:** Option-Command-1
- **Turn into header 2:** Option-Command-2
- **Turn into header 3:** Option-Command-3
- **Turn into paragraph:** Option-Command-4
- **Turn into code block:** Option-Command-5
- **Turn into math block:** Option-Command-6
- **Turn into quote:** Option-Command-7


Formatting:


- **Bold** : Command-B
- **Italics** : Command-I
- **Underline** : Command-U
- **Strikethrough** : Command-Y
- **Inline code** : Command-E
- **Inline math** : Command-Shift-B
- **Hyperlink** : Command-Shift-K


## **Final steps**


When you’ve finished writing your document, Type can help you with the final steps, whether that’s sharing privately or publishing to a wide audience.


### **Export**


Type offers several export options to ensure your work is easily shared and compatible with various platforms. To access the export feature, open the toolbar menu **\[...\]** and select **Export document.** Export your document in the following options:


1. **PDF** : Ideal for maintaining layout integrity across all devices.
2. **Word Document (.docx)** : Perfect for collaborative editing in Microsoft Word or Google Docs.
3. **Markdown (.md)** : Suitable for developers and those working within text-based environments.
4. **Plain Text (.txt)** : A simple format that strips away any formatting, useful for importing into other applications.


### **View-only URLs**


You can share your Type documents as view-only URLs. Generate a unique, private link that only those with access can view. The published version of your document will automatically update as you make any edits, and you can always unpublish links to make your work private again.


### **Publish on your own Type blog**


Preview this[sample blog](https://posts.type.ai/santa) and this[post](https://posts.type.ai/santa/i-love-cookies) live.


Publish your work as posts to your own Type blog. This Type feature lets you share your ideas with the world.


Like the private URLs, posts update live, and unpublish your posts to make them private again.


Your Type blog and its posts have unique URLs you can set. You can change your links, but keep in mind that doing so will break older links with the previous URL.


## **Additional Resources**


- [Prefer a video tutorial?](https://www.youtube.com/watch?v=YcYmqtYRXUM)
- [FAQs](https://blog.type.ai/faqs)
- [How to write better prompts](https://blog.type.ai/post/non-obvious-prompting-advice-for-writers)
- Contact the team:hello@type.ai
