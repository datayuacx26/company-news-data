---
schema_version: "1.0.0"
document_id: "b379ac9feddca7edae283fef331238eaee71a2bdd303c3c010a6dc56b155bce4"
company_key: "yc-readme"
company: "ReadMe"
source_id: "yc-readme-news-import-ecdc4511c006"
canonical_url: "https://readme.com/blog/mdxish"
published_at: null
first_seen_at: "2026-07-22T11:00:35.158559+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:46433289b5f46082cb6309751c26f85b5ce3beed6e6dadea7401ed6488a0de7a"
---

# Introducing MDX-ish

Last year, we introduced our new Git-based editing experience. It came with an upgrade we were excited about: we swapped out Markdown for MDX. It allows users to write complex, interactive components in their docs. However, while MDX is incredibly powerful, it also required perfectly valid JSX which felt like a step back for anyone writing in ReadMe.


So, we built MDX-ish. It’s as permissive as Markdown, without sacrificing the versatility of MDX.


## What’s Wrong with Markdown?


Most people know what Markdown is. It’s a “standard” introduced in 2004 by John Gruber as a syntax that lets users add formatting to docs that looks good both rendered and unrendered.


```text
Hello, this is **Markdown**. You can learn more
on [Daring Fireball](https://daringfireball.net/projects/markdown/syntax).


Some features include:
- Is readable as plaintext
- Is simple to write
- Can be rendered as HTML
```


While there’s a lot of seriously great things about Markdown, there’s shortcomings when you’re building out complex documentation.


