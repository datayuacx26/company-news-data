---
schema_version: "1.0.0"
document_id: "8e9c97f95bec8022feaddcc6b1717da9cb91247723877064e25472583c7fc90e"
company_key: "yc-intuned"
company: "Intuned"
source_id: "yc-intuned-news-import-ba3d72b83236"
canonical_url: "https://intunedhq.com/blog/playwright-for-browser-automation"
published_at: "2026-03-25T00:00:00+00:00"
first_seen_at: "2026-07-25T09:50:45.873485+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:a6bbc39c2e35d0669e734929e24f2790a22afc6c761313d0adc5170f20326a1d"
---

# Playwright for Browser Automation

Intuned's runtime leverages Playwright, the open-source browser automation framework, for deterministic automation code. Projects contain APIs — handler functions that receive browser page and context objects plus custom parameters.


The handler function receives a Page object representing your browser tab and a BrowserContext that provides an isolated session (like incognito mode). This guide covers everything you need to know to write effective Playwright automations on Intuned.


## Basics


### Navigation


Use` goto()` to navigate to a URL. This is typically the first step in any automation.


```text
await   page  .  goto  (  'https://books.toscrape.com/'  )  ;
```


### Wait for Page Load


Three wait states are available:` load` (full page including images — the default),` domcontentloaded` (DOM ready, resources still pending), and` networkidle` (no network connections for 500ms+). Choose the right one depending on whether you need all resources loaded or just the DOM.


### Wait for URL


You can wait for the page URL to match an exact string, a glob pattern, or a regex before proceeding. This is useful when navigating through multi-step flows where the URL changes.


### Create New Pages


Use` context.newPage()` to create additional browser tabs and run parallel operations within the same browser context. This is helpful when you need to scrape multiple pages simultaneously.


### Handle New Tabs and Popups


Capture tabs opened via` target="_blank"` using` waitForEvent("page")` . For popup windows, use` waitForEvent("popup")` . Always set up the event listener before triggering the action that opens the new tab.


## Finding Elements


### Locators


Locators are the primary way to find elements in Playwright. They're lazy — not evaluated until you interact with them. This means you can define a locator once and reuse it throughout your script.


### CSS vs XPath


CSS selectors offer simpler syntax and slightly better performance. XPath handles complex DOM traversals better — for example, selecting a parent element based on a child's text. Use CSS when possible and fall back to XPath for complex cases.


### CSS Selector Extensions


Playwright extends CSS selectors with powerful pseudo-classes:` :has-text("text")` matches elements containing text anywhere,` :text("text")` finds the smallest element with that text,` :text-is("text")` does exact matching,` :visible` filters to only visible elements, and` :has(selector)` matches elements containing a specific child.


### Semantic Locators


Prefer role-based locators for more resilient automations.` getByRole()` finds elements by ARIA role,` getByText()` by visible text,` getByLabel()` for form fields,` getByPlaceholder()` for inputs,` getByAltText()` for images,` getByTitle()` by title attribute, and` getByTestId()` by data-testid attribute.


```text
// Semantic locators are more resilient to DOM changes
await   page  .  getByRole  (  "button"  ,     {   name  :     "Submit"     }  )  .  click  (  )  ;
await   page  .  getByLabel  (  "Email"  )  .  fill  (  " [email protected]  "  )  ;
await   page  .  getByPlaceholder  (  "Search..."  )  .  fill  (  "query"  )  ;
```


### Chaining and Filtering


Refine your locators with` .first()` and` .last()` for the first/last match,` .nth(index)` for a specific position,` .filter()` to add conditions, and` .and()` or` .or()` to combine locators. These let you precisely target elements even in complex DOMs.


### Checking Element State


Check element state with` isVisible()` ,` isHidden()` ,` isEnabled()` ,` isChecked()` , and` count()` . Important: Playwright operates in strict mode by default — if your locator matches zero or multiple elements, you'll get a strict mode violation error. Always ensure your locator uniquely identifies one element.


## Auto-Waiting and Timeouts


### What Playwright Waits For


Before performing actions, Playwright automatically waits for elements to be: attached to the DOM, visible (non-empty bounding box), stable (not animating), enabled, and able to receive events (not obscured by other elements). This eliminates most flaky test issues.


### Explicit Waits


When auto-waiting isn't enough, use` waitForSelector()` to wait for elements to appear and` waitForResponse()` to wait for specific network responses. Avoid` waitForTimeout()` — always prefer waiting for a specific condition instead of arbitrary delays.


### Configuring Timeouts


The default timeout is 30 seconds. Set a global default with` page.setDefaultTimeout(10000)` , or configure per-action:


```text
// Global timeout
page  .  setDefaultTimeout  (  10000  )  ;


// Per-action timeout
await   page  .  locator  (  "#slow-element"  )  .  click  (  {   timeout  :     60000     }  )  ;
```


## Extracting Data


### Text Content


Three methods for extracting text:` textContent()` returns all text including hidden content,` innerText()` returns only visible text, and` innerHTML()` gives you the raw HTML. Choose based on whether you need hidden text or just what the user sees.


### Attributes and Bulk Extraction


Extract HTML attributes like` href` ,` src` , and` data-*` values. For bulk extraction, use` .all()` to get all matching elements as an array, then iterate and extract data from each. You can also loop using` count()` and` nth(index)` for indexed access.


```text
// Extract all links from a page
const   links   =     await   page  .  locator  (  "a.product-link"  )  .  all  (  )  ;
const   data   =     [  ]  ;
for     (  const   link   of   links  )     {
data  .  push  (  {
text  :     await   link  .  textContent  (  )  ,
href  :     await   link  .  getAttribute  (  "href"  )  ,
}  )  ;
}
```


