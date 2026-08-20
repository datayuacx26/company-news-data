---
schema_version: "1.0.0"
document_id: "83ac75736334802567a1ba7a5409312eee6054519e4e80099495f30cbc3e9a08"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-use-framer-ai-to-make-a-right-click-to-download-logo-menu/"
published_at: "2025-06-05T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:57:27.560850+00:00"
content_hash: "sha256:0aa1ed1e5ee85d0e213132d16167c487c4b2c4c0bf2929a4a69a35942977a4ca"
---

# How to use Framer AI to make a right click to download logo menu

Realized that we hadn’t added back the right click to download menu to the Basedash website since our rebrand and relaunch earlier this year.


Saw this post by Felix from Lovable and decided to spend about 10 minutes in Framer seeing if I could make it happen.


[https://x.com/felixhhaas/status/1930344241328467982](https://x.com/felixhhaas/status/1930344241328467982)


Worked out pretty well, not that hard to do TBH.


Here’s the video of me making over some smooth smooth jazz:


Not too hard, pretty happy with the results.


Here’s the component that I ended up with if you want to use it yourself:


```text
// Right-click to download header with dark mode menu and color props for menu background, text, and hover
import   { useState, useRef, useEffect, startTransition,   type   CSSProperties }   from   "react"
import   { addPropertyControls, ControlType, RenderTarget }   from   "framer"


const   defaultSVG   =   "https://framerusercontent.com/images/GfGkADagM4KEibNcIiRUWlfrR0.svg"
const   defaultPNG   =   "https://framerusercontent.com/images/GfGkADagM4KEibNcIiRUWlfrR0.png"
const   defaultFull   =   "https://framerusercontent.com/images/aNsAT3jCvt4zglbWCUoFe33Q.png"


interface   DownloadHeaderProps   {
darkBrandFile  :   string
lightBrandFile  :   string
iconFile  :   string
fullBrandFile  :   string
brandPackFile  :   string
menuOptions  :   Array  <{   label  :   string  ;   type  :   string  ;   icon  ?:   string   }>
backgroundColor  :   string
menuBgColor  :   string
menuTextColor  :   string
menuHoverBgColor  :   string
style  ?:   CSSProperties
}


/**
* Right-click Download Header (Dark Menu)
*
*   @framerIntrinsicWidth   600
*   @framerIntrinsicHeight   120
*
*   @framerSupportedLayoutWidth   any-prefer-fixed
*   @framerSupportedLayoutHeight   fixed
*/
export   default   function   DownloadHeader  (  props  :   DownloadHeaderProps  ) {
const   {
darkBrandFile   =   defaultSVG,
lightBrandFile   =   defaultPNG,
iconFile   =   defaultFull,
fullBrandFile   =   defaultFull,
brandPackFile   =   "https://framerusercontent.com/assets/brand-pack.zip"  ,
menuOptions   =   [
{ label:   "Download dark brand mark"  , type:   "dark"   },
{ label:   "Download light brand mark"  , type:   "light"   },
{ label:   "Download icon"  , type:   "icon"   },
{ label:   "Download full brand mark"  , type:   "full"   },
{ label:   "Download brand pack (zip)"  , type:   "pack"   },
],
backgroundColor  ,
menuBgColor  ,
menuTextColor  ,
menuHoverBgColor  ,
style  ,
}   =   props


const   [  menuOpen  ,   setMenuOpen  ]   =   useState  (  false  )
const   [  menuPos  ,   setMenuPos  ]   =   useState  ({ x:   0  , y:   0   })
const   containerRef   =   useRef  <  HTMLDivElement  >(  null  )


useEffect  (()   =>   {
function   handleClick  (  e  :   MouseEvent  ) {
if   (containerRef.current   &&   !  containerRef.current.  contains  (e.target   as   Node  )) {
startTransition  (()   =>   setMenuOpen  (  false  ))
}
}
if   (menuOpen) {
window.  addEventListener  (  "mousedown"  , handleClick)
}
return   ()   =>   window.  removeEventListener  (  "mousedown"  , handleClick)
}, [menuOpen])


function   handleContextMenu  (  e  ) {
e.  preventDefault  ()
startTransition  (()   =>   {
setMenuOpen  (  true  )
setMenuPos  ({ x: e.clientX, y: e.clientY })
})
}


function   handleMenuClick  (  type  :   string  ) {
let   url   =   ""
let   filename   =   ""
if   (type   ===   "dark"  ) {
url   =   darkBrandFile
filename   =   "brand-dark.svg"
}   else   if   (type   ===   "light"  ) {
url   =   lightBrandFile
filename   =   "brand-light.svg"
}   else   if   (type   ===   "icon"  ) {
url   =   iconFile
filename   =   "brand-icon.svg"
}   else   if   (type   ===   "full"  ) {
url   =   fullBrandFile
filename   =   "brand-full.svg"
}   else   if   (type   ===   "pack"  ) {
url   =   brandPackFile
filename   =   "brand-pack.zip"
}
if   (url) {
const   link   =   document.  createElement  (  "a"  )
link.href   =   url
link.download   =   filename
document.body.  appendChild  (link)
link.  click  ()
document.body.  removeChild  (link)
}
startTransition  (()   =>   setMenuOpen  (  false  ))
}


// Show fullBrandFile as background image
const   headerStyle  :   CSSProperties   =   {
...  style,
width:   "100%"  ,
height:   "100%"  ,
backgroundColor: backgroundColor,
backgroundImage:   `url(${  fullBrandFile  })`  ,
backgroundRepeat:   "no-repeat"  ,
backgroundPosition:   "center center"  ,
backgroundSize:   "auto 80%"  ,
borderRadius:   8  ,
boxShadow:   "0 2px 8px rgba(0,0,0,0.06)"  ,
display:   "flex"  ,
alignItems:   "center"  ,
position:   "relative"  ,
userSelect:   "none"  ,


padding:   "0 8%"  ,
fontSize:   28  ,
fontWeight:   700  ,
color:   "#222"  ,
overflow:   "visible"  ,
}


return   (
<  div
ref  =  {containerRef}
style  =  {headerStyle}
onContextMenu  =  {handleContextMenu}
tabIndex  =  {  0  }
aria-label  =  "Header with download menu"
>
{menuOpen   &&   RenderTarget.  current  ()   !==   RenderTarget.thumbnail   &&   (
<  div
style  =  {{
position:   "fixed"  ,
top: menuPos.y,
left: menuPos.x,
background: menuBgColor,
borderRadius:   8  ,
boxShadow:   "0 4px 16px rgba(0,0,0,0.32)"  ,
minWidth:   220  ,
zIndex:   9999  ,
padding:   8  ,
display:   "flex"  ,
flexDirection:   "column"  ,
gap:   2  ,
}}
role  =  "menu"
>
{menuOptions.  map  ((  opt  ,   i  )   =>   (
<  MenuButton
key  =  {opt.type   +   i}
label  =  {opt.label}
icon  =  {opt.icon}
onClick  =  {()   =>   handleMenuClick  (opt.type)}
textColor  =  {menuTextColor}
hoverBg  =  {menuHoverBgColor}
/>
))}
</  div  >
)}
</  div  >
)
}


function   MenuButton  ({   label  ,   icon  ,   onClick  ,   textColor  ,   hoverBg   }) {
const   [  hover  ,   setHover  ]   =   useState  (  false  )
return   (
<  button
style  =  {{
background: hover   ?   hoverBg   :   "none"  ,
border:   "none"  ,
textAlign:   "left"  ,
padding:   "10px 16px"  ,
borderRadius:   6  ,
fontSize:   16  ,
color: textColor,
cursor:   "pointer"  ,
transition:   "background 0.05s"  ,
outline:   "none"  ,
}}
onMouseEnter  =  {()   =>   setHover  (  true  )}
onMouseLeave  =  {()   =>   setHover  (  false  )}
onClick  =  {onClick}
role  =  "menuitem"
>
{icon   &&   (
<  img   src  =  {icon}   alt  =  ""   style  =  {{ width:   20  , height:   20  , marginRight:   10  , verticalAlign:   "middle"   }} />
)}
{label}
</  button  >
)
}


addPropertyControls  (DownloadHeader, {
darkBrandFile: {
type: ControlType.File,
allowedFileTypes: [  "svg"  ,   "png"  ],
title:   "Dark Brand Mark"  ,
},
lightBrandFile: {
type: ControlType.File,
allowedFileTypes: [  "svg"  ,   "png"  ],
title:   "Light Brand Mark"  ,
},
iconFile: {
type: ControlType.File,
allowedFileTypes: [  "svg"  ,   "png"  ],
title:   "Icon"  ,
},
fullBrandFile: {
type: ControlType.File,
allowedFileTypes: [  "svg"  ,   "png"  ],
title:   "Full Brand Mark"  ,
},
brandPackFile: {
type: ControlType.File,
allowedFileTypes: [  "zip"  ],
title:   "Brand Pack (zip)"  ,
},
menuOptions: {
type: ControlType.Array,
title:   "Menu Options"  ,
control: {
type: ControlType.Object,
controls: {
label: { type: ControlType.String, defaultValue:   "Download dark brand mark"   },
type: {
type: ControlType.Enum,
options: [  "dark"  ,   "light"  ,   "icon"  ,   "full"  ,   "pack"  ],
optionTitles: [  "Dark Brand"  ,   "Light Brand"  ,   "Icon"  ,   "Full Brand"  ,   "Brand Pack (zip)"  ],
defaultValue:   "dark"  ,
},
icon: {
type: ControlType.File,
allowedFileTypes: [  "svg"  ,   "png"  ,   "jpg"  ,   "jpeg"  ,   "webp"  ],
title:   "Icon"  ,
},
},
},
defaultValue: [
{ label:   "Download dark brand mark"  , type:   "dark"   },
{ label:   "Download light brand mark"  , type:   "light"   },
{ label:   "Download icon"  , type:   "icon"   },
{ label:   "Download full brand mark"  , type:   "full"   },
{ label:   "Download brand pack (zip)"  , type:   "pack"   },
],
maxCount:   5  ,
},
backgroundColor: {
type: ControlType.Color,
title:   "Background"  ,
defaultValue:   "#FFFFFF"  ,
},
menuBgColor: {
type: ControlType.Color,
title:   "Menu BG"  ,
defaultValue:   "#181A1B"  ,
},
menuTextColor: {
type: ControlType.Color,
title:   "Menu Text"  ,
defaultValue:   "#F5F5F5"  ,
},
menuHoverBgColor: {
type: ControlType.Color,
title:   "Menu Hover BG"  ,
defaultValue:   "#23272A"  ,
},
})
```