First up, notice how I used scary-quotes around the word “standard” above? That’s because the[original post](https://daringfireball.net/projects/markdown/) was pretty loose on details when it came to handling edge cases. Different parsers can reasonably produce different output from the same input, which isn’t great.


This spawned a number of different flavors, such as[GitHub Flavored Markdown](https://github.github.com/gfm/) (which bulked up Markdown with tables, strikethrough, autolinking of URLs, fenced code blocks and more) or[CommonMark](https://commonmark.org/) (an attempt to turn Markdown into a strongly defined specification). We’ve even gone back and forth ourselves here at ReadMe. Some tweaks tightened the output to better match the source (like honoring linebreaks), while others made the parser more forgiving (like accepting` #Header Title` without a space after the hash).


Markdown also lacked the ability to do more complex UI, which made docs written with it start to feel quite dated. As frontend started to move toward component-based architectures, Markdown began to feel like it needed a bit of a punch-up.


## Next up, MDX!


When we decided to do a major migration, switching to MDX made a lot of sense.


[MDX](https://mdxjs.com/) is exactly what it sounds like: Markdown with JSX. Here’s the tagline from their site:


> MDX lets you use JSX in your markdown content. You can import components, such as interactive charts or alerts, and embed them within your content. This makes writing long-form content with components a blast. 🚀


This framing makes it seem like MDX is just a superset of Markdown, which sounds great! Take what’s already working, and just make it a bit more powerful.


As we started to migrate, it became clear it wasn’t that easy.


Both Markdown and HTML share a unique property: they’ll always do their best to render *something* . It might not be exactly what you were going for (or, often, even close), however it will still do it’s best to render something. A` <b` that isn’t closed won’t render anything bold, but the whole page won’t sputter to a halt.


JSX is less forgiving. One wrong character, and the whole page will throw an error. There’s no fault-tolerance.


Markdown and MDX feel spiritually similar, but they’re quite different in practice.


` <Accordion>Hi</Accordion>`


` <div style="color: blue">Hi</div>`


` <div style={{ color: "blue" }}>Hi</div>`


` <div className={"hi"} style="background: orange">Hi</div>`


` <!-- html comment -->`


` {/* JSX comment */}`


` <b>invalid HTML`


` <br>`


### Old Docs


We had 23 million pages in ReadMe, written over the better part of a full decade. There was everything from small errors to 10,000-line jumbled messes of half-working HTML. And on top of that, even perfectly valid HTML (such as` <div style="color: red">` ) would need to be converted to JSX (such as` <div style={{ color: "red" }}>` ).


We wrote a parser that would convert Markdown pages to MDX. We did our best to do it deterministically, which was able to flawlessly convert about 65% of pages hosted on ReadMe... which, if you're keeping track, left about 10 million pages we just couldn't get right with pure code. We used a combination of AI and contractors to convert the rest. It’s been a slow and resource-intensive undertaking the past year.


### New Docs


We thought once we got through the migration, things would be wonderful. As it turns out, that’s not true. It was a pain to write new docs, too. You had to remember JSX syntax, and slight errors threw out an incomprehensible error message.


There had to be a better way…


## Introducing MDX-ish


One of our company goals is Make Things that Just Work, and MDX didn't feel that way. It's powerful, but it was too pedantic. The failed saves took you out of the writing flow.


This is how we got to MDX-ish. It’s exactly what it sounds like. It’s MDX… sorta. It’s the best of both worlds (with some tradeoffs, of course), with the goal of Just Working.


To explain how MDX-ish works, we first need to break down how Markdown works behind the scenes.


While early Markdown rendering engines were a bunch of regexes, most modern Markdown parsers convert the syntax into an Abstract Syntax Tree. And then they convert that syntax into HTML… or, well, whatever output target the user wants. That’s the abstract part: it allows for arbitrary compilation.


Markdown


```text
# Hello World


This is   **bold**   text with a   [link](https://example.com)  .
```


MDAST


root


heading (depth: 1)


text: "Hello World"


paragraph


text: "This is "


strong


text: "bold"


text: " text with a "


link (url: "https://example.com")


text: "link"


text: "."


HTML


```text
<h1>Hello World</h1>
<p>  This is   <b>bold</b>   text with a   <a href="https://example.com">link</a>  .  </p>
```


Like I mentioned before, because Markdown and HTML are so fault-tolerant, *something* will always render. You might not get the results you were going for, but it will always be able to convert Markdown into something.


This got me thinking… what if we parsed Markdown into an Abstract Syntax Tree, and then made a per-node decision on if it should be rendered as HTML or JSX? That way, if it’s valid JSX, we’ll get all the power of MDX. However, if it’s HTML (broken or otherwise), our parser will still do its best to render something.


Here’s what an MDX-ish document looks like. You can mix Markdown


, JSX


and HTML


:


```text
Regular   **markdown**   renders just   _fine_  .      You can   <span>nest   _markdown_   in HTML</span>      You can mix   <strong style="color: red">HTML</strong>   and   <strong style={{ color: "blue" }}>MDX</strong>   together.      You can even   <span   style="color: red"   className={"test"}>mix and match attributes</span>   in the same element.      You can use   <!-- html comments -->   next to   {/* JSX comments */}      <Callout>MDX components just work, too!</Callout>      Invalid HTML will still   <b>make the rest of the line bold      And self-closing tags don't need to be closed manually!  <br>
```


I mentioned downsides. The biggest, of course, is it’s always better to stick to an[existing standard](https://xkcd.com/927/) .


By relaxing MDX’s strictness, we lose one of its biggest strengths: with pure MDX, you know what you’re going to get, assuming the JSX is 100% valid. When you start mixing and matching things, however, unpredictable things can happen.


Another downside is that a lot of people see the permissiveness of Markdown as a bug, not a feature. They want to know when something’s wrong, and they’d rather have the parser yell at them than silently render something unexpected. It’s the same argument you hear about JavaScript vs. TypeScript: some people love that JS just runs, and others want the compiler to catch their mistakes before anything ships. Ultimately, we felt that for our customers, visually debugging by seeing the weird output and fixing it was easier and more approachable than parsing a stack trace.


Lastly, in MDX-ish, you can’t write JSX components inline. There’s no technical reason we couldn’t support this; we just knew very few people chose to write MDX components inline rather than writing them in our component editor. If you do start your page with an MDX component, we fall back to pure MDX.


## Let’s get technical


The hardest part of MDX-ish is how we decide, on the fly, whether any given` <tag>` should be treated as HTML or JSX.


The decision is made at the tokenizer level, before any AST is built. We use[micromark](https://github.com/micromark/micromark) , the streaming character-by-character parser that powers the[unified](https://unifiedjs.com/) /[remark](https://remark.js.org/) ecosystem, and register a custom` mdxComponent` construct that competes with[CommonMark](https://commonmark.org/) 's built-in HTML construct at every` <` . The rule is simple: PascalCase tags are always claimed as JSX, lowercase tags are HTML by default, and the only thing that escalates a lowercase tag to JSX is the presence of a` {...}` attribute somewhere in the opening tag.


So, we assume it’s HTML going into each element, but as soon as we see it either begins with an uppercase letter or we get to the first` {` , we switch to JSX.


Scanning for JSX


{ isElement: false


, isJSX: false


}


W


e


s


u


p


p


o


r


t


<


b


>


H


T


M


L


<


/


b


>


,


<


M


D


X


C


o


m


p


o


n


e


n


t


s


/


>


,


a


n


d


a


<


s


p


a


n


s


t


y


l


e


=


{


{


c


o


l


o


r


:


"


r


e


d


"


}


}


>


m


i


x


<


/


s


p


a


n


>


.


W


e


s


u


p


p


o


r


t


<


b


>


H


T


M


L


<


/


b


>


,


<


M


D


X


C


o


m


p


o


n


e


n


t


s


/


>


,


a


n


d


a


<


s


p


a


n


s


t


y


l


e


=


{


{


c


o


l


o


r


:


"


r


e


d


"


}


}


>


m


i


x


<


/


s


p


a


n


>


.


After tokenization, remark hands us an[MDAST](https://github.com/syntax-tree/mdast) (Markdown Abstract Syntax Tree) and our transformers walk the tree to convert claimed html nodes into proper JSX tree nodes (both block-level and inline variants) shaped to match the standard MDX AST so the rest of the remark ecosystem can read them as if they came from a real MDX parser.


We want to also support non-MDX attributes (mixed in with MDX attributes), so we use a second hand-rolled parser that handles quoted strings, brace expressions, unquoted values, and bare booleans, interchangeably on the same tag. Basically, if it sees a (now) MDX component with a` style="color: red"` , it will convert it to` style={{color: "red"}}` .


The contents inside the braces are kept as opaque source text. We never run them through a JavaScript AST parser like[acorn](https://github.com/acornjs/acorn) , which is what real MDX does. Instead, we do this later, when we can safely sandbox the evaluation. From there the tree flows through[remark-rehype](https://github.com/remarkjs/remark-rehype) to a[HAST](https://github.com/syntax-tree/hast) , where[rehype-raw](https://github.com/rehypejs/rehype-raw) parses any raw HTML strings with[parse5](https://github.com/inikulin/parse5) . We tell rehype-raw to skip our JSX nodes (via its` passThrough` option), so their property values survive as real JS objects, arrays, and numbers instead of being round-tripped through HTML serialization.


That tokenizer-only approach is what makes MDX-ish so permissive. Take` <span style="color: red" className={"test"}>` ; the brace flips it to JSX, we record both attributes verbatim, and the string "color: red" lands in the HAST's property bag without anything (yet) validating that JSX semantically requires style to be an object. The rescue happens at render time, where[rehype-react](https://github.com/rehypejs/rehype-react) (via[hast-to-hyperscript](https://github.com/syntax-tree/hast-to-hyperscript) ) recognizes style as a special hast property and runs string values through an inline-style parser before handing them to React. The same boundary turns class into className, joins or splits space-separated token lists, and normalizes a handful of other HTML-shaped quirks. Real MDX would catch all of this with` acorn` , but it'd also reject every loose ReadMe document that wasn't authored as strict JSX. We deliberately chose the other side of the tradeoff: a tokenizer-level decision, a tiny structural AST walk, and the rendering layers below doing the actual interpretation.


## What it means for you


You’ll be able to write either MDX or Markdown in any doc. You can mix and mingle them in the same doc, the same line, or even the same element.


If you want the simplicity of Markdown (and basic HTML), go for it! If you prefer the power that comes along with JSX, even better.


We’ve also paired it with a brand new editor, written from the ground up, to support the more permissive syntax. It can highlight both JSX and Markdown and no longer throws errors if you’ve written something that’s not valid MDX. (And there’s a lot more great about it! But that’s for another blog post.)


Play around with it, and let us know what you think! If you see any problems,[file an Issue](https://github.com/readmeio/markdown) and we’ll get on it right away.


Our new MDX-ish render is currently available on all new projects by default, and self-serve via a one-click upgrade. We're rolling it out to Enterprise, but contact your CSM if you want to try it out right away.


---


*Shout out to Rafe Goldberg and Kevin Ports for taking my crazy prototype and architecting it into something production-worthy that could performantly render 23 million documents across two different Markdown dialects. They worked with a team of wonderful engineers, Dimas Anugerah, Falco Widjaya, and Jayden Nguyen, to make it happen.*
