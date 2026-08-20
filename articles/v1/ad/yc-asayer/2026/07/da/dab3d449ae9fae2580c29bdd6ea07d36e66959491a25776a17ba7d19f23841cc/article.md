---
schema_version: "1.0.0"
document_id: "dab3d449ae9fae2580c29bdd6ea07d36e66959491a25776a17ba7d19f23841cc"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/reusable-form-controls-web-components/"
published_at: "2026-07-11T00:00:00+00:00"
first_seen_at: "2026-07-25T18:02:25.733496+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:1aba8aab6a6532aef8caecb4373fe28b602d2730d9dabf242521173138f265d0"
---

# Build Reusable Form Controls with Web Components

A custom element that wraps an` <input>` inside Shadow DOM is invisible to its parent` <form>` : its value never lands in` FormData` , native validation skips it, and a reset leaves it untouched. The fix is two lines —` static formAssociated = true` plus` this.attachInternals()` in the constructor — and the` ElementInternals` API that those lines unlock. This article builds one complete` <text-field>` control that submits, validates, labels, resets, and restores like a native input, using only the raw platform API.


With the release of Safari 16.4 in March 2023,[web components reached a milestone in their capability to interact with the form element using the ElementInternals API](https://dev.to/stuffbreaker/custom-forms-with-web-components-and-elementinternals-4jaj) ; prior to this, input elements located within a Shadow DOM were not discoverable by forms, which meant they would not validate on submission and their data would not be included in the FormData object. That support question is now settled, so the article skips the “can I use this yet” hand-wringing and focuses on the full recipe none of the older tutorials assemble end to end.


## Key Takeaways


- Two lines turn any autonomous custom element into a form control:` static formAssociated = true` declares intent, and` this.attachInternals()` returns the` ElementInternals` object that connects your element to its parent form.
- As of September 2025, form-associated custom elements are Baseline Widely Available — supported in Chrome since 77, Edge since 79, Firefox since 98, and Safari since 16.4 — so the old “is this supported yet?” caveat no longer applies.
- A control’s value only reaches` FormData` when you call` internals.setFormValue(value)` ; passing` null` drops the element out of submission entirely.
- ` setValidity()` blocks submission and applies` :invalid` , but accessible validation also requires a visible,` aria-describedby` -linked message — otherwise the form silently refuses to submit.
- Of the two state-restore modes the spec defines, only` "restore"` is reliably fired in shipping browsers;` "autocomplete"` is specified but not reliably implemented.


## Why Shadow DOM breaks native form participation


A native` <input>` participates in a form only when it descends from the` <form>` in the same DOM tree. Shadow DOM is a separate tree, so an input rendered inside a custom element’s shadow root is structurally invisible to the form: the browser never adds it to` form.elements` , never collects its value into` FormData` , and never runs constraint validation against it. This is the encapsulation tradeoff working as designed — and the exact problem form-associated custom elements (FACE) exist to solve.


The platform answer is` ElementInternals` .[The HTMLElement.attachInternals() method returns an ElementInternals object](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/attachInternals) ; this method allows a custom element to participate in HTML forms, and the interface provides utilities for working with these elements the same way you would work with any standard HTML form element, while also exposing the Accessibility Object Model to the element. FACE is no longer a frontier feature: form-associated custom elements have been[Baseline Widely Available since 2025-09-27](https://web-platform-dx.github.io/web-features-explorer/features/form-associated-custom-elements/) , supported in Chrome 77 (2019), Edge 79 (2020), Firefox 98 (2022), and Safari 16.4, released 2023-03-27. A community[element-internals-polyfill](https://www.npmjs.com/package/element-internals-polyfill) still exists for legacy engines, but with Baseline support across evergreen browsers, most projects no longer need it.


One hard constraint:[adding a static formAssociated property with a true value makes an autonomous custom element a form-associated custom element](https://html.spec.whatwg.org/dev/custom-elements.html) . The element must extend` HTMLElement` directly — not a built-in subclass — and` attachInternals()` throws a` NotSupportedError` if the element is not a custom element.


## The minimal form-associated custom element


The smallest useful control declares` formAssociated` , grabs its internals, and pushes its value into the form through a setter. Everything else builds on this skeleton.


```text
class   TextField   extends   HTMLElement   {
static   formAssociated   =   true  ;
static   observedAttributes   =   [  '  value  '  ,   '  required  '  ];


#internals;
#value   =   ''  ;


constructor  () {
super  ();
this  .  #internals   =   this  .  attachInternals  ();
}


get   value  () {   return   this  .  #value  ; }
set   value  (  v  ) {
this  .  #value   =   v   ??   ''  ;
this  .  #internals  .  setFormValue  (  this  .  #value  );
}


get   form  () {   return   this  .  #internals  .  form  ; }
get   name  () {   return   this  .  getAttribute  (  '  name  '  ); }
get   type  () {   return   this  .  localName  ; }
}


customElements  .  define  (  '  text-field  '  ,   TextField  );
```


` setFormValue()` is the load-bearing call here. A control’s value only reaches` FormData` when you invoke` internals.setFormValue(value)` ; pass` null` and the element drops out of submission entirely. The setter pattern guarantees that every assignment to` .value` — from script, an attribute, or user input — re-syncs the submitted value. Once associated, the element behaves like a built-in:[the form’s elements collection](https://developer.mozilla.org/en-US/docs/Web/API/HTMLFormElement/elements) includes all` <button>` ,` <fieldset>` ,` <input>` ,` <object>` ,` <output>` ,` <select>` ,` <textarea>` , and form-associated custom elements associated with the form.


If you only need to inject *arbitrary* data into a submission — not build a reusable control — the lighter-weight` formdata` event is enough: listen for it on the form and call` event.formData.append(...)` . Reach for` ElementInternals` when you’re building a control that must validate, label, reset, and restore.


## Render and sync the internal input


The control needs an internal` <input>` whose value flows back out through the public setter, and whose` input` /` change` events re-dispatch from the host so listeners on` <text-field>` behave as they would on a native field. Because a shadow root can only be attached once, the setup is guarded so a disconnect-then-reconnect doesn’t call` attachShadow()` a second time (which would throw):


```text
connectedCallback  () {
if (  !  this  .  shadowRoot  ) {
const   root   =   this  .  attachShadow  ({   mode  :   '  open  '  ,   delegatesFocus  :   true   });
root  .  innerHTML   =   `  <input part="input" />  `  ;
this  .  input   =   root  .  querySelector  (  '  input  '  );


this  .  input  .  addEventListener  (  '  input  '  , ()   =>   {
this  .  value   =   this  .  input  .  value  ;          // re-syncs setFormValue
this  .  #validate  ();                        // real-time validity
});
this  .  input  .  addEventListener  (  '  change  '  , (  e  )   =>   {
this  .  dispatchEvent  (  new   e  .  constructor  (  e  .  type  ,   e  ));
});
}


this  .  value   =   this  .  getAttribute  (  '  value  '  )   ??   ''  ;
this  .  #validate  ();
}
```


Native` change` events don’t cross the shadow boundary, so cloning the event with` new e.constructor(e.type, e)` and re-dispatching it from the host re-emits it in the light DOM where parent code can hear it. Driving` this.value` from the input’s` input` listener keeps the submitted` FormData` value current on every keystroke.


## Labeling, focus delegation, and tabindex


A native` <label for="...">` should focus the control when clicked. For a form-associated custom element, that wiring comes from` delegatesFocus: true` on the shadow root. Setting[delegatesFocus: true](https://developer.mozilla.org/en-US/docs/Web/API/Element/attachShadow) is what makes a` <label for>` click move focus into your internal input, exactly like a native field — without it, clicking the label does nothing, and focusing the host doesn’t reach the input. Because a FACE is a labelable element, you write the markup exactly as you would for a native input:


```text
<  form  >
<  label   for  =  "  email  "  >  Email  </  label  >
<  text-field   id  =  "  email  "   name  =  "  email  "   required  ></  text-field  >
</  form  >
```


Screen-reader label association for custom controls has historically been inconsistent across browser and assistive-technology combinations, so test against your target AT and keep an in-shadow` <label>` fallback if you support a broad matrix. With` delegatesFocus` set, you generally don’t need to manage` tabindex` manually — focus delegates to the internal input, which is already in the tab order.


## Validation with setValidity() and the full ValidityStateFlags


` ElementInternals.setValidity()` mirrors the native constraint-validation system. You pass a` ValidityStateFlags` object, an optional human-readable message, and an optional anchor element; the browser then applies the` :invalid` pseudo-class and blocks submission. With this setup,[the :invalid pseudo-class automatically applies to the element](https://webkit.org/blog/13711/elementinternals-and-form-associated-custom-elements/) whenever a flag is set. Passing an empty object` {}` clears all errors.


The flags map one-to-one onto native validity reasons:


Flag Native trigger analog


` valueMissing`` required` with no value


` typeMismatch` value wrong for` type` (e.g.` email` )


` patternMismatch` value fails` pattern`


` tooLong` /` tooShort` exceeds` maxlength` / under` minlength`


` rangeUnderflow` /` rangeOverflow` below` min` / above` max`


` stepMismatch` value not aligned to` step`


` badInput` unparseable input


` customError` author-defined error via custom message


Validate on every` input` event for real-time feedback, and expose the standard methods (` checkValidity()` ,` reportValidity()` ,` validity` ,` validationMessage` ) so callers can treat your control like any native field:


```text
#  validate  () {
const   flags   =   {};
let   message   =   ''  ;
if (  this  .  hasAttribute  (  '  required  '  )   &&   !  this  .  #value  ) {
flags  .  valueMissing   =   true  ;
message   =   '  This field is required.  '  ;
}
this  .  #internals  .  setValidity  (  flags  ,   message  ,   this  .  input  );
this  .  #renderError  (  message  );
}


checkValidity  () {   return   this  .  #internals  .  checkValidity  (); }
reportValidity  () {   return   this  .  #internals  .  reportValidity  (); }
get   validity  () {   return   this  .  #internals  .  validity  ; }
get   validationMessage  () {   return   this  .  #internals  .  validationMessage  ; }
```


` setValidity()` is only half of accessible validation: the browser will block submission and apply` :invalid` , but unless you also render a visible,` aria-describedby` -linked message, the user gets a form that silently refuses to submit. The` #renderError` step is covered in the accessibility section.


## The four form lifecycle callbacks


Form-associated custom elements get four extra reaction callbacks beyond the standard custom-element lifecycle. These are the completeness-defining piece most tutorials skip.` formAssociatedCallback(form)` is called when the associated form changes;` formResetCallback()` is called when the form is being reset and the element should clear whatever value the user set;` formDisabledCallback(isDisabled)` is called when the disabled state changes; and` formStateRestoreCallback(state, reason)` is called when the browser restores the element’s state (reason` "restore"` ) or fulfills autofill (reason` "autocomplete"` ).


Callback Fires when Argument Typical body


` formAssociatedCallback` element gains/loses a form owner the new` form` (or` null` ) wire up form-dependent state


` formDisabledCallback` host or ancestor` fieldset` disabled toggles` boolean` mirror onto internal input


` formResetCallback` form is reset none restore the default value


` formStateRestoreCallback` back/forward nav, reload, autofill restored **state** , reason rehydrate from saved state


```text
formDisabledCallback  (  disabled  ) {
this  .  input  .  disabled   =   disabled  ;
}


formResetCallback  () {
this  .  value   =   this  .  getAttribute  (  '  value  '  )   ??   ''  ;
this  .  #internals  .  setValidity  ({});
this  .  #renderError  (  ''  );
}


formStateRestoreCallback  (  state  ,   reason  ) {
if (  reason   ===   '  restore  '  )   this  .  value   =   state  ;
}
```


The` disabled` and` readonly` attributes carry spec meaning here:[the disabled attribute makes a form-associated custom element non-interactive and prevents its submission value from being submitted](https://html.spec.whatwg.org/multipage/custom-elements.html) , while the` readonly` attribute specifies that the element is barred from constraint validation.


### State restoration, done to spec


` setFormValue()` accepts an optional second argument —` setFormValue(value, state)` — where` state` is a client-only representation that doesn’t get sent to the server, useful for restoring richer internal state than the bare submission value. The spec is now explicit about what the restore callback receives: when the user agent updates a form-associated custom element’s value on behalf of a user or as part of navigation, its` formStateRestoreCallback` is called, given the new *state* and a string indicating a reason,` "autocomplete"` or` "restore"` . An earlier spec inconsistency caused real divergence —[Chromium was calling the callback with the value while WebKit called it with the state](https://github.com/whatwg/html/issues/9114) — but that was resolved in favor of state. If you support older Chromium/Edge builds, handle the argument defensively.


Only` "restore"` is dependable today.[Firefox shipped formStateRestoreCallback support, but notes that 'autocomplete' for custom elements remains unsupported](https://bugzilla.mozilla.org/show_bug.cgi?id=1556358) . Treat` "restore"` (back-navigation and reload) as the only mode you can rely on across engines.


## Accessibility done right


Setting validity is not the same as communicating it. To make errors perceivable, render a message inside the shadow root, link it to the internal input with` aria-describedby` , and mark it as a live region so assistive technology announces it:


```text
#  renderError  (  message  ) {
let   err   =   this  .  shadowRoot  .  querySelector  (  '  .error  '  );
if (  !  err  ) {
err   =   document  .  createElement  (  '  div  '  );
err  .  className   =   '  error  '  ;
err  .  id   =   '  err  '  ;
err  .  setAttribute  (  '  role  '  ,   '  alert  '  );
err  .  setAttribute  (  '  aria-live  '  ,   '  assertive  '  );
this  .  shadowRoot  .  append  (  err  );
this  .  input  .  setAttribute  (  '  aria-describedby  '  ,   '  err  '  );
}
err  .  textContent   =   message  ;
}
```


Because both the input and the error live in the same shadow root,` aria-describedby` resolves correctly within that tree. The host element’s own ARIA semantics can be set through` ElementInternals` properties — and that surface is now broadly supported, including[ElementInternals.role](https://developer.mozilla.org/en-US/docs/Web/API/ElementInternals/role) and the` aria*` reflection properties.


A common production failure mode is the half-finished control that *looks* done but fails silently — and these are exactly the bugs session replay surfaces, because they throw no console error. Replay reveals a user clicking submit repeatedly against a control that reported` valueMissing` with no visible message; a label click that lands nowhere because` delegatesFocus` was never set; or a submitted payload missing a field because the setter never called` setFormValue` . One more trap worth a tool-tip in review: slotting a native` required` input into a custom element leaves the light-DOM control active in the form alongside the host, producing double participation. Keep native controls out of slots when the host is itself form-associated.


## Using this inside a framework


Framework integrations for form-associated custom elements wrap exactly the raw` ElementInternals` API shown here.[Stencil](https://stenciljs.com/docs/form-associated) ships first-class support as of v4.5.0 via an` @AttachInternals` decorator, and its` formStateRestoreCallback` also receives a second` mode` argument, either` "restore"` or` "autocomplete"` , indicating the reason for restoration.[Lit](https://lit.dev/docs/composition/mixins/) does *not* build FACE in — you add a small` FormAssociated` mixin yourself or import a community one — but that mixin wraps the same` formAssociated` flag,` attachInternals()` ,` setFormValue()` , and lifecycle callbacks shown here. Learning the platform API directly means you can read, debug, or replace any of those wrappers.


Assemble the pieces — the value setter, internal input sync,` delegatesFocus` ,` setValidity` with a linked` role="alert"` message, and all four lifecycle callbacks — and you have a single` <text-field>` that submits, validates, resets, and restores indistinguishably from a native input. Drop it into a` <form>` , wire a` <label for>` , and the next concrete step is to verify the round trip: submit the form, read the value back from` new FormData(form)` , and confirm constraint validation blocks an empty required field.


## FAQs


What is the difference between the formdata event and ElementInternals for adding values to a form?


The formdata event injects arbitrary data into a submission: you listen for it on the form and call event.formData.append() to add fields. ElementInternals builds a reusable control that participates fully in the form, including constraint validation, labeling, reset, and state restoration through setFormValue and setValidity. Use the formdata event for quick one-off data injection; reach for ElementInternals when you need a control that behaves like a native input.


Why does attachInternals() throw a NotSupportedError?


attachInternals() throws a NotSupportedError when the element is not an autonomous custom element. Form-associated custom elements must extend HTMLElement directly, not a built-in subclass like HTMLInputElement, and the class must be registered through customElements.define before instantiation. Calling attachInternals() more than once on the same element also throws. Place the call once in the constructor on a class that extends HTMLElement to avoid the error.


Does delegatesFocus interfere with manual tabindex management on a form-associated custom element?


With delegatesFocus set to true on the shadow root, you generally do not need to manage tabindex manually. Focus delegates to the first focusable element inside the shadow root, which is the internal input already in the tab order, so the host element forwards focus automatically. A label-for click and keyboard tabbing both reach the internal input. Adding a manual tabindex on the host can create a duplicate tab stop, so omit it when delegatesFocus handles the routing.


What happens when you call setFormValue with null?


Calling internals.setFormValue(null) removes the element from form submission entirely: its name and value are excluded from the FormData object the form produces. This differs from passing an empty string, which submits the field with an empty value. Use null to deliberately exclude a control, such as a disabled or readonly field, and pass an actual value string or FormData whenever the control should contribute to submission.


DevTools for the frontend


## Gain Debugging Superpowers


Unleash the power of session replay to reproduce bugs, track slowdowns and uncover frustrations in your app. Get complete visibility into your frontend with **OpenReplay** — the most advanced open-source session replay tool for developers.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
