---
schema_version: "1.0.0"
document_id: "7f59f53bd6914b30645fdcd085bf2f83981b2c993707ae1813f3d908dfcb8daf"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-style-console-log-messages/"
published_at: "2022-01-05T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:8d1d6c114c15321c466ee463dc6bfb983e4c07a28473c44df8f88180c237715a"
---

# How to style console log messages

We recently added a little easter egg to Basedash which shows ASCII art of our logo, and a short message with links in the browser console.


Basedash


I’ve seen a few other products do something similar, including Linear and Facebook:


Linear


Facebook


One thing you might notice is that both of these examples apply styling to the console messages. Linear uses a monospace font (which is necessary for ASCII art to display properly), and Facebook changes the text size and color.


Here’s how you can do the same:


## Applying styling to console.log messages


First, start with a standard` console.log` statement:


```text
console.log(  'Basedash is rad'  );
```


Then, add` %c` to the start of your string:


```text
console.log(  '%cBasedash is rad'  );
```


Finally, add a second parameter with some CSS:


```text
console.log(  '%cBasedash is rad'  ,   'color: red; font-size: 20px;'  );
```


The CSS from the second parameter is applied to everything after the` %c` . Most CSS properties that affect text work—you can see the[full list on MDN](https://developer.mozilla.org/en-US/docs/Web/API/console#styling_console_output) .


You can also add multiple` %c` tags to apply different styles to different parts of your message. Each` %c` tag adds its own parameter to the` console.log` function call, like so:


```text
console.log(  '%cBasedash is %crad'  ,   'color: red',   'color: green'  );
```


Rad!


Some other ideas to try:


- Embed an image with` background-image`
- Change the` font-family` to match the rest of your website
- Add a 3D effect with` box-shadow`
- Italicize text with` font-style`


Check out the full MDN docs on[styling console output here](https://developer.mozilla.org/en-US/docs/Web/API/console#styling_console_output) .
