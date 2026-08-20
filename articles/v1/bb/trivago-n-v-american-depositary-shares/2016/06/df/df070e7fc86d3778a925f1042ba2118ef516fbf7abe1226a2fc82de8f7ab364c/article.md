---
schema_version: "1.0.0"
document_id: "df070e7fc86d3778a925f1042ba2118ef516fbf7abe1226a2fc82de8f7ab364c"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2016-06-16-thoughts-on-atom-building-in-the-pattern-library/"
published_at: "2016-06-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:27:31.756931+00:00"
content_hash: "sha256:ebb26b5be14d624eb384ddc38d4c1a422d97d1fe1bb34c6726626b3cae3110ac"
---

# Thoughts on Atom Building in the Pattern Library

At trivago we are building and using a Pattern Library which is based on Brad Frost’s[Pattern Lab](http://patternlab.io/) adapted to our needs; our patterns are written in[Twig](http://twig.sensiolabs.org/) .


This Pattern Lab is based around Brad’s[Atomic Design](http://bradfrost.com/blog/post/atomic-web-design/) , which is also something that we are embracing.


When constructing our patterns, particularly at the lowest level, I often find myself asking questions such as, “what is the purpose of this atom?, “how complicated can this atom be?”, which usually leads to “is this even an atom?”


This article contains some personal thoughts on my thinking process around the building of atoms, and does not reflect what we are actually doing at trivago. At least not yet…


The best way to explain what I am on about is through an example, and for this I will take one of the simplest examples of all, a button.


## Purpose


First of all, what is this atom supposed to do? At the most basic level it should provide a button pattern for a developer to use. This appears to be quite easy then, in that it should simply contain a` <button>` tag.


```text
<  button  ></  button  >
```


But of course this is pretty useless to anyone, as a button at least requires some text to display to a user.


```text
<  button  >Click Me!</  button  >
```


Sadly this doesn’t really increase the atom’s usefulness as the text to display should almost certainly be dynamic.


```text
<  button  >{{ text | raw }}</  button  >
```


This makes our button atom more usable — and even allows the text in question to contain some HTML — but it is still not very practical as it will probably need some classes to be added to it, depending on how it should appear.


```text
<  button  {%   if   classList %}   class  =  "{{   classList   }}"  {%   endif   %}>{{ text | raw }}</  button  >
```


Since it is a button, it will probably have some event listener(s) added to it at some point, and the simplest way to facilitate that is by allowing the button to have a unique id.


```text
<  button  {%   if   id %}   id  =  "{{   id   }}"  {%   endif   %}
{%   if   classList %}   class  =  "{{   classList   }}"  {%   endif   %}>{{ text | raw }}</  button  >
```


## Flexibility: Complexity


As you can see, this atom is already getting a bit complicated, but this is required in order for it to be actually usable in a way that a developer can include it in their templates.


```text
{%   include   "atoms-button"   with   {
classList:   "btn btn__price"  ,
text:   "View Deal"
}   only   %}
```


But this appears to be a necessary evil in order to make the atom usable. The question is, how complex can it be? Making it overly complex seems to move away from the apparent simplicity of Atomic Design’s base unit, the atom.


To make the atom even more flexibile and therefore usable, it could be required that an array of` data-*` attributes and WAI-ARIA attributes can be added, as well as the possibility to mark the button as disabled.


```text
<  button  {%   if   id %}   id  =  "{{   id   }}"  {%   endif   -%}
{%   if   classList %}   class  =  "{{   classList   }}"  {%   endif   -%}
{%   if   dataAttributes %} {{ dataAttributes | join(  " "  ) }}{%   endif   -%}
{%   if   ariaAttributes %} {{ ariaAttributes | join(  " "  ) }}{%   endif   -%}
{%   if   disabled %}   disabled  {%   endif   -%}
>{{ text | raw }}</  button  >
```


Now this is quite flexible, but is it way too complex to be an atom? It is still “only” one HTML tag, but it is now quite usable and can be used in most situations that require a button.


But can this be simplified even further? So far this atom contains very repetitive code, such as adding an` id` ,` classList` ,` dataAttributes` etc., and many other tags within atoms may need to add these. Could the code that adds these be moved into a separate “meta-pattern” as it were and included here?


### Proton


By creating another twig pattern — let’s stick with the science and call it a proton — the complexity can be moved elsewhere, thus simplifying the atom.


```text
{%   if   id %} id="{{ id }}"{%   endif   -%}
{%   if   classList %} class="{{ classList }}"{%   endif   -%}
{%   if   dataAttributes %} {{ dataAttributes | join(  " "  ) }}{%   endif   -%}
{%   if   ariaAttributes %} {{ ariaAttributes | join(  " "  ) }}{%   endif   -%}
{%   if   disabled %} disabled{%   endif   -%}
```


Moving the above code to` proton-attributes.twig` then allows the button atom to be reduced to something much simpler.


```text
<  button  {%   include   "proton-attributes.twig"   %}>{{ text | raw }}</  button  >
```


As mentioned above, the added benefit of this is that other atoms can also include this proton and avail of its functionality. In addition, if it is decided that a new attribute needs to be added to the proton, it will be automatically accepted by other atoms that include it.


## Atom?


After reducing the complexity of the atom, the answer to the question of “is this even an atom?” — in this case — is probably yes.


However, there are other cases where this question may not at first appear to be so easy to answer.


What if the requirement is for a button that can display a tooltip on hover? The tooltip in question is contained within another atom file, since it may also be used elsewhere.


```text
<  div   class  =  "btn-tooltip__wrapper"  >
{%   include   "atoms-tooltip"   with   {
text:   "Go to check in area"
}   only   %}
{%   include   "atoms-button"   with   {
classList:   "btn__checkin btn--icon"  ,
text:   "Check In"
}   only   %}
</  div  >
```


Would this be an[atom](http://bradfrost.com/blog/post/atomic-web-design/#atoms) or a[molecule](http://bradfrost.com/blog/post/atomic-web-design/#molecules) ?


My initial thoughts here are that it should still be an atom, as it will be used and displayed in the same way as a button would be, just with a tooltip that appears on hover.


But this complicates the atom once again, and if we decide to also follow a rough rule of “atoms shouldn’t include atoms”, then this cannot be an atom, and we need to go one step up and refer to it as a molecule.


Brad Frost[says that](http://bradfrost.com/blog/post/atomic-web-design/#atoms) :


> Atoms are the basic building blocks of matter. Applied to web interfaces, atoms are our HTML tags, such as a form label, an input or a button. Atoms can also include more abstract elements like color palettes, fonts and even more invisible aspects of an interface like animations. Like atoms in nature they’re fairly abstract and often not terribly useful on their own.


And this definition would clearly point at this particular example being a molecule, which again Frost[refers to](http://bradfrost.com/blog/post/atomic-web-design/#molecules) :


> Molecules are groups of atoms bonded together and are the smallest fundamental units of a compound.


Which I guess is a comprehensive explanation of why this particular example should be a molecule and not an atom, as it is a compound pattern that bonds one atom with another.


## Conclusion


Atoms in general should follow the mentality of “do one thing and do it well”, but in reality there are occasions where, in order to make them more dynamic, flexible, and therefore useful, they are required to become more complex. To facilitate this the proton idea has been introduced to help reduce this complexity when viewing the atom in its entirety.


Once you are bonding atoms together or adding extra HTML markup around an atom to build a pattern, then it should become a molecule, as atoms shouldn’t contain atoms and anything extra being added to an atom for a specific purpose changes its usage.


But of course these are only thoughts and guidelines from me, and everyone’s situation is different, and what you are working on may require a slightly different thought process depending on your needs.


Food for thought.
