---
schema_version: "1.0.0"
document_id: "110e6a5a5972d1ea40dcb7cb94939e83041a529e4cc5ce9fe3e2c5fb01d18611"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/android-new-architecture/"
published_at: "2018-02-06T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:26:53.665310+00:00"
content_hash: "sha256:4d9514f745a1e4bfd05efff6ea7ee3ccc881ea40bf173d5b31c55fdb331fa289"
---

# Rewriting the trivago Android app: challenges and lessons learnt

## Project Aurora


”Yes! We are rewriting our app, completely from scratch!”


Exciting times were ahead of us, but as a wise man once said: great power carries great responsibility.


There we were, facing what could be a once-in-a-lifetime chance to build an app from bottom up. But how could we exceed the expectations of so many users with the first release on day 1?


Obviously, the first thing you think in this kind of situation is: “Finally! Bye bye legacy code, no more refactoring, everything brand new and shiny standing exactly how we want and need it”. But to reach this ideal scenario, a series of decisions had to be made.


Not only was this a challenge for our architectural goals, but also for everyone else involved, including product owners, designers, and project managers. Having such a diverse team with great knowledge, allowed us to take advantage of its expertise when tackling hot topics such as cross-functional teams, self-organizational workflow, MVP goals definition and UX new and modern practices.


In this post, I will try to explain our reasoning and decision making. I will not dive deep into the details of the architecture we ended up using, but rather try to focus on the decisions we made, the problems we faced down the road, and what we learnt from them.


## Architecture


Now that I have described the situation, I can further explain how we approached this opportunity. Our goal was clear: don’t let the architecture get in the way. Architecture should be something transparent that allows developers to build **anything** seamlessly (emphasizing the word anything because at this point, we didn’t have any idea of what features were to come). We call that in software, **scalable** .


Of course, it’s not only about building new features on top of each other, those features are also going to stay there for some time and you want to make sure they remain working no matter how many other cool things come. There is an easy way of doing this: tests. Therefore, the architecture should also be **maintainable** and **testable** .


Last but not least, it also has to be used and owned by all team members. So it needs to be **understandable** and easy to use. This is where tradeoffs come in and the challenge of finding the balance between good practices and not over engineering needs to be accomplished.


### Clean architecture


With these principles in mind, we started evaluating different paradigms out there that fulfilled our goals and we came across **clean architecture** or in other words… keep the details exactly that, *details* .


We didn’t want to look at the whole thing with the mission statement “We are building an Android app”. But we wanted to shift the way of thinking to “we are building a client application to enable users to discover and book their ideal hotel”. The fact that it runs on Android was an implementation specification. That is how we tried to think. Trying to put more focus on the problem itself and not on the tools or methods we use to solve it.


