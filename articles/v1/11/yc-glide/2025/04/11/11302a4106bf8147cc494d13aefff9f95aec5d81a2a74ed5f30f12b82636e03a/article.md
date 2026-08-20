---
schema_version: "1.0.0"
document_id: "11302a4106bf8147cc494d13aefff9f95aec5d81a2a74ed5f30f12b82636e03a"
company_key: "yc-glide"
company: "Glide"
source_id: "yc-glide-news-import-b50025bbdd3a"
canonical_url: "https://www.glideapps.com/blog/claude-ai-app-glide-anthropic-api"
published_at: "2025-04-14T12:00:00+00:00"
first_seen_at: "2026-07-21T21:51:19.848750+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:7c716edb0196d05f5cb49189ddd858af997f4476e0854e94830e288aa7f58f78"
---

# Building a custom Claude AI app with Glide and the Anthropic API

Depending on who you ask, Anthropic’s[Claude](https://www.anthropic.com/claude) is *the* top performer among all LLMs.


For most people, interacting with Claude means using the[Claude website](https://claude.ai/) or mobile apps.


As great as those tools are, though, the real power of Claude lies behind the[Anthropic API](https://www.anthropic.com/api) .


The Anthropic API is what gives software developers more control over Claude’s capabilities, and it's how they integrate Claude into their own projects.


But what if you’re not a developer? Working with the Anthropic API can seem a little daunting. There’s a lot of technical jargon and code spread throughout their documentation.


Where do you even begin?


With Glide, you can build your own custom apps on top of the Anthropic API without having to touch any code. Our all-in-one platform gives you everything you need.


Here’s an example: a Glide app that lets your team hold group chats with Claude, and lets you create “Claude Presets” – a sort of custom persona for Claude, inspired by OpenAI’s custom GPTs.


Let's look at how it works.


## Using our Claude app


Our Claude app is a Glide app, which means it runs in the web browser and adapts to any device.


### Managing Claude Presets


A “Claude Preset” is the name we’re using for a preconfigured Anthropic API connection. The app’s administrators can create and manage Claude Presets for their team to use.


Preset options include Anthropic API parameters like model version, temperature, and the system prompt. The system prompt is what lets us give Claude a “persona”.


### Chatting with Claude


The home screen of our Glide app lets users start a new chat with Claude, or jump into an existing chat. When users start a new chat, they can choose from the available Claude Presets. They’re then taken to the message screen to begin their conversation.


Claude’s behavior is based on the specified Claude Preset. If the Preset configuration changes, Claude’s behavior in the chat will change, too.


Multiple users can join a Claude chat, and Claude will keep track of the entire conversation history.


### Sharing messages


Users can take action on individual messages within a Claude chat. They can copy the text of a message to their clipboard, or they can email a copy of the message to themselves.


## Building our Claude app


Our custom Claude app is surprisingly lean under the hood. We’re using a few data tables; a few layout screens; and a few interaction-triggered workflows.


### Data


Every Glide app starts with data. For this example we’re using our native[Glide Tables](https://www.glideapps.com/docs/glide-tables) .


#### Presets table


When administrators create a new Claude Preset, they’re adding a row (record) to the Preset table.


Each row contains the Preset ID, label, and description; the Anthropic API headers and body parameters; details about the admin user who created the Preset; and a timestamp.


We also use[Relations](https://www.glideapps.com/docs/relations) ,[If-Then-Else](https://www.glideapps.com/docs/automation/computed-columns/if-then-else-column) , and[Lookup](https://www.glideapps.com/docs/lookup-column) columns to check if a Preset is in use. We reference these columns on the Presets screen so admins can find related Claude chats.


#### Chats table


When a user starts a new conversation with Claude, they’re adding a row to the Chat table.


Each row includes a unique[Row ID](https://www.glideapps.com/docs/row-id-column) ; the ID of the user who started the chat; the ID of their chosen Claude Preset; the compiled message history; and a timestamp.


#### Messages table


This is where most of the action happens in our data.


Every message in a Claude chat gets added to the Messages table.


We use a variety of[Computed Columns](https://www.glideapps.com/docs/automation/computed-columns) to extract the information we need for our[Layouts](https://www.glideapps.com/docs/getting-started/intro-to-layout) and[Workflows](https://www.glideapps.com/docs/automation/workflows) , like the Claude Preset details, user details, and message history.


The Messages table is also where we store Claude responses from the Anthropic API. This ensures that all messages are stored in the same place, regardless of where they came from.


This lets us break apart the API responses so they’re usable in our Layout.


### Layout


[Layout screens](https://www.glideapps.com/docs/intro-to-layout) are made of drag-and-drop[components](https://www.glideapps.com/docs/essentials/components) that reference our app’s data.


#### Presets screen


The Presets screen is where app administrators create and manage Claude Presets.


We use a[Table component](https://www.glideapps.com/docs/essentials/components/collections/table) to list the existing Presets.


When an administrator creates a new preset, we display an[Overlay screen](https://www.glideapps.com/docs/screens#overlay) to enter the Preset name and description. When that’s submitted, they’re taken to[the Edit screen](https://www.glideapps.com/docs/automation/actions/show-edit-screen) finish setup.


#### Chats screen


The Chats screen serves as our app’s home screen.


We use a[Form Container component](https://www.glideapps.com/docs/forms#form-container-component) at the top for users to start a new Claude chat. When they hit Submit, they’re taken to the Messages screen for the chat thread. (More on that below.)


Beneath the form, we have a[List component](https://www.glideapps.com/docs/essentials/components/collections/list) showing all active Claude chats.


Administrators have the added ability to archive these chats so they’re hidden from regular users. This is hidden behind a[Visibility Condition](https://www.glideapps.com/docs/reference/user-experience/visibility-conditions) .


At the bottom of the Chats screen is another List component showing the available Claude Presets. It includes the Preset label, description, and the system prompt.


If an administrator changes any of these details on the Presets screen, this list will be automatically updated.


#### Messages screen


The Messages screen is actually a[detail screen](https://www.glideapps.com/docs/automation/actions/show-detail-screen) for a single Chat row (record).


We use a[Hint component](https://www.glideapps.com/docs/essentials/components/hint) to remind users to start the conversation, and set a Visibility Condition so that the component disappears when the chat has any message history.


For the conversation UI, we use Glide’s native[Comments](https://www.glideapps.com/docs/essentials/components/collections/comments) component with a Chat style, and set the data source to our Messages table.


We map the component properties to the corresponding column, and apply a[data filter](https://www.glideapps.com/docs/reference/user-experience/filter) to only show messages that belong to the active chat.


### Workflows


[Workflows](https://www.glideapps.com/docs/automation/workflows) add more functionality to your Glide app. They can be scheduled or triggered. For our example Claude app, there are three types of Workflows providing the bulk of functionality.


#### New Chat and New Preset


New Chat is triggered when a user starts a new chat on the Chat screen. Likewise, New Preset is triggered when an administrator submits the New Preset form on the Presets screen.


These Workflows let us chain multiple[Actions](https://www.glideapps.com/docs/automation/actions) together. We add more data to the row, show a confirmation notification to the user, and then redirect them to the appropriate screen.


#### Message actions


For an individual message, users can[copy the text to their clipboard](https://www.glideapps.com/docs/automation/actions/copy-to-clipboard) , or[send it to their email](https://www.glideapps.com/docs/automation/actions/send-email) .


These are just a couple of the many available Actions you can build your workflows around.


#### Add Message


Add Message is triggered when a user sends a new message on the Messages screen.


It’s the meatiest of the Workflows, pulling all of the pieces together from our Messages table.


Breaking down the steps:


**1. The**[Call API](https://www.glideapps.com/docs/automation/integrations/call-api) **action makes a POST request to the Anthropic API**


The request body pulls from a[Template column](https://www.glideapps.com/docs/template-column) , which in turn references other columns in the Messages table row. This makes the API call contextual to the chat it’s being used in.


The API response is stored in the same row as the message that triggered the API call.


**2.**[Wait For Condition](https://www.glideapps.com/docs/automation/actions/wait-wait-for-condition) **checks for an API response**


If the response takes longer than the specified time, we display an error message.


**3. We**[add a row](https://www.glideapps.com/docs/automation/actions/add-row) **to the Messages table**


This new row only includes the API response from Claude.


**4. We use**[Set Column Values](https://www.glideapps.com/docs/automation/actions/set-column-values) **to update the timestamp in the Chats table**


We access the Chats table through the Chat ID relation column in our Messages table.


### Settings


[App Settings](https://www.glideapps.com/docs/getting-started/app-settings) include all the global options for your Glide app.


#### Appearance


We're using orange as the accent color so it's reminiscent of the usual Claude experience.


Since there aren't many screens in this app, we're using a Top navigation layout.


The Content Width is set to Medium to strike a balance between the minimalist user interface and the denser administrator interface.


#### Access


Since this app is intended for teams, we set it to Private so only logged-in users can have access, and we allow anyone with our company email address to sign in. To make it even easier, we've enabled Sign in with Google.


## Putting it all together


We've built a custom Claude app with Glide and the Anthropic API. Our app users can have group chats with Claude, and app administrators can define unique Claude Presets for their team to use.


Our Glide build involved:


- Creating a relational database with Glide Tables
- Extending data with Computed Columns
- Building an adaptive interface with dynamic data
- Integrating Claude via the Anthropic API


…all without touching a single line of code.


Are you building something with Glide and Claude?[Tell us about it in the Glide Community.](https://community.glideapps.com/c/app-showcase/14)


Want to learn more about working with APIs?


[Check out our Intro to APIs guide](https://www.glideapps.com/blog/introduction-to-apis)


Create a Glide app in five minutes, for free.


[Sign Up](https://os.glideapps.dev/?utm_source=blog&utm_campaign=Building+a+custom+Claude+AI+app+with+Glide+and+the+Anthropic+API)


[Andy Claremont](https://www.glideapps.com/blog/author/andy-claremont)


[Andy Claremont](https://linkedin.com/in/andymci) leads ecosystem and community at Glide. His no-code journey started with WYSIWYG editors in the 90’s and he's been building ever since.


### Related Articles


[AI engineering is the next wave after no-code: Here is how business leaders can get started](https://www.glideapps.com/blog/no-code-to-ai)[Gideon Lahav](https://www.glideapps.com/blog/author/gideon-lahav) 28 Jul 2026


[How to build custom software for e-commerce operations with AI](https://www.glideapps.com/blog/build-ecommerce-software)[Wren Noble](https://www.glideapps.com/blog/author/wren-noble) 27 Jul 2026


[What is AI citizen development? A guide for 2026](https://www.glideapps.com/blog/ai-citizen-development)[Wren Noble](https://www.glideapps.com/blog/author/wren-noble) 24 Jul 2026
