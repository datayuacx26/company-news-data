---
schema_version: "1.0.0"
document_id: "1e565d937670fc08acb76452d08ec3a7c21e49d613b5610ddf9b56edd93647e0"
company_key: "yc-slash"
company: "Slash"
source_id: "yc-slash-news-import-9ec40849fc98"
canonical_url: "https://www.slash.com/engineering/blog/facelift"
published_at: "2025-11-21T18:00:00+00:00"
first_seen_at: "2026-07-24T01:09:43.930358+00:00"
fetched_at: "2026-07-28T22:25:10.100738+00:00"
content_hash: "sha256:688fd52e34b9e0bb238e4014892466b48d83d0f29affb155c6b121a24f01553a"
---

# The Facelift

We rebranded Slash as a part of our[Series B fundraising](https://www.slash.com/blog/slash-raises-series-b) , and we wanted to visually refresh our product alongside our brand. We quickly realized that along with updating our UI, this was a great opportunity to address much of the tech debt that we had accumulated over the last 3 years of scaling extremely quickly.


Thus, project ***Facelift*** was born - a chance for us to lay the foundation for Slash’s frontend for our scaling business and team. We wanted to make decisions that would stand the test of time by learning from the mistakes of our past.


## Part 1: Addressing the technical debt


In this section, I’ll talk about each point of “debt”. Later in part 2, I go into depth about our fixes for each point.


### Emotion: CSS in JS runtime bottlenecks


Our original design system built on top of[emotion](https://emotion.sh/docs/introduction) and served us well. The main advantage of a CSS in JS library was the co-location of a component’s markup and styling - which made iteration extremely quick. However, as our customers scaled and had increasingly large data volume, we started seeing performance bottlenecks in emotion. CSS-in-JS requires runtime overhead, and can slow down the main event loop. It’s noticeable when virtualizing large lists and scrolling quickly. When components mount and unmount quickly, dynamically computing styles and generating stylesheets impacts the browser to the point where it can’t render at 60fps. Other companies have[found similar performance bottlenecks](https://medium.com/airbnb-engineering/airbnbs-trip-to-linaria-dc169230bd12) and moved away from runtime overhead dependent solutions.


### Prop based “God components”


The original concept of “[God objects](https://en.wikipedia.org/wiki/God_object) ” stems from OOP, where an object would handle too much responsibility alone. We noticed similar things happening in our React components, which we started to call “God components”.


Many of our React original components were written with a props-based interface, where you provide data and callbacks and render a single component. On the surface this provides an elegant interface: simply provide data and props and trust the component to take care of all markup and business logic.


In practice, as the codebase grows, these components get extended to cover usages that weren’t originally considered or intended. Each time an engineer needed a new configuration, they would add props, making maintenance and usage more difficult over time. These components eventually became “God components” that needed care to maintain, with one-off props meant to solve certain issues, or “escape hatches” meant to inject/replace additional markup into the component itself (similar to a render prop pattern). I will show code examples later in the blog.


### Inconsistent form management


Our original form management solution was simply React state management. Local state is easy to use, but lacks standardization and deep type safety which led to inconsistent code quality. The way each engineer implemented local state would be a bit different - some would leverage the[context API](https://react.dev/learn/passing-data-deeply-with-context) with one large shared form state, while others would splinter the fields into individual` useState` calls.


When it came time to extend these forms, oftentimes metadata states like` isFieldTouched` would be added, creating a messy component with lots of explicitly defined local state to handle form interactions that should be standardized.


## Part 2: Execution


Now that we've outlined the current pitfalls of our codebase, I’m going to dive deep into the solutions and the decision-making behind each one.


### Migrating to Tailwind v4.0


When evaluating styling solutions, we had two finalists: Tailwind v4.0 and[PandaCSS](https://panda-css.com/) , as both have zero runtime overhead and allowed co-location of a component’s markup and styling. The advantages of PandaCSS were type safety and a low learning curve since our existing design system looked very similar.


Tailwind’s advantages were a mature ecosystem and developer familiarity. Many of our engineers had used Tailwind for personal projects and were quite fond of it, myself included. There are fantastic tools with built-in Tailwind support like[Supernova](https://www.supernova.io/) and[Tailwind Variants](https://www.tailwind-variants.org/) . AI coding tools are also excellent at writing Tailwind classnames out of the box, making migration much less painful.


To visualize the performance advantages of having no runtime overhead, here are the before and after flame charts of the same operation (scrolling quickly on a large, virtualized table view), in Emotion and Tailwind:


Emotion introduces runtime style injection overhead


Tailwind doesn’t suffer from these bottlenecks!


### Figma to Code pipeline


When migrating to Tailwind, we wanted a way to keep our Figma tokens and codebase in sync, with Figma being our source of truth. Our design system in Figma is maintained by our design agency,[Metacarbon](https://www.metacarbon.com/) , who makes the design decisions and creates the stylesheet with semantic tokens.


Our goal was to have a pipeline where any changes to the underlying design system could be manually reviewed and implemented with minimal effort. We chose Supernova to pull design tokens out from our Figma and into code, as they have an excellent Tailwind v4.0 to code extractor.


While working with our css output, we encountered an interesting problem: ***we wanted to have semantic tokens point to different values, depending on the property they applied to.*** Here’s an illustrative example:


```text
/* Raw Supernova output */
@theme   {
--color-bg-neutral-subtle-default: var(--color-light-product-neutral-100);
--color-border-neutral-subtle-default: var(--color-light-product-neutral-500);
--color-text-neutral-subtle-default: var(--color-light-product-neutral-1000);
}
```


We want` neutral-subtle-default` to resolve to 3 different shades of` light-product-neutral` :` 100` ,` 500` and` 1000` - however, if we just used this, the Tailwind v4.0` --color`[theme variable](https://tailwindcss.com/docs/theme) would spawn` bg-bg-neutral-subtle-default` ,` border-bg-neutral-subtle-default` ,` bg-text-neutral-subtle-default` and many other classNames that both don’t make sense and are redundant in naming.


As such, we wrote a custom build script to transform our output to utilize Tailwind v4.0’s more specific[theme variable namespace](https://gist.github.com/philipp-spiess/83101e0d0fce4d70fa080dbf31560894) :


```text
/* Build script output */
@theme   {
--background-color-neutral-subtle-default: var(--color-light-product-neutral-100);
--border-color-neutral-subtle-default: var(--color-light-product-neutral-500);
--text-color-neutral-subtle-default: var(--color-light-product-neutral-1000);


/* Additional properties that we want to have the same values as other tokens */
--divide-color-neutral-subtle-default: var(--color-light-product-neutral-500);
}
```


Which resolves to classNames like` bg-neutral-subtle-default` ,` border-neutral-subtle-default` and` text-neutral-subtle-default` - which all resolve to their distinct shades.


So, as our underlying design system changes, we simply need to run our Supernova exporter and put the output into our codebase to review - then, our build script automatically converts the raw output to a built CSS file that all of our packages consume.


Good to know


At Slash, we follow a philosophy of ***maintaining control*** over our tooling . This allows us to extend functionality quickly and prevent vendor lock in. It also helps us diagnose and fix issues directly. By having our build script, we aren’t locked into Supernova as a vendor, and can extend it - like adding support for` divide` to be the same color as` border` , or light vs dark mode themes.


Other examples include our query builder and JSON schema to typescript code generation pipeline.


### Headless UI libraries: Base UI and React Aria


Firstly I want to explain why we wanted a headless UI library, and not a styled solution. The entire point of the facelift is for our brand identity to be expressed clearly through our product - and using a pre-styled solution like Shadcn or MaterialUI would defeat this purpose by pre-styling our components.


Pre-facelift we used Radix UI, but we weren’t[sure about its future](https://dev.to/mashuktamim/is-your-shadcn-ui-project-at-risk-a-deep-dive-into-radixs-future-45ei) , as we saw the core maintainers move on to Base UI. Base UI has emerged as a simple and extensible library that fits our needs very well, with great accessibility adherence. Given its excellent maintainers, Base UI gives us confidence in the future that it will grow alongside Slash’s needs.


For certain components without Base UI support yet (ex., our date picker), we went with React Aria, which was our runner-up in our hunt for a headless UI. React Aria is battle-tested and adheres strictly to all accessibility standards. Ultimately, we built our components on top of Base UI since React Aria’s approach is quite opinionated and requires us to fully buy into the React Aria ecosystem, and we preferred something with less mental overhead.


### Compound Components


As previously mentioned, props-based components are initially elegant, but suffer from poor extensibility. Our solution for creating more extendable components was to lean heavily into the[compound component architecture](https://www.patterns.dev/react/compound-pattern/) , which hands off markup control of a component to its parent. The best way to illustrate the difference between the two is an example - here is a simplified version of our old and new searchable select component:


```text
// Old SearchableSelect implememtation
type   Props  <  T  >   =   {
selection  ?:   string  ;
setSelection  ?:   (  val  :   string  )   =>   void  ;


// Escape hatches added over time
shouldSetIsChangedFromTextRefAfterTableViewUpdate  ?:   boolean  ;
customTextSelectionLogic  ?:   boolean  ;
preloadOptions  ?:   boolean  ;
hideDropdownOnEmptyOptions  ?:   boolean  ;
renderSelectedItem  ?:   (  item  :   T  )   =>   ReactNode  ;
hideRightIcon  ?:   boolean  ;
isClearable  ?:   boolean  ;


// ... more props
};


export   function   SearchableSingleSelect  <  T  >(  props  :   Props  <  T  >) {
const   {
customTextSelectionLogic   =   false  ,
preloadOptions   =   false  ,
hideDropdownOnEmptyOptions   =   false  ,
renderSelectedItem  ,
hideRightIcon   =   false  ,
isClearable   =   true  ,
shouldSetIsChangedFromTextRefAfterTableViewUpdate   =   true  ,
// ... more props
}   =   props;


return   (
<  FormElementWrapper
renderInput  =  {(  props  )   =>   (
<  MenuBase
setSelection  =  {(  value  )   =>   {
setSelection  (value);


// Escape hatch for custom behavior
if   (customTextSelectionLogic) {
return  ;
}


// Different logic based on renderSelectedItem prop
if   (value   &&   !  renderSelectedItem) {
setTextInput  (  findLabel  (value));
}   else   if   (value   &&   renderSelectedItem) {
setTextInput  (  ''  );
}
}}
>
{  /* Conditional rendering based on prop */  }
{renderSelectedItem   &&   selectedItem   ?   (
<  CustomRenderedItem   item  =  {selectedItem} />
)   :   null  }


<  TextInput   value  =  {textInput}   onChange  =  {  ...  } />
</  MenuBase  >
)}
{  /* Nested ternaries based on multiple props */  }
{  ...  (hideRightIcon   ?   {}   :   {
rightIcon  : ()   =>   {
return   !  isClearable   ?   (
<  ChevronIcon   />
)   :   textInput?.  length   ||   (renderSelectedItem   &&   selectedItem)   ?   (
<  ClearButton   onClick  =  {clearSelection} />
)   :   (
<  ChevronIcon   onClick  =  {toggleDropdown} />
);
}
})}
/>
);
}
```


```text
// Old SearchableSelect usage
<  SearchableSingleSelect
selection  =  {selection}
setSelection  =  {setSelection}
tableManager  =  {tableManager}
isClearable  =  {  true  }
customTextSelectionLogic  =  {  false  }
hideDropdownOnEmptyOptions  =  {  false  }
renderSelectedItem  =  {(  item  )   =>   <  CustomView   item  =  {item} />}
shouldSetIsChangedFromTextRefAfterTableViewUpdate  =  {  true  }
// ... more props
/>
```


And here is our new searchable select component, which uses a compound component architecture:


```text
// New SearchableSelect implementation


// Context manages shared state
const   SearchableSelectContext   =   createContext  <{
selectState  :   SingleSelectState  ;
tableManager  :   TableManagerState  ;
}>(  null  );


// Root sets up context
const   Root   =   ({   children  ,   tableManager  ,   type   })   =>   {
const   selectState   =   useSingleSelectState  ();


return   (
<  SearchableSelectContext.Provider   value  =  {{ selectState, tableManager }}>
<  MenuV2.Root  >{children}</  MenuV2.Root  >
</  SearchableSelectContext.Provider  >
);
};


// EachItem exposes items with callbacks
const   EachItem   =   ({   children   })   =>   {
const   {   selectState  ,   tableManager   }   =   useSearchableSelectContext  ();


const   items   =   tableManager.data.  map  ((  item  ,   index  )   =>   ({
item,
callbacks: {
onClick  : (  item  )   =>   selectState.  toggleItem  (item.key, index),
getIsSelected  : (  item  )   =>   selectState.  isItemSelected  (item.key),
},
}));


return   <>{items.  map  (children)}</>;
};


// SearchBar handles its own state
const   SearchBar   =   ()   =>   {
const   {   tableManager   }   =   useSearchableSelectContext  ();
const   [  textInput  ,   setTextInput  ]   =   useState  (  ''  );


useEffect  (()   =>   {
tableManager.  updateSearch  (textInput);
}, [textInput]);


return   <  InputV2.Field   value  =  {textInput}   onChange  =  {  e   =>   setTextInput  (e.target.value)} />;
};


// ... Other components: Content, DefaultTrigger, Item
```


```text
// New SearchableSelect usage
<  SearchableSelect.Root   type  =  "single"   tableManager  =  {tableManager}>
<  SearchableSelect.DefaultTrigger  >
Select an option...
</  SearchableSelect.DefaultTrigger  >


<  SearchableSelect.Content  >
<  SearchableSelect.SearchBar   />


<  SearchableSelect.EachItem  >
{({   item  ,   callbacks   })   =>   (
<  MenuV2.Item   onClick  =  {()   =>   callbacks.  onClick  (item)}>
<  CustomView   item  =  {item} />
</  MenuV2.Item  >
)}
</  SearchableSelect.EachItem  >
</  SearchableSelect.Content  >
</  SearchableSelect.Root  >
```


At first glance, the compound components are more cumbersome - the developer needs to know the markup pattern and follow it. The advantage is how flexible it is to extend the markup, since full control lies with the parent. As such, markup hacks never make it into the core component. If an engineer needs to add a footer to the select? Simply add it to the markup - no need for a` renderSelectFooter` prop to the core component! If a footer becomes a common requirement, create a` <SearchableSelect.Footer />` component!


Another common pitfall of compound components is that engineers can duplicate implementations of common patterns that aren’t in the core component. As such, our team needs to be more aware of common patterns, adding them to the core component when necessary. You can also use compound components as the underlying API for a one-line component - but we do this very intentionally as to not turn THESE components into god components. I go into more detail on how we address knowledge sharing via Storybook in a later section.


### Tailwind Variants


In actually styling our components, we relied heavily on[Tailwind Variants](https://www.tailwind-variants.org/) . Some of us have used[Class Variance Authority](https://cva.style/docs) (CVA) and wanted that kind of variant logic in our styling solution. Tailwind Variants had a[couple of features that pushed us to use it](https://www.tailwind-variants.org/docs/comparison) , the slots API and built-in conflict resolution being the main ones.


Ultimately, we just needed a solution that could keep our styling variant code tidy and was extensible enough to cover our compound component approach. Using Tailwind Variants significantly standardized our old conditional styling code, which would explicitly apply different tokens to the emotion-styled div based on its props.


Below is an illustrative example of our button implementation and usage:


```text
import   { tv }   from   'tailwind-variants'  ;


const   buttonVariants   =   tv  ({
// Slots API - different parts of the button component
slots: {
base: [
'inline-flex'  ,
'items-center'  ,
'justify-center'  ,
'gap-1'  ,
'group'  ,
],
hoverOverlay: [
'opacity-0'  ,
'absolute'  ,
'inset-0'  ,
'group-hover:group-enabled:opacity-100'  ,
'pointer-events-none'  ,
],
},


// Variants - different visual options.
variants: {
color: {
cta: {
base: [
'border'  ,
'border-neutral-boldest-default'  ,
'bg-button-cta-default'  ,
'shadow-cta-neutral-default'  ,


'hover:border-neutral-bolder-hover'  ,
'hover:bg-button-cta-hover'  ,
'hover:text-neutral-bolder-hover'  ,
],
},
ghost: {
base: [
'bg-transparent'  ,
'text-neutral-normal-default'  ,


'hover:bg-neutral-subtle-hover'  ,
'hover:text-neutral-normal-hover'  ,
],
},
},
size: {
sm: {
base: [
'h-6'  ,
'px-1.5'  ,
'typography-button-small'  ,
'rounded-border-radius-sm'  ,
],
},
md: {
base: [
'h-8'  ,
'px-2'  ,
'typography-button-medium'  ,
'rounded-border-radius-md'  ,
],
},
},
},


// Compound Variants - styles when multiple variants are combined
compoundVariants: [
{
// Ghost buttons with small size get reduced padding
color:   'ghost'  ,
size:   'sm'  ,
class: {
base:   'px-1'  ,
},
},
{
// CTA buttons at large size get enhanced shadows
color:   'cta'  ,
size:   'lg'  ,
class: {
base:   'shadow-cta-neutral-prominent'  ,
},
},
],


// Default Variants - applied when no variant is specified
defaultVariants: {
color:   'neutral'  ,
size:   'md'  ,
disabled:   false  ,
},
});


// Extract variant props type from buttonVariants
type   ButtonVariantProps   =   VariantProps  <  typeof   buttonVariants>;


// Button component props
interface   ButtonProps   extends   ButtonVariantProps   {
children  ?:   React  .  ReactNode  ;
onClick  ?:   ()   =>   void  ;
as  ?:   ElementType  ;
className  ?:   string  ;
}


// Button component implementation
export   const   Button   =   forwardRef  <  HTMLButtonElement  ,   ButtonProps  >(
({
children,
color,
size,
disabled,
onClick,
as: Component   =   'button'  ,
className,
...  rest
}, ref)   =>   {
// Generate slot classes based on variant props
const   slots   =   buttonVariants  ({ color, size, disabled });


return   (
<  Component
ref  =  {ref}
className  =  {slots.  base  ({ className })}
onClick  =  {onClick}
disabled  =  {disabled}
{  ...  rest}
>
{  /* Hover overlay slot */  }
<  span   className  =  {slots.  hoverOverlay  ()} />


{  /* Button content */  }
{children}
</  Component  >
);
}
);
```


```text
// End usage example
<  Button   color  =  "cta"   size  =  "lg"  >
Large CTA Button
</  Button  >
```


As you can see, the caller of` Button` doesn’t actually need to use` buttonVariants` - it passes props into` Button` as configuration options, that then get used to call` buttonVariants` internally. We also make use of the` extend` feature to share styles between similar components, like input fields and select fields.


You’ll also notice how easy it is to scan the styles because of our semantic classnames.


### Testing and documentation: Storybook + Chromatic


For the first half of the project, I worked solo, and didn’t value testing and documentation nearly as much as I should have. However, as I finished the baseline component library and more engineers joined to facelift their respective products, it became more apparent we needed a centralized place to answer very simple questions like “do we have this component?” and “how do I use this component correctly?”.


One of our new engineers, Sam, singlehandedly set up our Storybook and Chromatic testing suite. Without these, we could have very quickly fallen into the same tech debt traps of component misuse - especially considering the learning curve of compound components.


Storybook allows us to centralize usage examples for all components, allowing those questions to be answered in full with code examples. It also becomes immediately obvious what component an engineer should use, and whether or not it supports the configuration options they need.


Chromatic catches visual regressions, such that any changes to the underlying components will be flagged immediately if they change any visuals - blocking merging until they get manually reviewed.


### Form management: Zod vs ArkType


Slash’s codebase is written entirely in Typescript, and we take advantage of that fact heavily by generating shared types across our codebase at build time. We wanted a form management solution that made it difficult for developers to shoot themselves in the foot, and took advantage of our extensive type safety.


We ended up choosing ArkType, since it was so strongly coupled with Typescript, we could prevent any amount of drift. We could take our generated types and couple them one-to-one with ArkType types using` satisfies` , such that if our underlying type changes, our codebase will throw type errors.


Here is an illustrative example:


```text
# models/entity/Address.yaml
title  :   Address
type  :   object
properties  :
addressLine  :
type  :   string
addressLine2  :
type  :   string
addressCity  :
type  :   string
addressState  :
type  :   string
addressCountry  :
type  :   string
minLength  :   2
maxLength  :   2
description  :   ISO 3166-1 Alpha-2 Country Code (e.g., US, FR, UK, DE, ...).
required  :
-   addressLine
-   addressCity
-   addressState
```


```text
// shemas/entity/Address.ts


// Schema implementation of an address model
import   type   { entity }   from   '@slashfi/models'  ;
import   {   type   Type, type }   from   'arktype'  ;


// Helper to configure default error messages
export   const   nonEmptyString   =   (  fieldName  :   string  )   =>
type  (  'string>0'  ).  configure  ({
message:   `${  fieldName  } is required.`  ,
});


export   const   Address   =   type  ({
addressLine:   nonEmptyString  (  'Address Line'  ),
addressLine2:   'string | undefined'  ,
addressCity:   nonEmptyString  (  'Address City'  ),
addressState:   nonEmptyString  (  'Address State'  ),
addressCountry:   type  (  'string==2 | undefined'  ).  configure  ({
message:   'Country code must be a 2-letter ISO 3166-1 Alpha-2 code'  ,
}),
})   satisfies   Type  <  entity  .  Address  >;
// entity.Address is built automatically from our YAML spec


export   const   defaultAddress  :   typeof   Address.infer   =   {   ...   }
```


```text
// Illustrative form component that submits an address - using Tanstack form
const   AddressForm   =   ()   =>   {
const   form   =   useAppForm  ({
defaultValues: defaultAddress,
// Validate the form input on mount and on change
validators: {
onMount: Address,
onChange: Address,
},
onSubmit  : ({   value   }  :   {   value  :   Address   })   =>   {
// Submit the value
console.  log  (value);
},
});


return   (
<  form.AppForm  >
<  form.Field   name  =  "addressLine"  >
{(  field  )   =>   (
<>
<  Input
value  =  {field.state.value}
onChange  =  {(  e  )   =>   field.  handleChange  (e.target.value)}
/>
{  /* Use built in metadata like isTouched */  }
{field.state.meta.isTouched   ?   (
<  p  >{  getMessage  (field.state.meta.errors)}</  p  >
)   :   null  }
</>
)}
</  form.Field  >


{  /* ... more inputs for each field in address */  }


{  /* Subscribe for reactive form update */  }
<  form.Subscribe
selector  =  {({   canSubmit  ,   isSubmitting   })   =>   ({
canSubmit,
isSubmitting,
})}
>
{({   canSubmit  ,   isSubmitting   })   =>   (
<  Button   loading  =  {isSubmitting}   disabled  =  {canSubmit}>
Create vendor
</  Button  >
)}
</  form.Subscribe  >
</  form.AppForm  >
);
};
```


While Zod and React Hook Form could have served us well, we wanted something that directly took advantage of TypeScript, and ArkType was the clear leader here. We chose Tanstack Form over React Hook Form due to its stricter Typescript adherence.


One thing you’ll notice with Tanstack form is how it forces you into a strict, opinionated form structure. While this introduces a learning curve, we felt it was worth it to enforce standardization in how we write forms across our codebase.


### Implementation: Feature flag and code branching


When it came to actually writing code for the Facelift, we wanted to avoid one enormous PR, since we were still shipping new features in the old design system alongside the facelift.


We used a[feature flag](https://blog.jetbrains.com/space/2022/06/16/feature-flags/) , and simply enabled the feature flag for ***our account only*** , allowing us to[dogfood](https://en.wikipedia.org/wiki/Eating_your_own_dog_food) our own changes. We also implemented a simple toolbar to toggle the facelift on and off, making it extremely easy to spot regressions by quickly toggling the facelift on and off.


The toggle we would use to quickly toggle the facelift on and off to catch regressions


As for the actual code branching, we aimed to not duplicate business logic code, doing so as such:


```text
const   CardsPage   =   ()   =>   {
const   {   isFacelift   }   =   useFaceliftToolbar  ();


// Existing business logic
const   cards   =   useCards  ()


if   (isFacelift) {
// New, facelifted components
return   (
<  CardsTableV2   cards  =  {cards} />
)
}


// Existing UI
return   (
<  CardsTable   cards  =  {cards} />
)
}
```


***The example above illustrates an ideal case*** - the reality of introducing massive amounts of new code into an existing codebase is much messier. For example: if you perform the` isFacelift` switch a high level (at the top page level, for example), you necessarily have to duplicate all the business logic for all branches underneath that component. But if you perform the check at the individual component level, you are beholden to the old markup logic of the parent, which often needs a facelift anyways.


What protected us from production issues was hiding all this functionality behind a feature flag so that for all actual customers,` isFacelift` was always` false` until we were ready. Internally, we would keep it turned on so we could spot regressions as they made it into the platform.


As the facelift became feature complete, we partnered with customers we had close relationships with and enabled it for them with the option to turn it off, asking for feedback and spotting regressions without blocking any existing workflows.


### Notes about execution


While the above section is laid out in a very ordered way, this was by no means the order of implementation. In reality, these decisions were made by trying things and iterating quickly. For example,


- My first draft of our CSS build script spammed the` @utilty` directive to create each custom classname we wanted - then Sam built on top of it by finding this[github discussion](https://github.com/tailwindlabs/tailwindcss/discussions/19020) which outlined the tailwind theme variable namespaces.
- There were 2 iterations of our searchable single select component using different BaseUI components before we settled on our current approach, using the[menu API](https://base-ui.com/react/components/menu) .


All this to say that we didn’t make these decisions as elegantly as the described tradeoffs above. The common thread is constant iteration without the paralysis of feeling like you need to get it perfect the first time.


## Part 3: Learnings


I personally learned a lot from a project where the scope encompassed the entire frontend. Our team learned a lot as well, especially as more engineers contributed to facelifting their respective products.


### “Raising the floor”


A major reason why tech debt accumulated in the first place was due to a lack of shared knowledge. At a young startup this isn’t a problem, as everyone works so closely together. But as a team grows and a product’s surface area expands, it becomes impossible to share assumptions and usage patterns as quickly as before.


Our philosophy for the facelift was to “raise the floor”. We want to make it difficult to write bad frontend code, which means documenting what good frontend code looks like and catching errors before they hit production. Storybook and Chromatic are critical for this, and are well worth the investment to set up. Every bit of effort you spend raising the floor will have compounding returns on the code quality across the codebase - with the inverse being true.


### Reality of long projects


The facelift from beginning to end took around 4 months of continuous work, with a dedicated “hack week” at the end that lasted 3 weeks and involved 6 other engineers contributing to the facelift (much love to them <3). I also had an excellent PM (Andy) who helped scope the entire project, communicate with Metacarbon, and provide feedback on new UX patterns.


Since the facelift was not a project I could release in parts, I didn’t get the joy of releasing product to customers for a very long time. I learned that a good approach is to context switch internally - sometimes I’d work on the core component library, then apply those core components to some full pages, and rotate between the two. This approach also helps you quickly catch edge cases in your core component usage, forcing you to go back and draw better lines of abstraction.


It can become demoralizing to refactor tons of code with no customer love to show for it. What kept me motivated was a couple of things:


1. Coming into work was still fun every day because of the people at Slash!
2. I could still show off the progress internally. Having some sort of dopamine from showing anyone the progress you’re making is a great way to stay motivated, especially if you love your craft.
3. This project was going to lay the foundation for our frontend codebase for hopefully a long time to come, so it was worth it to get it right.
4. Slash is growing and winning, which was the[OVERALL](https://www.slash.com/overall) motivation.


## Part 4: What’s next


With our facelift rollout well on its way, our main priority becomes catching edge cases and regressions. Luckily, we have a world class support team that helps our customers through this transition and surfaces issues at lightning speed.


If you’d like to check out our work, you can see it on our[demo site](https://demo.slash.com/platinum) , where you can also check out the features that Slash offers!


If you made it here and are a talented engineer looking to solve problems for real customers at a fast growing startup, apply[here](https://www.slash.com/company/careers) . We’d love to hear from you.


## Read more from us


### [One Engineer, Three Months, Zero Compromises](https://www.slash.com/engineering/blog/new-mobile-app)


≈ 17 minutes read


### [Building Card Transaction Processing at Slash](https://www.slash.com/engineering/blog/card-transaction-processing)


≈ 12 minutes read


### [Building Banking-Grade Stablecoin Rails at Slash](https://www.slash.com/engineering/blog/banking-grade-stablecoin-rails)


≈ 19 minutes read


### [My Internship with Slash](https://www.slash.com/engineering/blog/intern-blog)


≈ 13 minutes read


### [Winning & Culture](https://www.slash.com/engineering/blog/winning-and-culture)


≈ 4 minutes read


### [Scaling 1M lines of TypeScript: Registries](https://www.slash.com/engineering/blog/scaling-1m-lines-of-typescript-registries)


≈ 5 minutes read


### [Health Checks](https://www.slash.com/engineering/blog/health-checks)


≈ 7 minutes read


### [Type challenge: Datadog Logs Preview](https://www.slash.com/engineering/puzzles/datadog-preview-type-challenge)


≈ 2 minutes read
