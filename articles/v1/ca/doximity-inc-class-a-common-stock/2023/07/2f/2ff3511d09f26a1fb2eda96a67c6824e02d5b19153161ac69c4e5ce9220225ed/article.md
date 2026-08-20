---
schema_version: "1.0.0"
document_id: "2ff3511d09f26a1fb2eda96a67c6824e02d5b19153161ac69c4e5ce9220225ed"
company_key: "doximity-inc-class-a-common-stock"
company: "Doximity Inc."
source_id: "doximity-inc-class-a-common-stock-rss-4199c1c9997e"
canonical_url: "https://technology.doximity.com/articles/managing-state-in-vue-applications-the-composable-provider-pattern"
published_at: "2023-07-21T07:23:00+00:00"
first_seen_at: "2026-07-25T01:15:21.586776+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:a3654d41b629f3c2e12537b9cf5ad31e4d81f643cd2b7d1b9ef3f61e18b60433"
---

# Managing State in Vue Applications: The Composable Provider Pattern

State management is a fundamental concept of front-end application design, and a critical aspect in building robust Vue applications. As Vue applications grow in complexity, managing state becomes an increasingly challenging task, especially when working with a large organization of engineers spread across various product teams. Fortunately, the Vue and Nuxt ecosystem provides a variety of state management solutions, each with its own trade-offs. In this blog post we will explore the Composable Provider pattern, which handles state management by combining the[Vue Composition](https://vuejs.org/guide/extras/composition-api-faq.html) and[Provide/Inject](https://vuejs.org/guide/components/provide-inject.html) APIs, and why its clean and simple implementation is a powerful approach to handling complex, feature-driven state in modern Vue applications.


*Note: This article was originally intended as a reference for our internal engineering team, but given its potential general use among Vue application developers we are publishing it here. We hope you’ll find this pattern helpful when designing your Vue application’s state as we have here at Doximity!*


## Why Efficient State Management Matters


Efficient state management plays a crucial role in the development of Vue applications. As applications grow in complexity, the ways in which we manage and handle changes to state become increasingly important. Here are a few key reasons why managing state efficiently matters:


1. Scalability: As Vue applications and the teams building them grow in size, the complexity and size of their state grow also. Efficient state management allows for better scalability by providing a structured and organized approach to handling state. It becomes easier to add new features, maintain existing functionality, and reason about the flow of data within the application.
2. Code Maintainability: Well-managed state reduces code complexity and promotes code maintainability. When state is not encapsulated properly, it becomes challenging to understand and modify the application’s behavior. By implementing sound state management practices, such as the Composable Provider pattern, we can make it easier to maintain and refactor the code base.
3. Reusability: Efficient state management enables the reuse of state and logic across multiple components. When state is decoupled from individual components and managed separately, it can be easily shared and reused in different parts of the application. This promotes code modularity and reduces duplication, leading to cleaner and more reusable code.
4. Performance: Proper state management can significantly impact the performance of a Vue application. Inefficient state management can lead to unnecessary re-rendering of components, excessive memory usage, and reduced application responsiveness. By managing state efficiently, we can minimize unnecessary updates and ensure that only the necessary components react to state changes, thereby optimizing performance.
5. Collaboration: Clear state management practices facilitate collaboration among engineers. When state is well-organized and documented, it becomes easier for team members to understand and work on different parts of the application. Additionally, efficient state management simplifies debugging and troubleshooting, as the state-related issues are isolated and can be easily traced and resolved. By prioritizing efficient state management, we can create Vue applications that are performant, scalable, maintainable, and reusable. In the next section, we will introduce the Composable Provider pattern, a powerful approach to managing state in Vue applications, and explore its benefits in detail.


## State Management in Vue Applications


Managing state in an application involves storing and synchronizing data that drives the application’s appearance and behavior across various components. Vue provides several techniques for state management, each catering to different application sizes and complexities. Before diving into the Composable Provider pattern, let’s explore some of the other options for managing state in Vue.


### Local Component State


Vue components have their own local state, either with` data()` in the Options API, or with` ref()` in the Composition API. In either case, this state is reactive, meaning that changes to the data encapsulated by the state will automatically update elsewhere in the component, which is great for making easy UI changes. However, because this state is confined to the component, it is only suitable for small-scale state that doesn’t need to be shared or utilized by other components.


### Props and Custom Events


Vue components can share state between each other by passing props down and bubbling custom events up. Props flow top-down from parent component to child, and child components can pass data to their parent by emitting custom events. This approach works well for passing data between closely related components, but can become cumbersome when sharing state between distant components.


### Vuex and Pinia


[Vuex](https://vuex.vuejs.org/) and its successor[Pinia](https://pinia.vuejs.org/introduction.html) are structured, centralized data stores used for managing state in large applications. They both implement a[Flux-like](http://fluxxor.com/what-is-flux.html) unidirectional data flow which helps keep state clean without side effects and allows state to be accessed anywhere in the application. However, these solutions come with some inherent complexity and verbosity, requiring a non-trivial amount of boilerplate code to use. Additionally, their rigid structures require state to be located in a central store, separate from the application code using it, which can be troublesome to refactor as applications evolve and business requirements change. Still, these libraries are tried and tested solutions and are worth considering when building large Vue applications.


## The Composable Provider Pattern


The Composable Provider pattern is a state management technique for Vue that combines the power of the Provide/Inject and Composition APIs. It offers a flexible and composable way to manage state in Vue applications, promoting code reusability, encapsulation, and maintainability.


The Composable Provider pattern revolves around creating a provider composable and a corresponding context composable. The provider composable encapsulates the state and serves as the central store of data, which it provides to its descendant components via Vue’s Provide API. The context composable, in turn, injects that data into any descendant component of the provider using Vue’s Inject API. By utilizing these two composables in tandem, we can be flexible about where we locate state in our application, whether globally at the top-level of the application, or more locally within a particular layout or group of components.


From an implementation perspective, this looks something like:


```text
// Page.vue
import    {    useMyFeatureProvider    }    from    '~/composables/useMyFeature'  ;
export    default    {
setup  ()    {
useMyFeatureProvider  (  /* any initial state or options */  );
},
}


// SomeComponentOnThePage.vue
import    useMyFeature    from    '~/composables/useMyFeature'  ;
export    default    {
setup  ()    {
const    {
someStateA  ,
someStateB  ,
updateDataMethod  ,
}    =    useMyFeature  ();
const    handleButtonClick    =    ()    =>    {
updateDataMethod  (  /* some data */  );
}


return    {
someStateA  ,
someStateB  ,
handleButtonClick  ,
};
},
};


```


The provider composable acts as a central source of truth for the state in the application. It sets up the reactive state using the Composition API and exposes the necessary methods and computed properties to interact with that state. By using the Provide/Inject API, the provider composable makes the state and its associated methods accessible to all its descendant components, regardless of the component hierarchy, avoiding the tedium of[prop-drilling](https://vuejs.org/guide/components/provide-inject.html#prop-drilling) .


Child components can then use the context composable, which in turn uses the inject function from the Composition API, to access the state provided by the provider composable. This allows them to read and modify the state as needed, ensuring a clear separation of concerns and promoting reusability.


This pattern lends itself well to feature-driven state management, as we can easily create as many sets of Composable Providers as we desire, such as one for each bespoke collection of feature data. Additionally, due to its use of Vue’s Composition API it follows the principles of composition, meaning that we can combine smaller data stores into larger related ones as needed, keeping our state management modular and reusable.


### How it Differs from Other State Management Solutions in Vue


The Composable Provider pattern differs from traditional state management solutions like Vuex in a few key ways. While Vuex provides a centralized store that manages the application's state, the Composable Provider pattern takes a more decentralized approach. Instead of having a single store, each provider composable encapsulates its own state, allowing for more fine-grained control and modularity.


Additionally, the Composable Provider pattern leverages the Composition API, which provides a more intuitive and flexible way to manage state compared to the options offered by Vuex. It allows engineers to use composable functions to access and modify the state, promoting code reuse and encapsulation.


### Writing a Composable Provider


Creating a Composable Provider is a relatively straightforward process. Start with a simple composable:


```text
// ~/composables/useCounter.js
function    useCounter  ()    {
const    count    =    ref  (  0  );


const    addOne    =    ()    =>    {
count  .  value    =    count  .  value    +    1  ;
}


return    {
count  ,
addOne  ,
};
}


```


This composable function works on its own for keeping track of the count state and mutating it with the` addOne()` method. However, because the state is local to the composable, each component calling it will have its own instance of count state! To make this state shared across all components, we need to combine the Composition API with Provide/Inject. We’ll accomplish this with a simple` createProvider()` factory:


```text
// ~/utils/createProvider.js
export    default    function    createProvider  (  key  ,    composable  )    {
const    keySymbol    =    typeof    key    ===    'symbol'    ?    key    :    Symbol  (  key  );


const    useContext    =    ()    =>    {
const    context    =    inject  (  keySymbol  );


if    (  !  context  )    {
throw    new    Error  (
`Attempted to access context outside of provider for   ${  keySymbol  .  toString  ()}  `  ,
);
}


return    context  ;
};


return    [
(...  args  )    =>    {
const    context    =    composable  (...  args  );
provide  (  keySymbol  ,    context  );
return    context  ;
},
useContext  ,
];
}


```


Those who have used[VueUse](https://vueuse.org/shared/createInjectionState/) ’s` createInjectionState()` function may find this familiar – it uses the Composable Provider pattern under the hood! This factory takes a key and a composable and returns a tuple containing the provider composable and the context composable.


- The key is used to ensure that the context is unique so that we can inject the provided state without collisions from other providers. This is accomplished by creating a Symbol off of the key string, guaranteeing that each invocation of the factory returns a uniquely identifiable provider and matching context.
- The composable we pass to the factory is wrapped by the provider composable, which provides the composable’s return value using the Vue Provide/Inject API. The composable’s return value is also returned by the provider composable so that we can use state directly from the provider as well.
- The context composable injects this provided context using the unique Symbol created by the factory, so that any component calling` useContext()` will share the same provided context. If` useContext()` does not find anything on the given Symbol, this means it was called outside of its corresponding provider, so we throw an error.


With this basic factory we can turn any composable into a Composable Provider very simply. Using our counter example from earlier:


```text
// ~/composables/useCounter.js
import    createProvider    from    '~/utils/createProvider'  ;


function    useCounter  ()    {  /* ... */  }


const    [  useCounterProvider  ,    useCounterContext  ]    =    createProvider  (  'Counter'  ,    useCounter  );


export    {    useCounterProvider    };
export    default    useCounterContext  ;


```


We call the` createProvider()` factory with an identifiable string “Counter” and the` useCounter()` composable and receive the provider/context tuple in return. Finally all we have to do is export the provider composable and by default its matching context composable. We use the context composable as the default export since the provider is typically used just once, but the context might be used many times throughout the application. Now in our application we can use this Provider Composable and the state will be shared throughout:


```text
// Page.vue
import    {    useCounterProvider    }    from    '~/composables/useCounter'  ;
export    default    {
setup  ()    {
useCounterProvider  ();
},
};


// DisplayComponent.vue
import    useCounter    from    '~/composables/useCounter'  ;
export    default    {
setup  ()    {
const    {    count    }    =    useCounter  ();


return    {    count    };
},
};


// IncrementButtonComponent.vue
import    useCounter    from    '~/composables/useCounter'  ;
export    default    {
setup  ()    {
const    {    addOne    }    =    useCounter  ();


return    {    addOne    };
},
};


```


Now, the` DisplayComponent` and` IncrementButtonComponent` , which are both descendants of the Page, will share the same counter state defined in the original` useCounter()` composable, thanks to the Provide/Inject API and our Composable Provider. These components can be nested anywhere in the application, so long as they are descendants of the Page component where` useCounterProvider()` is invoked. This allows us to simply and cleanly access state anywhere without the burden of passing props and emitting custom events across several levels or setting up a central store. In other words, our state management solution with the Composable Provider pattern is performant, maintainable, and reusable.


Next, we’ll take a look at how the Composable Provider pattern is also scalable as we see how to leverage the power of composition to combine separate pieces of state.


### Composing State with Multiple Composable Providers


We’ve already seen how powerful combining Provide/Inject with Composition API can be, but now let’s take a look at what can be accomplished using composition. Because our Composable Providers are, as named, composable, we can combine them to share state between them without breaking encapsulation. This is useful because it allows us to create individual Composable Providers to encapsulate bespoke feature state and business logic while still benefiting from shared state coming from elsewhere in the application.


Let’s say for instance that we want to create a new feature that uses the count from our Counter feature to reset the count after it reaches 10. (For the sake of example, we want this to be a separate feature, not part of the existing Counter feature.) We can pass the count ref along to a new Composable Provider which will utilize Vue’s reactivity to watch the value and reset it once it reaches 10. To do this, we’ll use composition, passing the ref from our` useCounter()` composable to our new` useResetCounter()` composable:


```text
// ~/composables/useResetCounter.js
import    createProvider    from    '~/utils/createProvider'  ;


function    useResetCounter  (  counterRef    /* this is a reactive variable */  )    {
const    resetCount    =    ()    =>    {
counterRef  .  value    =    0  ;
};


watch  (  counterRef  ,    value    =>    {
if    (  value    >=    10  )    {
resetCount  ();
}
});


return    {
resetCount  ,
count  :    counterRef
};
}


const    [
useResetCounterProvider  ,
useResetCounterContext  ,
]    =    createProvider  (  'ResetCounter'  ,    useResetCounter  );


export    {    useResetCounterProvider    };
export    default    useResetCounterContext  ;


// Page.vue
import    {    useCounterProvider    }    from    '~/composables/useCounter'  ;
import    {    useResetCounterProvider    }    from    '~/composables/useResetCounter'  ;
export    default    {
setup  ()    {
// Note: We can also pull context off of the provider
const    {    count    }    =    useCounterProvider  ();
useResetCounter  (  count  );
}
};


```


Now, any time count reaches 10,` useResetCounter()` will reset its value to 0. Furthermore, because of Vue’s reactivity, we can mutate count from either` useCounter()` or` useResetCounter()` and we’ll refer to the same state! In our application we can get the value of count using this shared state from either our` useCounter()` or` useResetCounter()` feature. This contrived example may not look like much, but in a large and complex application the ability to share state across separately encapsulated composables is tremendously powerful.


## Key Features and Benefits of the Composable Provider Pattern


The Composable Provider pattern offers a range of features and benefits that make it a powerful approach to state management in Vue applications. Let's explore some of its key advantages:
Reusability and Maintainability of Code: The Composable Provider pattern promotes code reusability by encapsulating state and logic within the provider composable. Composables can be easily shared and used across multiple components, reducing code duplication and enhancing the maintainability of the codebase. This modular approach simplifies updates, refactoring, and collaboration among team members.


- Flexibility in Managing Different Types of State: The Composable Provider pattern provides flexibility in managing various types of state within a Vue application. Since each provider composable encapsulates its own state, you can create multiple provider composables, each responsible for a specific area of state. This allows for fine-grained control and separation of concerns, enabling efficient management of complex state requirements.
- Encapsulation of Business Logic for Cleaner Component Code: By encapsulating state and related business logic within the provider composable, the Composable Provider pattern promotes clean and concise component code. Components that consume the state can focus solely on rendering and interacting with the state, without being burdened by the details of state management. This encapsulation enhances the readability and maintainability of component code, making it easier to reason about and modify.
- Improved Performance through Optimized Reactivity: The Composable Provider pattern leverages Vue's reactivity system to optimize performance. By utilizing reactive state and ensuring that only the components that depend on the state react and update, unnecessary re-rendering and performance bottlenecks are minimized. This leads to improved rendering efficiency and a smoother user experience. Easy Integration with Vue's Composition API: The Composable Provider pattern seamlessly integrates with Vue's Composition API, which provides a more intuitive and flexible approach to managing state in Vue applications. By leveraging the Composition API's capabilities, such as reactive state, computed properties, and lifecycle hooks, the Composable Provider pattern enables efficient state management while taking full advantage of Vue's reactive programming paradigm.
- Enhanced Testability: The Composable Provider pattern promotes testability by encapsulating state and logic within the provider composable. Since the state and logic are isolated, it becomes easier to write unit tests for individual composable functions, ensuring the correctness of state management operations. This facilitates comprehensive testing of the state management layer, leading to more robust and reliable applications.


## Best Practices for Effective State Management


When utilizing the Composable Provider pattern for state management in Vue applications, it's important to follow best practices to ensure efficient and maintainable code. Let's explore some of the key practices for effective state management:


- Identifying State Requirements and Granularity: Before implementing the Composable Provider pattern, carefully analyze the state requirements of your application. Determine the granular level of state needed and identify the specific data that should be managed. Breaking down the state into smaller, focused units promotes modularity and makes it easier to reason about and manage the state.
- Proper Organization and Naming Conventions for State Variables: Organize state variables within the provider composable in a logical and consistent manner. Group related state variables together and use descriptive naming conventions to enhance readability and understanding. For example, the convention of prefixing composables with the word “use” as in` useCounter()` helps to convey the function’s intent. This practice helps other engineers, including your future self, easily navigate and comprehend the state structure.
- Techniques for Managing Complex State Relationships: In applications with complex state relationships, consider employing techniques like module-based organization or sub-providers within the Composable Provider pattern. For example, consider breaking down your application into layers like Presentational, Business Logic, and Network. Breaking down the state into modules or utilizing sub-providers for specific areas of the application can help maintain a clear and manageable structure. This allows for easier maintenance and scaling as the application grows.


By following these best practices, you can establish a solid foundation for effective state management using the Composable Provider pattern. This will result in a well-structured, maintainable, and performant Vue application.


## Summing Up


The Composable Provider pattern is a powerful technique for managing state in Vue applications by combining the composability of Composition API with the access of Provide/Inject. It gives engineers the convenience of accessing state anywhere like Vuex, but with more flexibility around location and encapsulation. The Composable Provider pattern shines with its simplicity and clean API, allowing granular control over state management without much boilerplate code or configuration. It’s a scalable, maintainable, reusable, and performant solution to state management ready to use in any Vue application!


Special thanks to Pierre Lebrun, Emiliano Zilocchi, and Jeric de Leon for peer reviews of this article and to Chloe Chan for the illustrations.


---


Be sure to follow[@doximity_tech](https://twitter.com/doximity_tech) if you'd like to be notified about new blog posts.
