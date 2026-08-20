---
schema_version: "1.0.0"
document_id: "1494a37c4a7041dcd2a7ddddc655c343eed986756f4b00aed54aa2edadb7e7a6"
company_key: "yc-meticulous"
company: "Meticulous"
source_id: "yc-meticulous-news-import-624e920f0172"
canonical_url: "https://www.meticulous.ai/blog/react-error-boundaries-complete-guide"
published_at: "2024-01-16T00:00:00+00:00"
first_seen_at: "2026-07-25T14:57:24.607504+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:fc13ba8f7eee350a7c1268ebda90d49b8d4954c35ce2f32cf3f2439e6b03a9b6"
---

# React Error Boundaries | Complete Guide

While[catching errors before they hit production](https://app.meticulous.ai/docs) is ideal, some of them, such as network errors, might slip through testing and impact your users.


If your[React](https://www.meticulous.ai/blog/how-to-do-visual-regression-testing-in-a-react-app-a-step-by-step-tutorial) components are not properly catching errors thrown by third-party libraries or React hooks, such errors either end-up crashing the React lifecycle or reaching the top-level of the main execution thread, resulting in the “white screen” scenario:


> *As of React 16, errors that were not caught \[...\] will result in unmounting of the whole React component tree*


It is crucial that your application gracefully handle such errors by providing proper visual feedback and potential actions (ex: retry mechanisms).


Fortunately, implementing such UX patterns can be achieved with little work with the React API and, for the most advanced UX, with the help of lightweight React libraries.


Using JavaScript’s try-catch around React hooks calls won’t work due to the asynchronous nature of their execution. However, React API offers the[Error boundaries](https://reactjs.org/docs/error-boundaries.html) mechanism to catch all types of errors that might “bubble out” from a component.


For example, if the <ComponentA /> is wrapped in a React Error boundary, the error propagation will stop at the Error Boundary level, preventing the React App from crashing:


This article will cover how to implement Error Boundaries in your application, from simple error catching to displaying visual feedback and providing retry mechanisms.


## Simple Error Boundaries: Catching and Reporting Errors


Behind its sophisticated name, an Error Boundary is just a plain class React component implementing the componentDidCatch(error) method:


Note: React is not yet offering a hook-based alternative to implement error boundaries.


[As showcased in this CodeSandbox](https://codesandbox.io/s/meticulous-ai-blog-error-boundaries-simple-error-boundary-l7s66f?file=/src/App.tsx) , the componentDidCatch() class method will be called as soon as an error reaches our MyErrorBoundary component, allowing us to prevent the React app from crashing and forwarding the error to our error reporting tool. (The CodeSandbox might display a development error overlay that only shows in development, you can dismiss it to see the rendering result).


Let’s make our <ErrorBoundarySimple> more friendly by adding simple visual feedback when errors are raised. For this, we add some state to ErrorBoundarySimple and use the getDerivedStateFromError() method, as follows:


```text
class ErrorBoundarySimple extends React.Component {
state = { hasError: false };


componentDidCatch(error: unknown) {
// report the error to your favorite Error Tracking tool (ex: Sentry, Bugsnag)
console.error(error);
}


static getDerivedStateFromError(error: unknown) {
// Update state so the next render will show the fallback UI.
return { hasError: true };
}


render() {
if (this.state.hasError) {
return <p>Failed to fetch users.</p>;
}


return {this.props.children};
}
}


```


With the above setup, any error in the <Chat> component (or its descendant) would be caught in the Error Boundary wrapping the <Chat> component (not the “App” Error Boundary), allowing us to give a contextualized visual feedback. However, any error coming from all <App> descendants (excluding <Chat> and <TodoList>) will be caught by the “App” Error Boundary.


With a few lines of code, we just greatly improved our user experience by gracefully handling errors in our application.


However, such simple Error Boundaries implementations do have limitations. First,[according to the React documentation](https://reactjs.org/docs/error-boundaries.html#introducing-error-boundaries) , Error boundaries do not catch errors for:


- Event handlers
- Asynchronous code (e.g. setTimeout or requestAnimationFrame callbacks)
- Server-side rendering
- Errors thrown in the error boundary itself (rather than its children)


And, the previously showcased Error Boundaries do not provide any action to the user to recover from the error, for example, with a retry mechanism. In the next section, we will see how to leverage the react-error-boundary library to handle all these edge cases.


## Advanced Error Boundaries: Catching all Errors and Retry Mechanisms


Let’s now provide a superior error handling user experience by catching all kinds of errors and exposing recovery actions to the users. For this, we will use the react-error-boundary library which can be installed as follows:


```text
npm install --save react-error-boundary


yarn add react-error-boundary


```


## Provide a Retry Mechanism


[Our new CodeSandbox](https://codesandbox.io/s/meticulous-ai-blog-error-boundaries-simple-error-boundary-retry-vx8cbl?file=/src/App.tsx:23-721) defines a <Users> component that will fail to load users 50% of the time. (The CodeSandbox might display a development error overlay that only shows in development, you can dismiss it to see the rendering result).


Let’s use react-error-boundary to properly catch errors and provide a retry mechanism:


```text
import { ErrorBoundary, FallbackProps } from "react-error-boundary";
import { Users } from "./Users";


function ErrorFallback({ error, resetErrorBoundary }: FallbackProps) {
return (
<div role="alert">
<p>Failed to load users:</p>
<pre>{error.message}</pre>
<button onClick={resetErrorBoundary}>Try again</button>
</div>
);
}


export default function App(): JSX.Element {
return (
<div className="App">
<h1>Hello CodeSandbox</h1>
<h2>Start editing to see some magic happen!</h2>
<ErrorBoundary FallbackComponent={ErrorFallback}>
{/* Users will fail to load 50% of the time */}
<Users />
</ErrorBoundary>
</div>
);
}


```


<ErrorBoundary> takes one mandatory FallbackComponent= prop that should be the react component or JSX that will be rendered in case of error. In the case of a component, this FallbackComponent= function will receive FallbackProps:


- error can be used to display the error.
- resetErrorBoundary is a callback to reset the error state and re-render the children's components.


An ononError prop can also be provided to forward the error to your favorite error reporting tool (ex: Sentry). The[react-error-boundary documentation](https://github.com/bvaughn/react-error-boundary#error-recovery) showcases how to leverage other props (ex: onReset=) to handle more advanced scenarios.


## Catching all Errors


As aforementioned, Error boundaries do not catch errors for:


- Event handlers (learn more)
- Asynchronous code (e.g. setTimeout or requestAnimationFrame callbacks)


Because such errors happen outside of the React rendering lifecycle, Error boundaries won’t be invoked. Again, react-error-boundary has us covered by providing a handleError() hook that helps with catching event-related and asynchronous errors.


```text
import { useErrorHandler } from 'react-error-boundary'


function Greeting() {
const [greeting, setGreeting] = React.useState(null)
const handleError = useErrorHandler()


function handleSubmit(event) {
event.preventDefault()
const name = event.target.elements.name.value
fetchGreeting(name).then(
newGreeting => setGreeting(newGreeting),
error => handleError(error),
)
}


return greeting ? (
<div>{greeting}</div>
) : (
<form onSubmit={handleSubmit}>
<label>Name</label>
<input id="name" />
<button type="submit">get a greeting</button>
</form>
)
}


```


Errors happening inside of handleSubmit() function won’t be caught by React rendering lifecycle. For this reason, we use the handleError function provided by react-error-boundary’s useErrorHandler() to rethrow the error in the React lifecycle so that the nearest ErrorBoundary can catch it.


## Conclusion


Behind its sophisticated name, a React Error Boundary is a straightforward way to gracefully handle any kind of error in a React application.


[Good products should prevent errors from reaching production](https://meticulous.ai/) but also should use error boundaries to provide contextual feedback and recovery actions to their users in case of unexpected errors.


## Meticulous


Meticulous creates and maintains an exhaustive suite of e2e ui tests with **zero** developer effort.


This quote from the CTO of Traba sums the product up best: "Meticulous has fundamentally changed the way we approach frontend testing in our web applications, fully eliminating the need to write any frontend tests. The software gives us confidence that every change will be completely regression tested, allowing us to ship more quickly with significantly fewer bugs in our code. The platform is easy to use and reduces the barrier to entry for backend-focused devs to contribute to our frontend codebase."


This[post](https://www.meticulous.ai/blog/lessons-from-a-decade) from our CTO (formerly lead of Palantir's main engineering group) sets out the context of why exhaustive testing can double engineering velocity. Learn more about the product[here.](https://www.meticulous.ai/)
