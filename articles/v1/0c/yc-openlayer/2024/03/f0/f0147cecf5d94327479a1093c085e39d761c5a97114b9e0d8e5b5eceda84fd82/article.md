---
schema_version: "1.0.0"
document_id: "f0147cecf5d94327479a1093c085e39d761c5a97114b9e0d8e5b5eceda84fd82"
company_key: "yc-openlayer"
company: "Openlayer"
source_id: "yc-openlayer-news-import-df137f62af3c"
canonical_url: "https://www.openlayer.com/changelog/trace-every-step-of-your-requests"
published_at: "2024-03-28T00:00:00+00:00"
first_seen_at: "2026-07-25T18:01:08.657570+00:00"
fetched_at: "2026-07-28T22:26:05.625622+00:00"
content_hash: "sha256:89e874301e2d095e702efd47c942cd6be7cc8b34aa80faffcb0b5a2f552d6930"
---

# Trace every step of your requests

March 28, 2024


[Trace every step of your requests](https://www.openlayer.com/changelog/trace-every-step-of-your-requests)


We’re thrilled to share with you the latest update to Openlayer: comprehensive tracing capabilities and enhanced request streaming with function calling support.


Now, you can trace every step of a request to gain detailed insights in Openlayer. This granular view helps you to debug and optimize performance.


Additionally, we’ve expanded our request streaming capabilities to include support for function calling. This means that requests you stream to Openlayer are no longer a black box, giving you improved control and flexibility.


## Features


- •


Observability


Tracing (Trace every step of a request and view details, including latency and fun, Support for function calling in request streaming)ction inputs & outputs, in the UI,
- •


Integrations


Added support for using Azure OpenAI models


## Improvements


- •


Performance


Improved performance of the UI, including several networking optimizations
- •


UI/UX


Toggle button color improvements to make it easier to understand which is selected
- •


UI/UX


Improvement to color of background behind modals
- •


UI/UX


Column A mean / sum etc. grouped by column B values
- •


UI/UX


Surface generated question for answer relevancy metric
- •


UI/UX


Easily duplicate/fork test configurations
- •


UI/UX


Enable creating more tests without dismissing modal
- •


UI/UX


Improved design of request panel
- •


UI/UX


Warning displays in request pane when no prompt has been added
- •


Project dashboard


Request panel can be closed with the Esc key
- •


UI/UX


Navigate through requests in the panel by using the arrow keys
- •


UI/UX


Improved design of prompt roles in prompt blocks
- •


UI/UX


Ability to copy values of blocks and columns in request page
- •


Templates


RAG tracing example added to Openlayer examples gallery
- •


Templates


Azure GPT example added to Openlayer examples gallery
- •


Performance


Performance improvement: only automatically load inference pipelines and project versions if the user is in the relevant mode
- •


UI/UX


Remove Intercom app which was not utilized and was blocking core UI components
- •


UI/UX


Navigation callout components now have dark-mode purple styling
- •


UI/UX


Update notification page titles in settings
- •


UI/UX


Improvements and bug fixes for rendering content and metadata in selected row pane
- •


UI/UX


Updated copy icon
- •


UI/UX


Updated inconsistent delete icons throughout the app
- •


UI/UX


Render inputs in row panel even when no prompt is available
- •


UI/UX


Render metric scores and explanation columns further left in tables so they are in view without scrolling
- •


UI/UX


Updated format of date strings
- •


UI/UX


Enabled ability to collapse sections in row panels
- •


UI/UX


Enabled ability to collapse chat history blocks


## Fixes


- •


Templates


In-app Google Colab links were incorrect
- •


UI/UX


Checkboxes for suggested tests were not default selected
- •


UI/UX


Graph in test modal rendered too short sometimes
- •


UI/UX


Prompt roles did not render correctly when set to an unknown value
- •


API


Handle cases where data contains non-utf8 codes
- •


UI/UX


Create test pages overflow before enabling scroll
- •


UI/UX


Test modal overflows page
- •


UI/UX


Boolean values would not render in request pane metadata
- •


UI/UX


Labels in request pane overflowed improperly with long content
- •


UI/UX


Tests rendered broken graphs when all results were skipped
- •


Platform


Inference pipelines did not automatically load
- •


Platform


Inference pipelines did not automatically update tests or requests
- •


Platform


Commits did not automatically load nor update tests once processed
- •


API


Projects did not automatically appear when added from API
- •


SDKs


API key and project name were not auto-filling in TypeScript code snippet for starting monitoring
- •


UI/UX


Clicking to browse a commit always went to monitoring mode
- •


UI/UX


Monitoring test graphs did not show hovered results on initial load until refreshing
- •


UI/UX


Opening requests page showed no data until refreshed
- •


Platform


Column drift test wouldn’t run on non-feature columns
- •


UI/UX


Timeline page showed monitoring tests
- •


UI/UX


Checkboxes for suggested tests did not check properly on click
- •


UI/UX


Multiple copies of tests got created on successive clicks
- •


UI/UX


Unselected tests got created, and not all selected tests got created
- •


Performance


Tests loaded for too long when skipped or unavailable
- •


UI/UX


Copy button rendered twice in code labels
- •


UI/UX


Chat history input in row panels sometimes showed text editor
