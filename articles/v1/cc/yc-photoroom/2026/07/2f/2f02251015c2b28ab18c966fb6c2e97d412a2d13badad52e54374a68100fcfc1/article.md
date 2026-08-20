---
schema_version: "1.0.0"
document_id: "2f02251015c2b28ab18c966fb6c2e97d412a2d13badad52e54374a68100fcfc1"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/building-live-collaboration-in-rust-for-millions-of-users-part-3"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:299c5d1741b8d8f4237d654e213d6a89f9b15f3a63e418c56a2ccbabf367a4ac"
---

# Building live collaboration in Rust for millions of users, part 3

## Fine-grained reactivity across the FFI boundary


In[the previous part of this series,](https://www.photoroom.com/inside-photoroom/building-live-collaboration-in-rust-for-millions-of-users-part-2) we looked at the foundations of integrating networking into the Photoroom engine. We introduced Crux and covered the steps we took to build the comments feature inside the engine. The experience gave us the confidence to start working on bigger things.


You may remember[from the first part of the series](https://www.photoroom.com/inside-photoroom/building-live-collaboration-in-rust-for-millions-of-users-part-1) that our ultimate goal was to bring real-time collaborative editing to Photoroom. The first step was moving the actual project manipulation into the engine, so it can be consistent across the apps, and any necessary modifications could be implemented once. Very quickly however, we realized a problem.


The project manipulation is essentially a pure function:


```text
fn edit_project(project: Project, operation: Operation) -> Project {
...
}
```


The normal approach to updating the UI in Crux is that the shell requests a fresh view model when it needs it by calling a function called` view` on the core, which returns a complete, up to date view model. This would not work for projects.


The` Project` type can hold relatively complex data - a project with several layers with effects applied, etc., but more importantly, on the shell side, these components of a project have assets associated with them - images, fonts, etc. If we replaced the entire project every time we edited it, it would be very hard for the UI to understand what exactly changed and how to minimally update the UI in a performant way. This is why the web app used MobX in the first place – it enabled fine-grained reactivity.


And so we needed to come up with a more surgical approach to view model updates. One where on the shell side, we could keep an instance of the view model around, and the core could send out a series of targeted changes - “patches” - that should be applied to that instance.


### Stage one: explicit change types


We started this journey early, while still working on comments, and our initial approach was simple:


1.


Replace the crux built in render capability, which only serves as whole-state updates, with a custom capability, where the signal only carries the view model changes


2.


Introduce a` Change` enum type representing the changes


The capability was a simple job to add. The change type started simple as well. We had two types of changes:


-


An update - replacing a single comment with an updated one


-


A[splice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice) - changing a thread of comments by removing zero or more comments starting at a particular index and inserting zero or more comments in their place


The type looked something like this:


```text
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq)]
#[serde(rename_all = "camelCase", tag = "type")]
pub enum Change {
#[serde(rename_all = "camelCase")]
CommentsSplice {
/// position to insert comments
start: usize,
/// number of comments to replace
replace: usize,
/// the new comments
with: Vec<CommentViewModel>,
},
#[serde(rename_all = "camelCase")]
CommentUpdate {
/// the position to update
index: usize,
/// the updated comment
with: CommentViewModel,
},
}
```


On the shell side, this could be used as an instruction to update the view model accordingly.


The CommentViewModel is a wrapper around the comment data, dealing with the state of the comment relative to the remote API resource and the network request. It can be pending, complete or failed, and they carry the actual comment data or the inputs that started them as ‘placeholders’. This lets the UI optimistically present a good representation of, say, a comment which has been edited locally, but the edit has not yet committed remotely, or a comment which has failed to post, for which the UI can show a retry and cancel button. This type of lifecycle representation in the view represented the possible states well, although it was somewhat difficult to work with because of the nested enums involved, especially in TypeScript, where enums are represented as unions and the code for working with them easily does not read as well as in Swift, for example.


The actual representation of the view model aside, it was quite clear that this approach with specialized change types would not scale to the complexity of the` Project` type – we would just end up reflecting the input operations back to the shell, and not solving the problem at all.


We needed a different idea.


### Stage two: key paths and changes as data


What we needed was a way to generically target a value deep in the view model and change it, preferably without losing type safety in the process. We also needed this to be described as serializable data which we can send from the core to the shell. To achieve that, we borrowed the concept of[key paths](https://developer.apple.com/documentation/swift/keypath) from Swift.


We could keep the idea of updates and splices, but instead of having a special update and a special splice for each field or collection in the entire view model, we’d have a universal update and splice, carrying a key path to the location in the view model which has changed. A key path is a list of successive sub-selections to make in order to get to the target value. Each element can be a field of a struct, an index in a vector, etc.


In order to keep the type safety, we based constructing the key paths on a trait, which we called` Navigable` which returns a set of valid key paths starting in the original type. An implementation on a struct type` X` with fields` a` ,` b` and` c` would return an equivalent struct where the fields are key paths from` X` to` a` ,` b` and` c` respectively. This trait can also be derived with a proc macro, and the key path construction can be made ergonomic with a bit more macro magic.


Together, this gives us code like this:


```text
#[derive(Navigable, Serialize, Deserialize)]
struct Test {
my_scalar: usize,
my_vector: Vec<usize>,
my_nested: Nested,
my_vector_of_nested: Vec<Nested>,
}


#[derive(Navigable, Serialize, Deserialize)]
struct Nested {
my_string: String,
my_vector: Vec<f64>,
}


let mut test = Test {
my_scalar: 1,
my_vector: vec![2, 3, 4],
my_nested: Nested {
my_string: "Hello".to_string(),
my_vector: vec![1, 2, 3, 4, 5],
},
my_vector_of_nested: vec![],
};


// type of third_numbers is KeyPath<Test, usize>
let third_number = keypath![Test: my_vector.my_nested.my_vector[2]];


// record an update to the third number in my_vector
let change_to_5 = Change::update(third_number, 5);
```


This way we could make sure we never construct an invalid path, and the macros even meant IDE tools like “jump to definition” would take us to the correct field on the right type, when used on a segment of the path (for example, clicking on` my_vector` in the key path above would take you to the` Nested` type). The` Change` type could also statically check that the provided new value matches the type of the value pointed at by the path.


This allowed us to express the changes generically, and the` Change` type could be serialized, so on the TypeScript end, we could simply walk the key path and make the change.


For Swift and Kotlin, we needed extra support in order to mutate the view model using keypaths. Thankfully, we only needed to do this on types which were generated by our type generation library (which we looked at[in the last part](https://www.photoroom.com/inside-photoroom/building-live-collaboration-in-rust-for-millions-of-users-part-2) of this series), and so we could extend that generated code to include support for key paths based modifications.


This new approach worked quite well for a while, but had two major downsides - one, we needed to hand-write the view model changes like in the example above, and two, the only way to test that these changes were correct was to assert on the changes themselves. We could not check the outcome of what happens when they are applied to the view model, because we lacked support for doing so in Rust. We decided the latter would be easier to tackle first.


### Stage three: key path mutability in Rust, and Pathogen


We already had an example of implementing the key path based mutability in Swift and Kotlin, and adding it in Rust wasn’t that different. Unsurprisingly, it involved another trait -` KeyPathMutable` , which could also be derived with a proc macro.


So far, we had fully typed changes –` Change<Root, Value>` – and a fully type erased equivalent (called` Patch` ), which held everything serialized as JSON. For mutability, we need to be able to represent a collection of changes to the same` Root` , but different parts of the tree, which can be of a different` Value` type. To represent these, we needed an intermediate level, which we inventively called` ChangeOf<Root>` . The value type has been erased, but the root type remains, and so it’s impossible to accidentally apply a change of` User` to a` Project` .


The` KeyPathMutable` trait has a single method:


```text
fn apply_change(&mut self, change: &ChangeOf<Self>)
```


With it, we can now apply the changes in Rust, and especially in tests.


As a simple example:


```text
let mut data = SimpleStruct {
first_field: 1,
second_field: "hello".to_string(),
third_field: vec!["one".to_string(), "two".to_string()],
};


let change = Change::update(keypath![SimpleStruct: third_field[1]], "three".to_string());
data.apply_change(&change);


assert_eq!(
data,
SimpleStruct {
first_field: 1,
second_field: "hello".to_string(),
third_field: vec!["one".to_string(), "three".to_string()],
}
);


```


This set of tools proved quite useful to us, and so we decided to open source it as a[crate, called Pathogen](https://crates.io/crates/pathogen) . You can check it out for yourself!


Pathogen gives you the trait definitions for` Navigable` and` KeyPathMutable` , the derive macros for your custom types and the supporting types for key paths and changes. It might be useful anywhere where you need to synchronise long-lived instances of a type, express the changes happening to them as data, and send them over a network or another communication channel.


With Pathogen, we could stop worrying about the size and complexity of the view model prohibiting us from implementing features like moving concepts around with drag and drop gestures, and we could finish moving project editing into the engine.


Pretty soon, a pattern had emerged, where most of our state updates were hidden behind an API which looked like this:


```text
fn do_something_to_state(&mut self, input: Type) -> (Output, Vec<ChangeOf<ViewModel>>)
```


In other words, update the state, tell me what the result is, *and how the view model has changed as a result.*


This worked okay for quite a long time, but manually creating the changes was tedious. The original Crux approach of treating the view model as a straight projection of the model is definitely less labour intensive and error prone, even with the type checking of changes and keypaths.


To make this less error prone we added application of changes into our tests and then asserted on the results, but ultimately there was a limit to the developer experience we could support with this approach. And so eventually, we set out to make one more step and come full circle.


### Stage four: View model diffing with Difficient


What if instead of writing the changes to apply to the view model by hand, and then verifying them in tests, we could produce the changes automatically, by comparing two instances of a view model? If it could work for virtual DOM, why not view models?


If we did that, we could switch back to simply converting a model to a view model, and in every call to the` update` function, we could take a snapshot of the view model at the top, another snapshot at the bottom, compare the two, and generate the necessary changes. We decided to give that a try.


This diffing algorithm needed to do a few things:


-


Compare two values of the same type (A and B) and return a “diff”. A diff is the description of what changed from A to B


-


Apply a diff to A in order to get B


-


Work on primitive types, but also arbitrary` enum` s and` struct` s, even deeply nested


-


Work on key-value collections (at least HashMap and BTreeMap) and` Vec<T>`


-


Support converting the diff to a series of` ChangeOf` patches described above, so that we can serialize it and send across the FFI boundary


At the heart of this new library, which became known as[Difficient](https://crates.io/crates/difficient) , is a trait which looks like this:


```text
pub trait Diffable<'a>: Sized {
type Diff: Replace + Apply<Parent = Self>;


fn diff(&self, other: &'a Self) -> Self::Diff;


fn apply(&mut self, diff: &Self::Diff) -> Result<(), Vec<ApplyError>> {
let mut errs = Vec::new();
diff.apply_to_base(self, &mut errs);
if errs.is_empty() {
Ok(())
} else {
Err(errs)
}
}
}


```


Types which implement` Diffable` can` diff` another value against themselves, returning a` Diff` and apply the diff to themselves to become equal to the other value. The` Diff` type changes depending on what kind of type we operate on.


For primitive types (numbers, bools, strings), there are only two outcomes of a diff:


```text
pub enum AtomicDiff<'a, T> {
/// The diffed value is unchanged
Unchanged,
/// The diffed value is replaced
Replaced(&'a T),
}


```


and can be implemented in terms of` PartialEq` . Applying this diff is a simple replacement. In the Photoroom engine, this strategy is used even for strings, because we never operate on significantly long ones, but if needed, we could give them a special treatment.


The more interesting kind of diff is the deep diff:


```text
pub enum DeepDiff<'a, Full, Patch> {
/// The diffed value is unchanged
Unchanged,
/// The diffed value is partially changed - we will need to
/// descend into the `Patched` value to find out exactly how
Patched(Patch),
/// The diffed value is replaced in full
Replaced(&'a Full),
}
```


This is used for structs and enums.


For a struct like this:


```text
pub struct Parent {
pub c1: Child1,
pub c2: Vec<Child1>,
pub c3: HashMap<i32, Child2>,
pub val: String,
}
```


the diff might look like this:


```text
pub struct ParentDiff<'a> {
pub c1: <Child1 as Diffable<'a>>::Diff,
pub c2: <Vec<Child1> as Diffable<'a>>::Diff,
pub c3: <HashMap<i32, Child2> as Diffable<'a>>::Diff,
pub val: <String as Diffable<'a>>::Diff,
}
```


You can see how the value is essentially recursive, until you it gets down to the primitives. These Diff types are not hand-written, they are built with a derive macro.


The one tricky scenario is a diff of a vector, where the difference can be a set of changes. Its diff type is as follows:


```text
pub enum VecDiff<'a, T, U> {
Unchanged,
Replaced(&'a [T]),
Changed { changes: Vec<VecChange<'a, T, U>> },
}


pub enum VecChange<'a, T, U> {
Remove {
at_index: usize,
count: usize,
},
Insert {
at_index: usize,
values: &'a [T],
},
Splice {
at_index: usize,
count: usize,
values: &'a [T],
},
Patch {
at_index: usize,
patch: U,
},
}


```


There’s a number of different list diffing algorithm, and there was no value in us reimplementing one, so we rely on[https://github.com/mitsuhiko/similar](https://github.com/mitsuhiko/similar) , from the insta snapshot testing toolchain, for the actual implementation of vector diffs, and convert its representation into Difficient compatible changes.


#### Gradual migration is the name of the game


The remaining job was introducing this system into our existing Pathogen based patching approach, which represents changes as individual deep patches, rather than a single recursive diff “tree”. To support this, Difficient allows walking the diffs with a specified “visitor” which gets called with a location and new value of every change, and is therefore able to emit` ChangeOf` s.


This meant we could introduce Difficient bottom-up, starting with the very leaves of the view model tree, in small stages, and gain confidence in its correctness and performance. First, we replaced small code regions where constructing changes directly was very fiddly and error prone, then we started moving larger chunks, sometimes entire modules of the engine at a time. Alongside this migration we also implemented all the necessary projections from models to view models. At this point it came in handy that we already wrote most tests in terms of the resulting state of the view model (using Rust version of` KeyPathMutable` ), rather than asserting on the changes themselves.


After a few weeks, the migration converted all the change construction, and we ended up with a single place where the code hints at doing something special about the view model – at the very root Crux app in the engine, which looks like this:


```text
fn update(
&self,
event: Self::Event,
model: &mut Self::Model,
) -> Command<Effect, Event> {
let pre_update_view = self.view(model);


let command: Command<Effect, Event> = match event {
// ...handle the event, dispatch to other modules, etc.
};


let post_update_view = self.view(model);


// generate a diff from previous view model and current
let diff = pre_update_view.diff(&post_update_view);
let diff_command = if !diff.is_unchanged() {
let changes: Vec<_> = get_changes_from_diff::<Self::ViewModel>(&diff).collect();
ChangeNotifications::notify_many(changes)
} else {
Command::done()
};


command.and(diff_command)
}
```


We pay a small penalty for constructing the view model twice, but in practice we didn’t find the need to optimise this yet.


### Transparent granular reactivity


We’re confident that with the combination of Pathogen, Difficient and the code generation support in Kotlin and Swift, we have a scalable approach to updating the view model, which can support all the future features we want to add into the engine. It already supports collaborative editing, where the view model of the document is updated a lot, sometimes at animation speeds, and is working quite well, while not requiring the engineers to do anything special - view models are just plain data types.


We covered a lot of the surrounding infrastructure to support collaborative editing, but we haven’t yet gone into the detail of how it actually works under the hood. That’s what we will do in[the next post in this series](https://www.photoroom.com/inside-photoroom/building-live-collaboration-in-rust-for-millions-of-users-part-4) . If this sort of work sounds exciting to you, we’re hiring a head of cross-platform to provide a seamless experience to millions of users.[Learn more here](https://jobs.ashbyhq.com/photoroom/dc994a7c-e104-46e1-81c3-b88d635398b9) .
