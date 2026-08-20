---
schema_version: "1.0.0"
document_id: "c190795fb094f818e3a9bb861466b68d5cb0f20d7bba6a70c021315126ea6317"
company_key: "rent-the-runway-inc-class-a-common-stock"
company: "Rent the Runway Inc."
source_id: "rent-the-runway-inc-class-a-common-stock-rss-571d2b3d11b7"
canonical_url: "https://dresscode.renttherunway.com/blog/sass-lists-and-maps"
published_at: "2017-03-01T13:39:13+00:00"
first_seen_at: "2026-07-20T23:19:27.389009+00:00"
fetched_at: "2026-07-28T22:27:28.230343+00:00"
content_hash: "sha256:ab50b373138ac166be4a8ced10645f093ddb367854d8a8c563875f338953878b"
---

# Refactoring Sass with Lists and Maps

At Rent the Runway, we have an internal tool to easily create emails for our customers. It’s a Rails/React system that turns .erb template files into[SailThru](http://www.sailthru.com/) -compatible layouts (SailThru is our software for sending customer emails).


In this repo we use[Sass](http://sass-lang.com/) utility classes. If you’re unfamiliar with utility classes, they’re usually classes that contain a single CSS rule, like so:


```text
.left { text-align: left; }
```


They’re useful for highly descriptive markup – in our case, creating inline styles for emails. For example, to align your text left in an email, you would use the above class like so:


```text
<td class=”left”> Here is some left-aligned text. </td>
```


See[this post on CSS tricks](https://css-tricks.com/using-css-in-html-emails-the-real-story/) for a great summary of why inline design is commonly used for emails. Using utility classes allows us to more easily match inline styles to particular email elements.


However, creating stylesheets for utility classes can get messy. We quickly ended up having a number of stylesheet partial files that contained quite repetitive CSS blocks:


The padding partial had almost 200 lines of definitions like that! We had similar problems for heading definitions and background colors:


I could go on, but you get the idea – not very DRY and awkward to add to. What do you do if you quickly need another padding utility class? Just add it onto the end, and worry about duplication later. These stylesheets were getting unruly, unreadable, and impossible to maintain. It seemed like a great opportunity to refactor using two cool Sass features: lists and maps.


First up, we refactored the padding partial to use lists – similar to arrays in other languages. The padding classes were mixed up and mostly named after the padding pixel value specified (e.g. .padding-left-25px). However, a couple used comparative suffixes – e.g. padding-left-max. Our first job was to standardize all the classes to use pixel values in their class names. It’s easier to understand \`.padding-left-25px\` than \`.padding-left-max\`.


Next, we grouped all the classes by rules: whether they specified one particular padding (e.g. padding-left), two sides (e.g. top and bottom) or all padding.


Already it was easy to see that there were a couple of duplicate rules. Those could be deleted. The next step was to turn the necessary values into a list. Lists in Sass are incredibly easy to create: just take all values and separate them with commas:


```text
$padding-left-values: 5px, 10px, 20px, 30px, 50px, 70px;
```


Then, we could use the @each function of Sass to loop through all the pixel values. Using interpolation with #{} , we could name the class appropriately and give the padding value:


```text
@each $px in $padding-left-values {
.padding-left-#{$px} {
padding-left: $px;
}
}
```


This produces all the classes I need without having to type them all out manually. Also, it makes the process of adding a new padding class far easier: just add the px value to the list.


The next challenge was to refactor the heading sizes. Again, there were a number of repeated classes detailing the various header sizes. However, this time they were named based on relative sizes (e.g. small, medium, large) rather than the absolute pixel value that was found in the style rule.


This called for a map – similar to objects in JavaScript or hashes in Ruby. The heading sizes map had keys that corresponded with the name, and values that corresponded with the pixel size of the text:


```text
$heading-sizes: (
smallest: 25px,
small: 36px,
medium: 40px,
large: 80px,
largest: 110px
);
```


As with lists, you can loop over a map using the @each function. As we’re using both the key and the value, you define both of those first.


```text
@each $size, $px in $heading-sizes {
.heading-#{$size} {
@include times($px);
}
}
```


Notice here that we’re already using a mixin – times() – that sets an appropriate font family, size, and calculates a line-height based on the font-size passed in.


We did a similar thing with colors, using a color map to loop through and create utility classes for background colors:


```text
$colors: (
// Primary copy
midnight: #272c32,
cotton: #fff,
// Accent
tan: #cd9365,
fawn: #c79176,
tangerine: #e85940,
error: #eb5840,
);


// Background colors file


@each $color, $hex in $colors {
.#{$color}-bg {
background-color: $hex;
}
}
```


Both of these compile to CSS similar to our bloated Sass stylesheets from before. However, it’s much easier to add, remove, or check for duplicate colors or header sizes in this system. Let’s say a new seasonal color needed to be added – rather than having to write out the whole class definition, just add the name and hex value to the colors map.


What’s more, it always feels so satisfying making a commit with more deletions than additions, don’t you think?
