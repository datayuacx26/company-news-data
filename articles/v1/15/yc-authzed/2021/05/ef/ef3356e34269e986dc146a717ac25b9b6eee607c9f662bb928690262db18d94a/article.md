---
schema_version: "1.0.0"
document_id: "ef3356e34269e986dc146a717ac25b9b6eee607c9f662bb928690262db18d94a"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/show-and-tell"
published_at: "2021-05-05T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:9aba4a2a4067c18f7dbfac6b0e254fa189b1cb12e11ab669c939fadb12383db6"
---

# Show() && Tell()

Human beings innately react to movement: Our eyes are finely tuned to changes in the environment around us, immediately focusing our attention on differences and making the details of the moving objects far more memorable.[Studies have shown](https://www.sciencedirect.com/science/article/abs/pii/S0360131516301336) that movement and animation are a great learning tool, far more useful in conveying information than simple static displays. Changes in display are also beneficial in conveying not only how something works, but how it got into a working state: developers often find[video tutorials](https://authzed.com/demo) helpful as much for the *process* of development as the final result.


## View source of inspiration


In my opinion, the[Stripe landing page](https://stripe.com/) is a great example of an informative landing page. In addition to providing a clear and concise message immediately about what Stripe does, it has two additional animated sections that really stand out: a rotating globe showing all payments made on their platform (see[To design and develop an interactive globe](https://stripe.com/blog/globe) for how it was made), and an *animated example* of how to use the Stripe API.


The animated example is of particular interest: While at first glance it appears to be simple, it provides immense value to readers on how the Stripe API is used and the value it primarily provides, all without having to *say* much at all.


At[Authzed](https://authzed.com/) , we are building an API-driven service, focused on providing clear and concise APIs around answering authorization and permissions questions for our customers' applications. We felt it would be insightful to those reaching our landing page to see a similar animated example to demonstrate our API; unfortunately, we were unable to find a sufficiently extensible, simple and open solution.


Being the developers we are, we built one ourselves and are open sourcing it, so that others can benefit.


## Framing the work


The first decision we made was which framework to use, if any. As we make heavy use of React and TypeScript in our frontend applications (see[Learning through play](https://authzed.com/blog/learning-through-play/) for the[Playground](https://play.authzed.com/) we built recently), we naturally chose to continue using them for the development of this component.


Our task was to set up a new React project that could be exposed as a component and included by other projects. Getting all the necessary dependencies and` package.json` settings correct for such an endeavour could be daunting, but fortunately we discovered[create-react-library](https://github.com/transitive-bullshit/create-react-library) .


` create-react-library` is similar to` create-react-app` , but provides a simple command for instantiating a React project to be used as an embedded component/library. Thus, we were able to start our project like so:


sh


1


2


```text
npm install -g create-react-library
npx create-react-library


```


sh


1


2


```text
npm install -g create-react-library
npx create-react-library


```


After entering the name of our project (` animated-code-example-component` ),` create-react-library` generated all the necessary components of our project, including commands for building, testing, publishing and *demoing* our new library. We were now ready to build the actual animated editor!


## Displaying code


The first and most important component of the animated example was the *code editor* : The goal of the animated component is to display an example of how an API is used in a manner that is immediately obvious to developers. While the animated example would not be itself user-editable, we quickly realized that in order for the code example to be as realistic as possible, we'd want to leverage all the other aspects of a real code editor: syntax highlighting, formatting, and perhaps even autocompletion in the future.


Therefore, we chose to make use of the[Monaco code editor](https://microsoft.github.io/monaco-editor/) , the editor behind VS Code, and the same editor we made use of in the[Authzed Playground](https://play.authzed.com/) . By using a *real* code editor, we would not have to reinvent all of the above functionality, at the cost of a slightly larger component and some small workarounds necessary to meet the needs of this component.


### Returning to Monaco


As we previously discussed in[Learning through play](https://authzed.com/blog/learning-through-play/) , making use of Monaco in React is fairly straightforward by use of the existing open source[React monaco component](https://github.com/react-monaco-editor/react-monaco-editor) .


To start, we added the editor component itself:


ts


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


18


19


20


21


22


23


24


25


26


27


28


29


30


```text
const   monaco =  useMonaco  ();
const   editorRef = useRef<monaco. editor  . IStandaloneCodeEditor   |  null  >( null  );
const   [isEditorReady, setEditorReady] =  useState  ( false  );


const    handleEditorMounted   = ( editor  : monaco.editor. IStandaloneCodeEditor   ) => {
editorRef. current   = editor;
setEditorReady  ( true  );
}


...
< Editor
value={props. script  . initialEditorContent  }
width={props. editorWidth   ??  600  }
height={props. editorHeight   ??  200  }
language={props. script  . editorLanguage  }
onMount={handleEditorMounted}
options={{
theme  : theme ===  'dark'   ?  'vs-dark'   :  'vs'  ,
padding  : {  top  :  '10px'   },
scrollbar  : {  handleMouseWheel  :  false  ,  vertical  :  'hidden'  ,  horizontal  :  'hidden'   },
minimap  : {
enabled  :  false
},
highlightActiveIndentGuide  :  false  ,
cursorStyle  :  'block-outline'  ,
overviewRulerBorder  :  false  ,
wordWrap  :  'on'  ,
renderLineHighlight  :  'none'
}}
/>


```


ts


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


18


19


20


21


22


23


24


25


26


27


28


29


30


```text
const   monaco =  useMonaco  ();
const   [isEditorReady, setEditorReady] =  useState  ( false  );


editorRef. current   = editor;
setEditorReady  ( true  );
}


...
< Editor
value={props. script  . initialEditorContent  }
width={props. editorWidth   ??  600  }
height={props. editorHeight   ??  200  }
language={props. script  . editorLanguage  }
onMount={handleEditorMounted}
options={{
theme  : theme ===  'dark'   ?  'vs-dark'   :  'vs'  ,
padding  : {  top  :  '10px'   },
minimap  : {
enabled  :  false
},
highlightActiveIndentGuide  :  false  ,
cursorStyle  :  'block-outline'  ,
overviewRulerBorder  :  false  ,
wordWrap  :  'on'  ,
renderLineHighlight  :  'none'
}}
/>


```


Make note of the` options` specified here: These options disable various UI elements to prevent certain classes of user interaction and to simplify the display. Of particular importance is` renderLineHighlight` , as the component *simulates* the cursor, and thus the "active" line does not match our cursor as displayed.


### Animating the code


The key aspect of our component is that of *animation* : The code being displayed cannot simply appear, but rather needs to be animated in such a way that allows those watching to "follow along" as the example is built up. The code would therefore have to be added into Monaco in chunks.


Fortunately, Monaco makes executing these kinds of changes within the editor easy via the` executeEdits` function:


ts


1


2


3


4


5


6


7


8


9


10


11


12


13


```text
const   range =  new   monaco!. Range  (
lineNumber,
columnIndex,
lineNumber,
columnIndex
);
const   op = {
identifier  : id,
range  : range,
text  :  "texttoinsert"  ,
forceMoveMarkers  :  true  ,
};
editorRef. current  !. executeEdits  ( "animated-code-example"  , [op]);


```


ts


1


2


3


4


5


6


7


8


9


10


11


12


13


```text
const   range =  new   monaco!. Range  (
lineNumber,
columnIndex,
lineNumber,
columnIndex
);
const   op = {
identifier  : id,
range  : range,
text  :  "texttoinsert"  ,
forceMoveMarkers  :  true  ,
};
editorRef. current  !. executeEdits  ( "animated-code-example"  , [op]);


```


Here, an insertion command is used to insert the text at the specified` range` , which displays it immediately within the editor's existing text.


### Simulating the cursor


While using` executeEdits` made animating the code being "inputted" easy, it came with one large downside: Displaying the cursor. Since we decided to handle the editing of the Monaco editor ourselves, the editor will never receive focus and will therefore not display the cursor while the changes are being made.


Thus, to make it appear as if a real developer is entering the code within the component, we needed to **simulate** the cursor for the developer. Fortunately, this was fairly straightforward to achieve in Monaco via the use of[line decorations](https://microsoft.github.io/monaco-editor/api/interfaces/monaco.editor.imodeldecorationoptions.html) , which allow for attaching arbitrary styles to the text displayed within.


To begin, we defined a CSS class that simulates a cursor displayed after any decorated text:


css


1


2


3


4


5


6


7


8


9


10


```text
.fake-cursor  :after   {
content  :  " "  ;
background-color  : black;
height  :  1em  ;
width  :  2px  ;
position  : absolute;
top  :  2px  ;
margin-left  : - 2px  ;
display  : inline-block;
}


```


css


1


2


3


4


5


6


7


8


9


10


```text
.fake-cursor  :after   {
content  :  " "  ;
background-color  : black;
height  :  1em  ;
width  :  2px  ;
position  : absolute;
top  :  2px  ;
margin-left  : - 2px  ;
display  : inline-block;
}


```


We then needed to make sure to decorate the last character (and *only* the last character) that was inserted into the editor. This was accomplished by use of the[deltaDecorations](https://microsoft.github.io/monaco-editor/api/interfaces/monaco.editor.icodeeditor.html#deltadecorations) call to Monaco, which takes in a list of decorations to remove, as well as a list of decorations to add. We were therefore able to remove all previously added decorations (since we only want one), and add the new decoration onto newly added character:


ts


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


18


```text
const   newDecorations = [
{
range  :  new   monaco. Range  (
lineNumber,
columnIndex,
lineNumber,
columnIndex + insertedText. length
),
options  : {
inlineClassName  :  "fake-cursor"  ,
stickiness  :
monaco. editor  . TrackedRangeStickiness  . NeverGrowsWhenTypingAtEdges  ,
},
},
];


// Note: we send in the existing decorations for removal.
decorations = editorRef. current  !. deltaDecorations  (decorations, newDecorations);


```


ts


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


18


```text
const   newDecorations = [
{
range  :  new   monaco. Range  (
lineNumber,
columnIndex,
lineNumber,
columnIndex + insertedText. length
),
options  : {
inlineClassName  :  "fake-cursor"  ,
stickiness  :
monaco. editor  . TrackedRangeStickiness  . NeverGrowsWhenTypingAtEdges  ,
},
},
];


// Note: we send in the existing decorations for removal.


```


## Scripting the animation


Now that we had a means for inserting text and for displaying a fake cursor along with those insertions, the next step was for us to provide a way for defining the code to be inserted.


To produce the calls for editing the text within the editor (and other aspects of the animation), we defined the concept of a[DemoScript](https://github.com/authzed/animated-code-example-component/blob/main/src/index.tsx#L93) , which would allow anyone embedding the component to define the various steps of the animation:


ts


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


```text
const    script  :  DemoScript   = {
initialEditorContent  :  ``  ,
editorLanguage  :  "python"  ,
steps  : [
{  kind  :  DemoStepKind  . INSERT_TEXT  ,  target  :  StepTarget  . EDITOR  ,  value  :  "p"   },
{  kind  :  DemoStepKind  . INSERT_TEXT  ,  target  :  StepTarget  . EDITOR  ,  value  :  "r"   },
{  kind  :  DemoStepKind  . INSERT_TEXT  ,  target  :  StepTarget  . EDITOR  ,  value  :  "i"   },
{  kind  :  DemoStepKind  . INSERT_TEXT  ,  target  :  StepTarget  . EDITOR  ,  value  :  "n"   },
{  kind  :  DemoStepKind  . INSERT_TEXT  ,  target  :  StepTarget  . EDITOR  ,  value  :  "t"   },
{  kind  :  DemoStepKind  . INSERT_TEXT  ,  target  :  StepTarget  . EDITOR  ,  value  :  "("   },
{  kind  :  DemoStepKind  . INSERT_TEXT  ,  target  :  StepTarget  . EDITOR  ,  value  :  '"'   },
{  kind  :  DemoStepKind  . INSERT_TEXT  ,  target  :  StepTarget  . EDITOR  ,  value  :  "h"   },
{  kind  :  DemoStepKind  . INSERT_TEXT  ,  target  :  StepTarget  . EDITOR  ,  value  :  "e"   },
],
};


```


ts


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


```text
const    script  :  DemoScript   = {
initialEditorContent  :  ``  ,
editorLanguage  :  "python"  ,
steps  : [
],
};


```


Producing this script manually is fairly laborious, however, so we defined a function (` stepsForText` ) for converting a simple text string into a series of insertion steps:


ts


1


```text
steps  : [... stepsForText  ( "print('Hello World!')"  )];


```


ts


1


```text
steps  : [... stepsForText  ( "print('Hello World!')"  )];


```


## Human-like typing detected


With the above script now written, we could now see a live demo of the code within the editor:


However, the animated entering of code felt mechanical, due to its continuous process and lack of randomness: humans don't type code into keyboards at the same rate for all characters, even when they are touch typing. Rather, humans tend to pause on certain characters (such as spaces and newlines) because they have to move their hands to accommodate the locations on the keyboard (spacebar, enter key, etc).


Therefore, in order to closer approximate the entering of code by a person, we needed to add delays into the animation for specific characters.


First, we declared a new interface called` Delays` :


ts


1


2


3


4


```text
interface    Delays   {
startDelay  ?:  number   |  undefined  ;
endDelay  ?:  number   |  undefined  ;
}


```


ts


1


2


3


4


```text
interface    Delays   {
startDelay  ?:  number   |  undefined  ;
endDelay  ?:  number   |  undefined  ;
}


```


Next, we created a map to return the delays associated with specific keycodes found in the` DemoScript` :


ts


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


```text
const    DELAY_CHARACTERS  :  Record  < string  ,  Delays  > = {
" "  : {
endDelay  :  110  ,
},
"\n"  : {
endDelay  :  60  ,
},
"("  : {
startDelay  :  80  ,
endDelay  :  60  ,
},
")"  : {
startDelay  :  80  ,
endDelay  :  60  ,
},
};


```


ts


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


```text
const    DELAY_CHARACTERS  :  Record  < string  ,  Delays  > = {
" "  : {
endDelay  :  110  ,
},
"\n"  : {
endDelay  :  60  ,
},
"("  : {
startDelay  :  80  ,
endDelay  :  60  ,
},
")"  : {
startDelay  :  80  ,
endDelay  :  60  ,
},
};


```


By setting different start and end delays associated with specific characters, the entered text started to take on a more "natural" feel:


In addition to delays on the whitespace insertion, we also added a special delay for text marked as a "paste": Developers often copy/paste items (such as tokens), and there is typically a delay before the paste (from the developer switching contexts), followed by the text being inserted immediately as a single chunk, and then a smaller delay as the developer switches back to touch typing from Cmd/Ctrl-V. Therefore, paste operations are given special delays:


ts


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


```text
const    PASTE_DELAYS  :  Delays   = {
startDelay  :  300  ,
endDelay  :  100  ,
}


...
const    script  :  DemoScript   = {
initialEditorContent  :  ``  ,
initialReplContent  :  ''  ,
initialBrowserContent  :  'Hello World!'  ,
editorLanguage  :  'python'  ,
steps  : [
... stepsForText  ( `print('`  ,  StepTarget  . EDITOR  ,  1  ,  1  ),
{  kind  :  DemoStepKind  . INSERT_PASTED_TEXT  ,  target  :  StepTarget  . EDITOR  ,  value  :  'Some pasted value'   },
... stepsForText  ( `')`  ,  StepTarget  . EDITOR  ),
]
}


```


ts


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


```text
const    PASTE_DELAYS  :  Delays   = {
startDelay  :  300  ,
endDelay  :  100  ,
}


...
const    script  :  DemoScript   = {
initialEditorContent  :  ``  ,
initialReplContent  :  ''  ,
initialBrowserContent  :  'Hello World!'  ,
editorLanguage  :  'python'  ,
steps  : [
... stepsForText  ( `print('`  ,  StepTarget  . EDITOR  ,  1  ,  1  ),
... stepsForText  ( `')`  ,  StepTarget  . EDITOR  ),
]
}


```


To make this even easier to script, we chose to have` stepsForText` support delimiters for marking sections of pasted code. By default, we make use of` \['«', '»'\]` , but they can be overridden:


ts


1


```text
... stepsForText  ( `client = authzed.Client('«tc_my_client_token»')\n`  ,  StepTarget  . EDITOR  ,  1  ,  1  ),


```


ts


1


```text


```


With the addition of these delays and other small timing adjustments, we can now see animation that much more closely matches the typing of a real person


## REPL


With the editor complete, we could now move onto the simulated REPL. The simulated REPL was, fortunately, a far simpler component: a simple` <div>` that is bound to a React` useState` , which is updated via the same human-like typing code used above:


ts


1


2


3


```text
const   [replContent, setReplContent] =  useState  (props. script  . initialReplContent  );


< pre    className  = {clsx(  " repl  ")}>  $ {replContent} </ pre  >


```


ts


1


2


3


```text


< pre    className  = {clsx(  " repl  ")}>  $ {replContent} </ pre  >


```


The change to "type" into the REPL is accomplished by changing the` target` to` StepTarget.REPL` .


## Browser


The Stripe animated code example displays two panes of information: A simulated code editor, and a simulated shell, for executing the "entered" program. For Authzed, we needed a *third* component: That of a simulated web browser, to display the website both before and after Authzed permissions checking was added.


Fortunately for us, here we were able to take advantage of an open source component: A simulated browser GUI found at[https://codepen.io/4esnog/pen/PNmvNO](https://codepen.io/4esnog/pen/PNmvNO) . This CSS-only solution provided a basic "view" of a web browser, in which we could change the contents dynamically.


To make use of this component, we first added the CSS found in the example to our` index.css` file. Then, within the component, we added the browser display:


ts


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


```text
const   [browserContent, setBrowserContent] =  useState  (props. script  . initialBrowserContent  );


...


<div className={ clsx  ( Styles  [ "browser"  ],  Styles  [ "target"  ], { [ Styles  [ "active"  ]]: target ===  StepTarget  . BROWSER   })}>
< div    className  = {Styles[  " browser-navigation-bar  "]}>
< i  >  </ i  >  < i  >  </ i  >  < i  >  </ i  >
< input    value  = {props.browserDisplayedUrl   ?? ' https:  // example.com  '}  disabled   />
</ div  >


< div    className  = {Styles[  " browser-container  "]}  style  = {{    padding:   " 6px  " }}>
{browserContent !== '' && browserContent}
{browserContent === '' &&  < div    className  = {Styles[  " loader  "]} />  }
</ div  >
</div>
</div>


```


ts


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


```text


...


< div    className  = {Styles[  " browser-navigation-bar  "]}>
< i  >  </ i  >  < i  >  </ i  >  < i  >  </ i  >
</ div  >


{browserContent !== '' && browserContent}
{browserContent === '' &&  < div    className  = {Styles[  " loader  "]} />  }
</ div  >
</div>
</div>


```


The contents of the browser are set using a simple` useState` , and to make the browser feel more real, if the contents are set to` ''` , we display a CSS-based loader within the browser window to indicate content is being loaded.


## Starting the animation on scroll


The final piece of the puzzle was the ability to only start the animated component when it became visible within our landing page: we did not want to require users to have to hit a "Start" or "Run" button of some kind, and starting the animation before the component became visible would mean the user could potentially start viewing the animation in the middle of (or even after) its run.


Fortunately, within the vast toolkit of capabilities provided by modern browsers is[IntersectionObserver](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API) , which allows for programmatic callback when a DOM element becomes visible (or invisible) on a page due to a change in scroll position. Further fortunate for us, there exists a React wrapper library named[react-intersection-observer](https://github.com/thebuilder/react-intersection-observer) around` IntersectionObserver` , which allows for easy use of IntersectionObserver via a React hook:


ts


1


2


3


4


5


6


7


8


9


10


11


12


13


14


```text
import   { useInView }  from    'react-intersection-observer'  ;


const   { inView, ref } =  useInView  ({
threshold  :  0.8  ,
triggerOnce  :  true  ,
});


...


return    < div    ref  = {ref}  >
< div    className  = "animated-preview"  >
...
</ div  >
</ div  >


```


ts


1


2


3


4


5


6


7


8


9


10


11


12


13


14


```text
import   { useInView }  from    'react-intersection-observer'  ;


const   { inView, ref } =  useInView  ({
threshold  :  0.8  ,
triggerOnce  :  true  ,
});


...


return    < div    ref  = {ref}  >
< div    className  = "animated-preview"  >
...
</ div  >
</ div  >


```


With our root` <div>` bound to` ref` and our` threshold` set to` 0.8` , the` inView` state will only become` true` if and when the component is mostly visible on the page; further, by setting` triggerOnce` to` true` , it will only update the state of the component a single time when the component becomes visible, ensuring that we won't restart the animation if the user scrolls around.


Since we now had a state variable for visibility, our final task was to start the animation when` inView` became` true` , which could be easily accomplished via the use of a` useEffect` :


ts


1


2


3


4


5


6


7


8


9


10


11


12


13


```text
useEffect  ( () =>   {
const    runScript   = (  ) => {
( async   () => {
await    sleep  ( 300  );


... run the animation script ...
})();
};


if   (monaco && isEditorReady && inView) {
runScript  ();
}
}, [monaco, isEditorReady, inView, props. script  ]);


```


ts


1


2


3


4


5


6


7


8


9


10


11


12


13


```text
useEffect  ( () =>   {
const    runScript   = (  ) => {
( async   () => {
await    sleep  ( 300  );


... run the animation script ...
})();
};


if   (monaco && isEditorReady && inView) {
runScript  ();
}
}, [monaco, isEditorReady, inView, props. script  ]);


```


## Now available open source


And that was it! We now had a fully working, animated code example for the Authzed landing page!


We're happy to announce that the AnimatedCodeExample component is available now at[https://github.com/authzed/animated-code-example-component](https://github.com/authzed/animated-code-example-component) and in npm as[@authzed/animated-code-example-component](https://www.npmjs.com/package/@authzed/animated-code-example-component) :


sh


1


```text
npm install @authzed/animated-code-example-component


```


sh


1


```text
npm install @authzed/animated-code-example-component


```


or


sh


1


```text
yarn add @authzed/animated-code-example-component


```


sh


1


```text
yarn add @authzed/animated-code-example-component


```


Special thanks to the Stripe team for demonstrating the value of such a component. We hope the community finds it useful and please reach out to us with your animated components! We’d love to see the component in the wild and incorporate any improvements we can.


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- View source of inspiration
- Framing the work
- Displaying code
- Returning to Monaco
- Animating the code
- Simulating the cursor
- Scripting the animation
- Human-like typing detected
- REPL
- Browser
- Starting the animation on scroll
- Now available open source
- Additional Reading


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