If you are interested and want to know more about this approach, we gave a talk at the GDG Google I/O extended 2017 in Heraklion regarding this topic, you can check out the[slides](https://speakerdeck.com/rortega/clean-architecture) .


So, once the general mindset is established, let’s dive into what we learnt from trying to apply it to a real scenario like our Android app:


### Lessons


The first and general lesson would be: don’t be afraid of spending as much time as needed in discussions. This will probably save tons of time afterwards in code reviews, refactoring and struggles keeping your system healthy. Furthermore, making decisions as a team will increase the feeling of ownership of everyone involved, allowing the whole team to remember the reasoning behind the conclusions reached.


Other lessons we learnt from this process were:


#### Use cases management might not be trivial


When you think of domain logic, it is likely that you just create an instance of a use case for every operation that your app will perform. Nevertheless, this has to be well thought through. Try to answer these questions:


- What happens to the instances of use cases once the operation is finished?
- Can I have different instances of one use case? Is my dependency management system ready for that?
- When should a use case perform the operation and when should it just return the last value?
- What is to be delivered by the use case? Just the success? Should it handle the error, or just pass it to upper layers? Any change in the parameters?
- How long should use cases remain in memory? Who is responsible for cleaning them up when they are no longer needed?
- How should use cases behave during rotation?


Generally, even if use cases are at first sight concise and straightforward, you really need to consider all the situations because they are going to be all over the place and perform every crucial operation your app needs to carry out.


For us, that meant allowing our dependencies to host different instances of a use case to make them available even after rotation and create two different subclasses of them. One that will act like a[BehaviorSubject](http://reactivex.io/RxJava/2.x/javadoc/io/reactivex/subjects/BehaviorSubject.html) , meaning it will emit the result as soon as anyone starts listening to it and another one (based on a[PublishRelay](http://jakewharton.github.io/RxRelay/com/jakewharton/rxrelay2/PublishRelay.html) that will emit the result only after it is executed.


#### Integration testing can be as valuable as unit testing


With this architecture one of the effects we have is more classes that are smaller and basic, with “trivial” methods that often consist just of one single call to another dependency. As an example:


```text
/**
* Use case to check if an email is already registered at trivago
*/
class   CheckIfAccountExistsUseCase  (initialState: BaseUseCase.State<  String  ,   Boolean  >?,
private   val mAccountsRepository: IAccountsRepository) :
PublishUseCase<  String  ,   Boolean  >(initialState) {


// BaseUseCase


override fun   onExecute  (  email  :   String  ?): Observable<Result<  Boolean  >>:
if   (  email  :=   null   || !  email  .  isEmail  ()) {
Observable.  just  (Result.  Error  (  InvalidParameterException  (  "you should provide an email"  )))
}   else   {
mAccountsRepository.  doesAccountExist  (email)
}
}
```


As you can see, this use case is basically returning the output of a repository. This is a very simple case but is not that rare to come across use cases that are as concise as this one.


Because of this, unit testing becomes so simple that developers usually get the feeling that it is not bringing any value to the code base. Although one can understand that point of view, this is not always true and unit testing can’t just be dropped. Because even if it is just one call, we have to make sure that it happens, and the possible outcomes are properly mapped into what the upper layer expects. Something like this:


```text
class   CheckIfAccountExistsUseCaseSpecs   : FeatureSpec({


feature  (  "Emits an error if no email was provided"  ) {


val repository  :   IAccountsRepository  :   mock  ()


val useCase  :   CheckIfAccountExistsUseCase  (  null  , repository)


val testObserver  :   TestObserver  <  Result  <  Boolean  >>  ()
useCase.  result  ().  subscribe  (testObserver)


scenario  (  "an error is emitted if the email was null"  ) {
useCase.  execute  (  null  ,   true  )
testObserver.assertValue {
(it as Result.Error  <  Boolean  >  ).throwable  !!  .message.  equals  (  "you should provide an email"  )
}
}


scenario  (  "an error is emitted if the email was not valid"  ) {
useCase.  execute  (  "asdf"  ,   true  )
testObserver.assertValue {
}
}


scenario  (  "the result of the repository is emitted if a valid email is given"  ) {
val result  :   Result.  Success  (  true  )
`when`(repository.  doesAccountExist  (  "valid@email.com"  ))
.  thenReturn  (Observable.  just  (result))


useCase.  execute  (  "valid@email.com"  ,   true  )


testObserver.  assertValue  (result)
}
}
})
```


What we can do in order to increase the confidence in the code for the most concerned developers is testing how these classes interact with each other. Actually, since we are using this new approach, the complexity of classes is reduced, but the amount of them can be higher, and a good portion of our logic remains in the interactions of the classes. So, it is a good idea to also test this interaction: here is where integration tests come to the rescue:


```text
class   DealDetailsViewModelIntegrationSpecs   : FeatureSpec({


feature  (  "Loading the deal details"  ) {


scenario  (  "should emit an error if no hotel details object could be loaded"  ) {


// Prepare deal use case
`when`(mockDealsMemorySource.  loadDeal  (Mockito.  anyString  ())).  thenReturn  (Observable.  never  ())


// Prepare hotel details use case
`when`(mockHotelDetailsNetworkSource.  performHotelDetailsSearch  (Mockito.  anyString  ()))
.  thenReturn  (Observable.  error  (  RuntimeException  (  "Some network error!"  )))


// Load the deal details
viewModel.  loadDealDetails  (  "ru"  ,   "barb"  )


// Get the latest error
val error  :   viewModel.  error  ().  blockingFirst  ()


// Check the error
error.  shouldBe  (  "Some network error!"  )
}


})
```


In this test, we are not exactly testing a method, but that the whole interaction among all the components works as expected. We have a use case that has no cached value (we mock the memory source to never emit anything) and a failing network request. As a result, we don’t know exactly which method will be delivering the error, or in which form. What we are interested in, is that the view model emits an error through its observable.


This adds a new dimension to the quality of our testing.


#### Data flow is not the same as dependency flow (domain-data conflict)


This is, in my opinion, a really interesting discussion and we had it in a lot of contexts, also with iOS people. The question is: if the domain layer needs data from the data layer, does it have to rely on it (have a dependency on it)?


NO.


Where the data is coming from means nothing to the domain. That’s why we don’t directly ask the sources to deliver data, but the repositories. So even if the data flow looks like this:


The dependencies remain like this:


There is also a[long discussion](https://github.com/android10/Android-CleanArchitecture/issues/136) about this topic that I found particularly enlightening:


The short version of it is that the core of your application must be completely independent. It should only expose interfaces that define how it can get whatever it needs to perform the business logic and then other modules will implement these interfaces using any framework that should be beyond the visibility of our core.


### Takeaways


In this process, it is important that we listen to the needs of our colleagues, they will be the ones building the features that impact the user and create the wow-effect we are looking for. Architecture should only be there to provide the base ground for them to be able to shine.


If you have any suggestion, other thoughts about this, or you are simply interested in knowing more about any specific aspect. Don’t hesitate to leave a comment here or drop me a message!


Of course, special thanks to all the Android development team that contributed to making this possible.


Thanks for reading.