### Intuned SDK Helpers


Intuned provides additional helpers:` scrollToLoadContent` for infinite scroll pages,` clickUntilExhausted` to click a button until it disappears (great for "Load More" buttons), and` extractMarkdown` to extract page content as clean markdown.


## Performing Actions


### Clicking


Basic interactions include` click()` ,` dblclick()` , and` hover()` . If an element is obscured, you can use` click({ force: true })` to bypass actionability checks — but use this sparingly as it may indicate a real issue with your selector.


### Text Input


` fill()` sets an input's value directly (fastest, works for most cases).` clear()` empties a field.` pressSequentially()` types character-by-character, which is useful for inputs that have real-time validation or autocomplete.


```text
// Direct fill (fast)
await   page  .  locator  (  "#email"  )  .  fill  (  " [email protected]  "  )  ;


// Character-by-character (triggers keydown/keyup events)
await   page  .  locator  (  "#search"  )  .  pressSequentially  (  "playwright"  ,     {   delay  :     100     }  )  ;
```


### Keyboard Actions


Use` press("Enter")` for single keys,` press("Control+a")` for key combinations, and` keyboard.type()` for full key events. These are essential for interacting with custom widgets and keyboard-driven UIs.


### Forms


For dropdowns, use` selectOption()` by value, label, or index. For checkboxes and radio buttons, use` check()` ,` uncheck()` , and` setChecked()` . File uploads are handled with` setInputFiles()` .


### Handling Dialogs


Set dialog listeners before triggering them. This is critical — if you don't register the handler first, the dialog will block execution.


```text
// Always register dialog handler BEFORE the action that triggers it
page  .  on  (  "dialog"  ,     async     (  dialog  )     =>     {
console  .  log  (  dialog  .  message  (  )  )  ;
await   dialog  .  accept  (  )  ;
}  )  ;
await   page  .  locator  (  "#delete-button"  )  .  click  (  )  ;
```


### File Downloads


Handle file downloads by waiting for the download event while triggering the download action:


```text
const     [  download  ]     =     await     Promise  .  all  (  [
page  .  waitForEvent  (  "download"  )  ,
page  .  locator  (  "#download-button"  )  .  click  (  )  ,
]  )  ;
```


## Advanced Topics


### Working with Frames


Use` frameLocator()` to interact with iframe content. Playwright automatically pierces open Shadow DOM, so no special handling is needed for shadow roots.


```text
const   loginFrame   =   page  .  frameLocator  (  "iframe#login-iframe"  )  ;
await   loginFrame  .  locator  (  "input[name='username']"  )  .  fill  (  "test_user"  )  ;
await   loginFrame  .  locator  (  "input[name='password']"  )  .  fill  (  "secret"  )  ;
await   loginFrame  .  locator  (  "button[type='submit']"  )  .  click  (  )  ;
```


### Screenshots


Capture screenshots of the viewport, the full page (including below-the-fold content), or specific elements. You can also mask sensitive areas using the` mask` parameter to hide PII or dynamic content.


### Network Interception


Block unnecessary resources to speed up automations, or modify requests by adding custom headers. Use` page.route()` with glob patterns to intercept requests:


```text
// Block images and stylesheets for faster scraping
await   page  .  route  (  "**/*.{png,jpg,jpeg,css}"  ,     (  route  )     =>   route  .  abort  (  )  )  ;


// Add custom headers to all requests
await   page  .  route  (  "**/*"  ,     (  route  )     =>     {
route  .  continue  (  {
headers  :     {     ...  route  .  request  (  )  .  headers  (  )  ,     "X-Custom"  :     "value"     }  ,
}  )  ;
}  )  ;
```


### Executing JavaScript


Use` page.evaluate()` to execute custom JavaScript in the browser context. This is useful for programmatic scrolling, DOM manipulation, reading computed styles, and accessing browser APIs not exposed through Playwright.


```text
// Scroll to bottom of page
await   page  .  evaluate  (  (  )     =>   window  .  scrollTo  (  0  ,   document  .  body  .  scrollHeight  )  )  ;


// Get computed style
const   color   =     await   page  .  evaluate  (  (  )     =>     {
const   el   =   document  .  querySelector  (  ".highlight"  )  ;
return     getComputedStyle  (  el  )  .  backgroundColor  ;
}  )  ;
```


### API Requests


Use` page.request` to make HTTP requests that share the browser's cookies and session state. This is powerful for combining UI automation with direct API calls:


```text
const   response   =     await   page  .  request  .  get  (  "https://api.example.com/data"  )  ;
const   data   =     await   response  .  json  (  )  ;
```


### Cookies and Storage


Read cookies with` context.cookies()` , set them with` context.addCookies()` , and clear with` context.clearCookies()` . Access localStorage and sessionStorage through` page.evaluate()` . This is essential for managing authentication state across automation runs.


### Drag and Drop


Playwright supports drag-and-drop with a simple API:


```text
await   page  .  locator  (  "#draggable"  )  .  dragTo  (  page  .  locator  (  "#drop-zone"  )  )  ;
```


## Conclusion


Playwright provides a robust foundation for building deterministic browser automations. Combined with Intuned's runtime, built-in stealth mode, proxy management, and CAPTCHA solving, you have everything you need to automate even the most complex web workflows reliably at scale.


To get started, check out the Playwright basics cookbook in our docs, or sign up at app.intuned.io and let the Intuned Agent build your first automation for you.
