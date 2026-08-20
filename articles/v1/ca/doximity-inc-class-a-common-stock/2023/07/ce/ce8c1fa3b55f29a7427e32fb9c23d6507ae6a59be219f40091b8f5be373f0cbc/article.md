---
schema_version: "1.0.0"
document_id: "ce8c1fa3b55f29a7427e32fb9c23d6507ae6a59be219f40091b8f5be373f0cbc"
company_key: "doximity-inc-class-a-common-stock"
company: "Doximity Inc."
source_id: "doximity-inc-class-a-common-stock-rss-4199c1c9997e"
canonical_url: "https://technology.doximity.com/articles/building-a-note-taking-app-in-compose"
published_at: "2023-07-05T19:47:00+00:00"
first_seen_at: "2026-07-25T01:15:21.586776+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:5a7155865a0b20dd778ab867d4ab799cc6cc54cfda0b2a40572d6002263d5c2c"
---

# Building a Note-Taking App in Compose

In this case study, we will build a note-taking app that lets the user add, edit and delete notes. It uses Compose for both the view *and* presentation layers!


*Note: This is a follow up to[Part 1: Simplifying State Management with Compose](https://technology.doximity.com/articles/simplifying-state-management-with-compose) and assumes the reader is already familiar with[Jetpack Compose](https://developer.android.com/jetpack/compose) .*


## The Template


I find it useful to start with the model that represents the state of the screen we’re building. It will have a list of notes, with each note containing properties for the text and checkbox:


```text
data class    NotesUiModel  (  val    notes  :    List  <  Note  >)    :    UiModel    {
data class    Note  (  val    text  :    String  ,    val    isChecked  :    Boolean  )    :    UiModel
}


```


It’ll be the job of the presenter to produce this model. Initially, let’s implement a stub to return an empty list of notes (we’ll ignore parameters for now):


```text
class    NotesListPresenter    :    Presenter  <  NotesUiModel  ,    Unit  >    {


@Composable
override    fun    present  (  params  :    Unit  ):    NotesUiModel    {
return    NotesUiModel  (  notes    =    emptyList  ())
}
}


```


Then we can build our view to render the model that’s returned by the presenter:


```text
@Composable
fun    NotesScreen  ()    {
val    presenter  :    NotesListPresenter    =    koinInject  ()    // we use koin for DI
val    uiModel    =    presenter  .  present  (  Unit  )


Column    {
TopAppBar  (  title    =    {    Text  (  "Notes"  )    })
Notes  (  uiModel  )
}
}
}


```


## The View


The views themselves are pretty self-explanatory if you’re already familiar with building UIs in Compose. For the notes, we’ll take in a` NotesUiModel` argument and create a` LazyColumn` with the` notes` property. A FAB button is used for adding new notes, although we’ll skip the triggering of events and return to this part in a little bit:


```text
@Composable
private    fun    Notes  (  uiModel  :    NotesUiModel  )    {
Box    {
LazyColumn    {
items  (  uiModel  .  notes  )    {    note    ->
Note  (  note  )
}
}


FloatingActionButton  (  onClick    =    {    /* TODO */    })    {
Icon  (  imageVector    =    Icons  .  Rounded  .  Add  )
}
}
}


```


Then we can render each note with a checkbox, text field and delete button:


```text
@Composable
private    fun    Note  (  uiModel  :    Note  )    {
Row    {
Checkbox  (
checked    =    uiModel  .  isChecked  ,
onCheckedChange    =    {    /* TODO */    }
)


TextField  (
value    =    uiModel  .  text  ,
textStyle    =    TextStyle  (
textDecoration    =    if    (  uiModel  .  isChecked  )    {
TextDecoration  .  LineThrough
}    else    {
TextDecoration  .  None
}
),
placeholder    =    {    Text  (  "Add note"  )    },
onValueChange    =    {    /* TODO */    }
)


IconButton  (  onClick    =    {    /* TODO */    })    {
Icon  (  imageVector    =    Icons  .  Default  .  Delete  )
}
}
}


```


The state of the` Note` is used to determine whether the` Checkbox` is checked or not and also for the current value of the` TextField` . Again, we’ll implement the events later on.


## The Presenter


Returning to the presenter, we’ll need to obtain the data for the notes. Using[Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) , the presenter will interact with the data layer via use cases. We can inject an` ObserveNotesUseCase` into` NotesListPresenter` and then invoke it with the aid of` produceState` :


```text
class    NotesListPresenter  (
val    observeNotesUseCase  :    ObserveNotesUseCase
)    :    Presenter  <  NotesUiModel  ,    Unit  >    {


@Composable
override    fun    present  (  params  :    Unit  ):    NotesUiModel    {
val    notes  :    List  <  NoteDataModel  >    by    produceState  (  initialValue    =    emptyList  ())    {
observeNotesUseCase  ().  collect    {
value    =    it
}
}


// ...
}
}


```


` ObserveNotesUseCase` returns a` Flow<List<NoteDataModel>>` . In order to collect the flow, we need a` CoroutineScope` , which` produceState` provides us. In addition, the code block passed to` produceState` will only execute once. This prevents us from recollecting the flow every time` present()` recomposes.


To help cut down the noise of the` produceState` call, we can encapsulate it with an extension function:


```text
@Composable
fun    <  T  >    FlowUseCase  <  T  >.  launchUseCase  (  initial  :    T  ):    State  <  T  >    {
return    produceState  (  initialValue    =    initial  )    {
invoke  ().  collect    {
value    =    it
}
}
}


```


Which let’s us write the much briefer:


```text
val    notes    by    observeNotesUseCase  .  launchUseCase  (  initial    =    emptyList  ())


```


Now that we have the data, we can use it to derive the model:


```text
return    NotesUiModel  (
notes    =    notes  .  map    {    note    ->
var    text    by    remember  (  note  .  id  )    {    mutableStateOf  (  note  .  text  )    }
var    isChecked    by    remember  (  note  .  id  )    {    mutableStateOf  (  note  .  isFinished  )    }


Note  (  text  ,    isChecked  )
}
)


```


We` map` over each note in the list, transforming the` NoteDataModel` into a` NotesUiModel.Note` . We create two` MutableState` variables for` text` and` isChecked` , each initialized to their respective values from the data.


*Note: If we omitted` note.id` as a key to` remember` , Compose would associate each state with its position in the list, rather than a specific` NoteDataModel` . Deleting notes in the list, which we’ll implement shortly, would cause the states to be out of sync with the list.*


## The Events


We still need to add events, that way the view can communicate to the presenter. We want to support the following operations:


- Adding a new note
- Updating the checkbox of a note
- Updating the text of a note
- Deleting a note


The first operation, adding a new note, will be fired off as an event by the FAB button. We can simply add an` EventHandler` to` NotesUiModel` to support this:


```text
data class    NotesUiModel  (
val    notes  :    List  <  Note  >,
val    events  :    EventHandler  <  Event  >
)    :    UiModel    {
sealed    interface    Event    :    UiEvent    {
object    OnAdd    :    Event
}


// ...
}


```


In the view, the FAB button can now invoke the` EventHandler` of the model and pass it` Event.OnAdd` :


```text
FloatingActionButton  (  onClick    =    {    uiModel  .  events  .  handle  (  Event  .  OnAdd  )    })    {
Icon  (  imageVector    =    Icons  .  Rounded  .  Add  )
}


```


We need to handle this event on the presenter side, which we’ll accomplish with the aid of` CreateNoteUseCase` :


```text
class    NotesListPresenter  (
// ...
val    createNoteUseCase  :    CreateNoteUseCase
)    :    Presenter  <  NotesUiModel  ,    Params  >    {


@Composable
override    fun    present  (  params  :    Unit  ):    NotesUiModel    {
// ...
}
}


```


This particular use case runs as a` suspend` function and must be invoked from a` CoroutineScope` , which we can access inside of a Composable function with` rememberCoroutineScope()` :


```text
@Composable
override    fun    present  (  params  :    Unit  ):    NotesUiModel    {
val    scope    =    rememberCorotuineScope  ()


// ...
}


```


Then we can handle the event in the model that gets returned:


```text
return    NotesUiModel  (
notes    =    notes  .  map    {    note    ->
// ...
},
events    =    EventHandler    {    event    ->
when    (  event  )    {
is    Event  .  OnAdd    ->    scope  .  launch    {
createNoteUseCase  ()
}
}
}
)


```


When` CreateNoteUseCase` is invoked, it updates a data cache that will trigger a new emission on the flow returned by` ObserveNotesUseCase` . This emission will cause the` present()` function to recompose and return an updated model that contains the newly created note.


The other three operations are tied to a specific note in the list, therefore we want to add a separate` EventHandler` to the` Note` model:


```text
data class    Note  (
val    text  :    String  ,
val    isChecked  :    Boolean  ,
val    events  :    EventHandler  <  Event  >
)    :    UiModel    {
sealed    interface    Event    :    UiEvent    {
object    OnCheck    :    Event
data class    OnUpdateText  (  val    text  :    String  )    :    Event
object    OnDelete    :    Event
}
}


```


As before, we’ll inject the necessary use cases:


```text
class    NotesListPresenter  (
// ...
val    updateNoteUseCase  :    UpdateNoteUseCase  ,
val    deleteNoteUseCase  :    DeleteNoteUseCase
)    :    Presenter  <  NotesUiModel  ,    Unit  >    {


@Composable
override    fun    present  (  params  :    Unit  ):    NotesUiModel    {
// ...
}
}


```


Then handle the events when creating each` Note` :


```text
return    NotesUiModel  (
notes    =    notes  .  map    {    note    ->


Note  (
text    =    text  ,
isChecked    =    isChecked  ,
events    =    EventHandler    {    event    ->
when    (  event  )    {
Note  .  Event  .  OnCheck    ->    {
isChecked    =    isChecked  .  not  ()
scope  .  launch    {
updateNoteUseCase  (  NoteDataModel  (  note  .  id  ,    text  ,    isChecked  ))
}
}
is    Note  .  Event  .  OnUpdateText    ->    {
text    =    event  .  text
scope  .  launch    {
}
}
Note  .  Event  .  OnDelete    ->    scope  .  launch    {
deleteNoteUseCase  (  note  .  id  )
}
}
}
)
},
// ...
)


```


In the case of` Note.Event.OnCheck` and` Note.Event.OnUpdateText` , we are updating the local state before invoking` UpdateNoteUseCase` . This ensures that the UI stays responsive when the user rapidly enters text or clicks on a checkbox. If` UpdateNoteUseCase` instead triggered a new emission via` ObserveNotesUseCase` , the user might see buggy behavior since the flow may not keep up with the speed of the user’s inputs. On the other hand, deleting a note is a one time operation and we can simply wait for` DeleteNoteUseCase` to send a new emission.


Finally, let’s fire off each event from the view:


```text
@Composable
private    fun    Note  (  uiModel  :    Note  )    {
Row    {
Checkbox  (
checked    =    uiModel  .  isChecked  ,
onCheckedChange    =    {    uiModel  .  events  (  Note  .  Event  .  OnCheck  )    }
)


TextField  (
value    =    uiModel  .  text  ,
textStyle    =    TextStyle  (
textDecoration    =    if    (  uiModel  .  isChecked  )    {
TextDecoration  .  LineThrough
}    else    {
TextDecoration  .  None
}
),
placeholder    =    {    Text  (  "Add note"  )    },
onValueChange    =    {    uiModel  .  events  (  Note  .  Event  .  OnUpdateText  (  it  ))    }
)


IconButton  (  onClick    =    {    uiModel  .  events  (  Note  .  Event  .  OnDelete  )    })    {
Icon  (  imageVector    =    Icons  .  Default  .  Delete  )
}
}
}


```


## The Child


One of the nice things about Composable functions is that they can easily be broken down into smaller ones. This concept is no different with presenters: we can take a larger presenter and break it down into one or more smaller presenters. This is especially useful when writing code that needs to be reused across screens.


In the case of` NotesListPresenter` , we can seamlessly extract the management of each note into its own presenter. This aligns nicely with the model too, since` Note` is already nested inside of` NotesUiModel` :


```text
data class    NotesUiModel  (
val    notes  :    List  <  Note  >,
val    events  :    EventHandler  <  Event  >
)    :    NotesUiModel    {


data class    Note  (
val    text  :    String  ,
val    isChecked  :    Boolean  ,
val    events  :    EventHandler  <  Event  >
)    :    UiModel    {
// ...
}


// ...
}


```


With this in mind, we’ll create a` NoteItemPresenter` that returns a` NotesUiModel.Note` and accepts a` NoteDataModel` as a parameter:


```text
class    NoteItemPresenter  (
val    updateNoteUseCase  :    UpdateNoteUseCase  ,
val    deleteNoteUseCase  :    DeleteNoteUseCase
)    :    Presenter  <  Note  ,    NoteItemPresenter  .  Params  >    {


data class    Params  (  val    note  :    NoteDataModel  )


@Composable
override    fun    present  (  params  :    Params  ):    Note    {
val    scope    =    rememberCoroutineScope  ()


val    note    =    params  .  note


return    Note  (
text    =    text  ,
isChecked    =    isChecked  ,
events    =    EventHandler    {    event    ->
when    (  event  )    {
Note  .  Event  .  OnCheck    ->    {
isChecked    =    isChecked  .  not  ()
scope  .  launch    {
}
}
is    Note  .  Event  .  OnUpdateText    ->    {
text    =    event  .  text
scope  .  launch    {
}
}
Note  .  Event  .  OnDelete    ->    scope  .  launch    {
deleteNoteUseCase  (  note  .  id  )
}
}
}
)
}
}


```


The presentation logic is identical to what we had before inside the` map` of` NotesListPresenter` , but now we can just call the` present()` function of` NoteItemPresenter` for each note in the list:


```text
class    NotesListPresenter  (
val    noteItemPresenter  :    NoteItemPresenter  ,
// ...
)    :    Presenter  <  NotesUiModel  ,    Unit  >    {


@Composable
override    fun    present  (  params  :    Unit  ):    NotesUiModel    {
// ...


return    NotesUiModel  .  Data  (
notes    =    notes  .  map    {    note    ->
noteItemPresenter  .  present  (  NoteItemPresenter  .  Params  (  note  ))
},
events    =    EventHandler    {    event    ->
// ...
}
)
}
}


```


Since each` Note` is already broken out into its own Composable function in the view, we can easily reuse this view and its presenter on another screen.


## The Test


Now that we have a fully working implementation, let’s write some tests for` NotesListPresenter` . We’ll add the following plugins to help us with testing:


```text
buildscript    {
repositories    {
mavenCentral  ()
}
dependencies    {
classpath    'app.cash.molecule:molecule-gradle-plugin:0.9.0'
classpath    'com.jeppeman.mockposable:mockposable-gradle:0.4'
}
}


apply    plugin:    'app.cash.molecule'
apply    plugin:    'com.jeppeman.mockposable'


```


Since we’re testing Composable functions, we need a way to call them from the regular, non-Composable functions of our tests.[Molecule](https://github.com/cashapp/molecule) bridges this gap by creating a flow that will emit the value returned by a Composable function each time it recomposes. We can then collect and assert on these values with the aid of[Turbine](https://github.com/cashapp/turbine/) , which comes packaged with Molecule.


[Mockposable](https://github.com/jeppeman/mockposable) does what the name suggests: it allows us to mock the behavior of Composable functions. This is useful when we’re testing a presenter that calls another presenter, which is the case with` NotesListPresenter` calling` NoteItemPresenter` .


*Note: These tests are also using[mockk](https://mockk.io/) and[kluent](https://github.com/MarkusAmshove/Kluent) .*


First, let’s write a test that verifies the notes are correctly loaded and we get back the model we’re expecting:


```text
class    NotesListPresenterTest    {


@Test
fun    `  load    and    return    notes  `  ()    {
TODO  ()
}
}


```


We’ll want to mock the behavior of` ObserveNotesUseCase` to return a flow that emits a list of notes:


```text
@Test
fun    `  load    and    return    notes  `  ()    {
val    observeNotesUseCase  :    ObserveNotesUseCase    =    mockk    {
every    {    invoke  ()    }    returns    flowOf  (
listOf  (
NoteDataModel  (  id    =    1  ,    text    =    "Note 1"  ,    isFinished    =    false  ),
NoteDataModel  (  id    =    2  ,    text    =    "Note 2"  ,    isFinished    =    true  ),
NoteDataModel  (  id    =    3  ,    text    =    "Note 3"  ,    isFinished    =    false  )
)
)
}
}


```


Then we’ll want to mock the behavior of` NoteItemPresenter` , which is responsible for transforming each` NoteDataModel` into a` NotesUiModel.Note` . As mentioned previously, we can use Mockposable to assist us with this:


```text
@Test
fun    `  load    and    return    notes  `  ()    {
// ...


val    noteItemPresenter  :    NoteItemPresenter    =    mockk    {
everyComposable    {    present  (  any  ())    }    answersComposable    {
val    noteArg    =    (  firstArg  ()    as    NoteItemPresenter  .  Params  ).  note
Note  (  noteArg  .  text  ,    noteArg  .  isFinished  ,    EventHandler    {})
}
}
}


```


Here we’re taking the argument passed in, which is a` NoteItemPresenter.Params` , and forwarding the data of the` note` property into a` NotesUiModel.Note` .


*Note: Since` EventHandler` is stateless and doesn’t effect equality, we can simply use an empty one as a placeholder in the test.*


Now that we have the dependencies mocked, we can instantiate` NotesListPresenter` and invoke its` present()` function with the aid of molecule:


```text
@Test
fun    `  load    and    return    notes  `  ()    {
// ...


val    notesListPresenter    =    NotesListPresenter  (
notePresesnter  ,
observeNotesUseCase  ,
createNoteUseCase    =    mockk  ()    // not used by this test
)


moleculeFlow  (  RecompositionClock  .  Immediate  )    {
notesListPresenter  .  present  (  Unit  )
}
}


```


` moleculeFlow()` takes in a lambda that gives us a context for invoking the Composable` present()` function and returns us a flow that will emit a new` NotesUiModel` each time` present()` recomposes.` RecompositionClock.Immediate` allows us to control the execution of the Composable in a testable manner. To collect emissions, we call` test()` on the flow that is returned by molecule:


```text
@Test
fun    `  load    and    return    notes  `  ()    =    runTest    {
// ...


moleculeFlow  (  RecompositionClock  .  Immediate  )    {
notesListPresenter  .  present  (  Unit  )
}.  test    {
TODO  ()
}
}


```


The lambda passed to` test()` gives us access to a Turbine for the flow, which we’ll use to consume emissions and assert the behavior of the` present()` function.


*Note: We surround the test with` runTest()` since` test()` is a` suspend` function.*


Recall that when invoking` ObserveNotesUseCase` with` launchUseCase()` , we provide it an initial value of` emptyList()` :


```text


```


Therefore, we can expect the first model emitted to contain no notes:


```text
moleculeFlow  (  RecompositionClock  .  Immediate  )    {
notesListPresenter  .  present  (  Unit  )
}.  test    {
awaitItem  ()    shouldBeEqualTo    NotesUiModel  (  emptyList  (),    EventHandler    {})
}


```


After this first emission, we can then expect the list of notes we used when stubbing:


```text
moleculeFlow  (  RecompositionClock  .  Immediate  )    {
notesListPresenter  .  present  (  Unit  )
}.  test    {
// ...


awaitItem  ()    shouldBeEqualTo    NotesUiModel  (
notes    =    listOf  (
Note  (  text    =    "Note 1"  ,    isChecked    =    false  ,    EventHandler    {}),
Note  (  text    =    "Note 2"  ,    isChecked    =    true  ,    EventHandler    {}),
Note  (  text    =    "Note 3"  ,    isChecked    =    false  ,    EventHandler    {})
)
events    =    EventHandler    {}
)
}


```


For a second test, let’s verify the behavior for adding a new note:


```text
@Test
fun    `  add    a    new    note  `  ()    =    runTest    {
val    observeNotesUseCase  :    ObserveNotesUseCase    =    mockk    {
every    {    invoke  ()    }    returns    emptyFlow  ()
}


val    createNoteUseCase  :    CreateNotUseCase    =    mockk  (  relaxedUnitFun    =    true  )


val    notesListPresenter    =    NotesListPresenter  (
noteItemPresenter    =    mockk  (),    // not used by this test
observeNotesUseCase  ,
createNoteUseCase
)


moleculeFlow  (  RecompositionClock  .  Immediate  )    {
notesListPresenter  .  present  (  Unit  )
}.  test    {
awaitItem  ().  events  .  handle  (  Event  .  OnAdd  )
coVerify    {    createNoteUseCase  ()    }
}
}


```


Since we only need to verify that` CreateNoteUseCase` is invoked, we can simply consume the first emission and then send` Event.OnAdd` to its` EventHandler` .


A similar set of tests can be written for` NoteItemPresenter` and is left as an activity for the reader.


## The Extras


Let’s assume that the loading of notes may take an indeterminate amount of time to load and in the meantime we want to show a loading indicator to the user. We can change` NotesUiModel` to a` sealed interface` and have it contain a` Loading` and` Data` type:


```text
sealed    interface    NotesUiModel    :    UiModel    {
object    Loading    :    NotesUiModel


data class    Data  (
val    notes  :    List  <  Note  >,
val    events  :    EventHandler  <  Event  >
)    :    NotesUiModel    {
// ...
}
}


```


If we update the` launchUseCase()` extension function to support nullable types and use` null` as the initial state, we can check for this to determine if the data has loaded or not:


```text
@Composable
fun    <  T  >    FlowUseCase  <  T  >.  launchUseCase  (  initial  :    T  ?    =    null  ):    State  <  T  ?  >    {
return    produceState  <  T  ?>(  initialValue    =    initial  ,    producer    =    {
invoke  ().  collect    {
value    =    it
}
})
}


@Composable
override    fun    present  (  params  :    Unit  ):    NotesUiModel    {
// ...


val    notesResult    by    observeNotesUseCase  .  launchUseCase  ()
val    notes    =    notesResult    ?:    return    NotesUiModel  .  Loading


// ...
}


```


You’ll notice that we have two variables now,` notesResult` and` notes` . The latter unwraps the delegate value and returns early if` null` . This allows us to reference notes without treating it as a nullable value, which we can’t do when referencing the delegate value directly.


*Note: By convention, we add` “Result”` to the delegate variable name.*


In the view, we can use a` when` expression to unwrap the model returned by the presenter and decide if we want to show the notes or a loading indicator:


```text
@Composable
fun    NotesScreen  ()    {
val    presenter  :    NotesListPresenter    =    koinInject  ()
val    uiModel    =    presenter  .  present  (  Unit  )


Column    {
TopAppBar  (  title    =    {    Text  (  "Notes"  )    })


when    (  uiModel  )    {
is    TodoUiModel  .  Data    ->    Notes  (  uiModel  )
TodoUiModel  .  Loading    ->    LoadingProgressIndicator  ()
}
}
}


```


To further enrich the user experience, let’s add an empty note when there are no existing notes to show:


```text
@Composable
override    fun    present  (  params  :    Unit  ):    NotesUiModel    {
// ...


// create an initial note if there are none
LaunchedEffect  (  Unit  )    {
if    (  notes  .  isEmpty  ())    {
createNoteUseCase  ()
}
}


// ...
}


```


The` LaunchedEffect` is key’d with` Unit` to ensure that the check and the subsequent call to` createNoteUseCase()` only executes once inside of` present()` . This prevents us from needlessly adding a note after the user has deleted the last remaining one in the list.


Lastly, you’ll recall that we don’t emit changes to the flow of notes with` UpdateNoteUseCase` , as this causes bugs when the user rapidly enters input. We’ll want to ensure these emissions happen when the user leaves the screen though, otherwise the changes would be lost when returning to it from another screen. To do this, we’ll need to listen for Lifecycle events, which we can do with the following Composable function:


```text
@Composable
fun    LifecycleEffect  (  onEvent  :    (  Lifecycle  .  Event  )    ->    Unit  )    {
val    lifecycleOwner    =    rememberUpdatedState  (  LocalLifecycleOwner  .  current  )


DisposableEffect  (  lifecycleOwner  .  value  )    {
val    lifecycle    =    lifecycleOwner  .  value  .  lifecycle
val    observer    =    LifecycleEventObserver    {    _  ,    event    ->
onEvent  (  event  )
}


lifecycle  .  addObserver  (  observer  )
onDispose    {
lifecycle  .  removeObserver  (  observer  )
}
}
}


```


Then we can call` LifecycleEffect()` from the view and listen for` Lifecycle.Event.ON_PAUSE` :


```text
@Composable
private    fun    Notes  (  uiModel  :    NotesUiModel  .  Data  )    {
LifecycleEffect    {
if    (  it    ==    Lifecycle  .  Event  .  ON_PAUSE  )    {
uiModel  .  events  .  handle  (  Event  .  OnSave  )
}
}


// ...
}


```


*Note: We choose to do this in the view and then communicate it to the presenter via an event, as it makes testing the presenter easier.*


To handle the event in the presenter, we can’t rely on` rememberCoroutineScope()` like we’ve done with the other events. Since we’re performing the save operation as the view is being paused (and possibly destroyed), we need to ensure the operation has time to complete. For this type of scenario we’ll introduce an` AppScope` class that will exist for the lifetime of the app:


```text
class    AppScope  (
dispatcher  :    CoroutineDispatcher    =    Dispatchers  .  Main
)    :    CoroutineScope  ,    DefaultLifecycleObserver    {


override    val    coroutineContext    =    SupervisorJob  ()    +    dispatcher


init    {
this  .  launch  (  dispatcher  )    {
ProcessLifecycleOwner  .  get  ().  lifecycle  .  addObserver  (  this  @AppScope  )
}
}


override    fun    onDestroy  (  owner  :    LifecycleOwner  )    {
coroutineContext  .  cancelChildren  ()
}
}


```


*Note: This scope is preferable to using` GlobalScope` . For more details on why,[refer to this article](https://elizarov.medium.com/the-reason-to-avoid-globalscope-835337445abc) .*


We can then inject` AppScope` into` NotesListPresenter` , along with` SaveNotesUseCase` , and properly handle` Event.OnSave` :


```text
class    NotesListPresenter  (
// ...
val    appScope  :    AppScope  ,
val    saveNotesUseCase  :    SaveNotesUseCase
)    :    Presenter  <  NotesUiModel  ,    Unit  >    {


@Composable
override    fun    present  (  params  :    Params  ):    NotesUiModel    {
// ...


return    NotesUiModel  .  Data  (
notes    =    notes  .  map    {    note    ->
// ...
},
events    =    EventHandler    {    event    ->
when    (  event  )    {
Event  .  OnAdd    ->    // ...
Event  .  OnSave    ->    appScope  .  launch    {
saveNotesUseCase  ()
}
}
}
)
}
}


```


For testing, we can inject` Dispatchers.Unconfined` into` AppScope` , which ensures the code executes in a blocking manner.


## The End


This completes the case study of the Notes app. This should give you a realistic and thorough example of using Compose to manage the presentation logic of an app. A[full code sample](https://gist.github.com/jpetitto/0e1ed12ac89fbda012563709d4f4176d) is provided as a Gist.


---


Be sure to follow[@doximity_tech](https://twitter.com/doximity_tech) if you'd like to be notified about new blog posts.
