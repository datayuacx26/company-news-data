---
schema_version: "1.0.0"
document_id: "fbf51ff8ea3bf59f1ddeade0cff5cb83bcf7e9e38a68032b07939508dddbee52"
company_key: "yc-redouble-ai"
company: "Redouble AI"
source_id: "yc-redouble-ai-rss-94bf26a72f2a"
canonical_url: "https://deconvoluteai.com/blog/like-button-with-firebase"
published_at: "2024-04-13T00:00:00+00:00"
first_seen_at: "2026-07-27T11:30:56.530208+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:4dc169ba1fc6def32ceeeb3a5fb6640d16f17eb8d08a1f9ff7c567812615793f"
---

# Creating a like button with Firebase

# Introduction


Like buttons are a simple yet powerful way to boost user interaction and gather feedback on your website with a minimum of friction for users. This post explains how this feature can be added to your NextJS or React project without the hassle of user accounts. We will walk through creating a like button powered by TypeScript and Firebase, focusing on the core logic while leaving the styling up to you.


I assume that you already have a Firebase Firestore database configured and added to your project via the Firebase SDK.


## Requirements


Before we dive into code, let's outline the core requirements for our like button functionality:


- **Ease of use:** Users should be able to like a post without the hurdle of creating accounts or signing in. This promotes a low-friction interaction.
- **Single vote:** To ensure the integrity of the feedback, we will restrict users to liking a piece of content only once.
- **Instant feedback:** After a user clicks the like button, they should see immediate visual confirmation that their like has been registered. This reinforces the sense of interaction.
- **Minimized reads:** For cost reasons as well as performance, we want to limit the amount of reads to a minimum.
- **Focus on functionality:** While aesthetics are important, this tutorial will concentrate on the underlying mechanism of the like button. Styling and animations can be customized later to match your project's look and feel.


# Defining the data model


Let's begin by choosing a structure for storing our like data in Firebase Firestore that will be both efficient and scalable for our application. We can then use the data model to guide our implementation.


## Firestore Structure


Firestore's flexibility allows for different approaches. For this project, we will create a top-level collection called` posts` . Each document within this collection represents a single post, with the document's ID being an encoded version of the post's URL. This ensures a unique identifier while accommodating special characters in URLs.


## Document Structure


The document itself will hold a simple object:


```text
{ likes: <count> }
```


This structure has the benefit that it keeps likes directly associated with their corresponding posts. Additionally, it makes adding more data for a post, for example comments, easy.


One drawback of using an encoded URL is that if the URL of a post changes, the corresponding record cannot be found easily by looking at the URL. However, such migrations are best handled programatically, and there we can simply add the encoding step.


Here is an example of a record:


```text
posts/<encoded-url>    // Document ID
{   likes  :     5     }          // Document content
```


## How to create an initial record


Let's determine the best strategy for populating our Firestore database with initial like records. Since simplicity and minimizing reads are priorities, here's a breakdown of your options:


**1. On New Post Creation:**


We could create the record whenever a new post is created. However, this might not be easy to implement depending on how you create posts. Additionally, separate logic is required for existing posts.


**2. On First Like (Lazy Initialization):**


The record could be created when the first like for a post is given. This is a simple solution and works for both new and existing posts. There is potentially a slight delay for an initial like as the record is created, but this can be handled with optimistic updates.


**3. On Entry Not Found:**


Similar to option 2, but here the record is created when it is not found on page load. This means once a page is viewed for the first time a record is created. This solution works well if you always want to display like counts, even if a post has not received any likes yet. The drawback compared to option 2 is that is is slightly more complex, and introduces potentially more writes.


### Decision


Given our requirements, option 3 (on entry not found) seems the most suitable. It is a simple to implement solution, allows for displaying likes even if a post has not received any yet, and populates the database with records which could be used for further data.


# Creating button component


Now that we have the requirements and a data model defined, let's implement the button with a focus on the function.


Let's begin with some initial code:


```text
const LikeButton = ({ pageUrl }: { pageUrl: string }) => {
// TODO
return (
<div>
<button onClick={}>
// TODO
</button>
</div>
);
};
```


Let's explain it in detail and expand it.


## Getting the like count


To display the current like count for a page, our like button component has a couple of options:


**Fetch on client-side:** We will fetch the like count from Firebase Firestore directly within the component. This is ideal when you want to ensure the button always reflects the most up-to-date count, even if the like data changes elsewhere.


**Provide as prop:** The parent component (potentially a server-side rendered component) could fetch the like count and pass it to the button as a prop. This is efficient if the like count is unlikely to change frequently while the user is on the page.


To meet the instant feedback requirement, we will go with client-side fetching. The function to fetch the data encodes the url, and then reads the collection from Firestore. Note that this fails if the collection does not exits. This aligns with our chosen strategy for creating the database record when an entry is not found. To interact with Firebase, we use the[Firebase SDK](https://firebase.google.com/docs/web/setup) .


```text
async function getPostLikeCount(url: string) {
const encodedUrl = btoa(url);
try {
const docSnapshot = await getDoc(doc(database, "posts", encodedUrl));
if (docSnapshot.exists()) {
return docSnapshot.get("likes")
} else {
// Create the record here.
return 0;
};
} catch (error) {
console.log(error);
return 0;
};
};
```


To trigger this function once our button component is mounted, we call it from a` useEffect` hook. Because we use that hook, we must mark the button component with the directive` "use client"` if you use NextJS, as required by` useEffect` (and later` useState` ). Here is our updated component.


```text
"use client";


import { useState, useEffect } from "react";


const LikeButton = ({ pageUrl }: { pageUrl: string }) => {
const [currCount, setCurrCount] = useState(0); // Initialize the count.


useEffect(() => {
const fetchData = async () => {
try {
const count = await getPostLikeCount(pageUrl);
setCurrCount(count);
} catch (error) {
console.log("Error fetching data:", error);
}
};
fetchData();
}, []);


return (
<div>
<button onClick={}>
// TODO
</button>
</div>
);
}
```


We initialize the` currCount` state variable with` 0` as an initial count, and update it as soon as we have fetched the actual count from the database. This means there is a slight delay when the page loads. But if you add the button at the bottom of a page, most users will not notice.


## Liking a post


To increment the like count for a post when a user clicks the like button, we create a` handleLike()` function. This function is triggered by a click on the button. It then calls the function` incrementPostLikeCount()` to update the like count in the database.


```text
async function handleLike() {await incrementPostLikeCount(pageUrl) };


async function incrementPostLikeCount(url: string) {
const encodedUrl = btoa(url);
try {
const docRef = doc(database, "posts", encodedUrl);
await updateDoc(docRef, { likes: increment(1) });
} catch (error) {
console.log(error);
};
};
```


Here, we encode the URL and update the record in Firestore. We will see how this is embedded into the button code next, but first we have to address the instant feedback requirement.


## Providing instant feedback


When a user likes a post, we want to provide immediate visual confirmation without waiting for the database update to complete. Here's how we can achieve this.


**Optimistic Updates:** Update the like count and button appearance instantly in the user interface, as if the database operation has already succeeded. This gives the perception of a super-responsive app.


**State Management:** Introduce a state variable` liked` to track whether the user has clicked the like button, and conditionally display a "liked" version of the button.


**Background Update:** After the optimistic update, perform the actual Firestore increment in the background. This is done within the` handleLike()` function using` incrementPostLikeCount()` .


```text
"use client";


import { useState, useEffect } from "react";


const LikeButton = ({ pageUrl }: { pageUrl: string }) => {
const [liked, setLiked] = useState(false);
const [currCount, setCurrCount] = useState(0);


useEffect(() => {
// Get like count
// ...
}, []);


async function handleLike() {
setLiked(true); // Optimistic update for instant feedback
setCurrCount(currCount + 1); // Optimistic count update


try {
await incrementPostLikeCount(pageUrl); // Actual database update
} catch (error) {
// If the database update fails, revert the changes
console.error("Error updating like count:", error);
setLiked(false);
setCurrCount(currCount - 1);
}
};


return (
<div>
<button onClick={}>
{liked ? "Post liked" : "Not liked"}
<p >{currCount}</p>
</button>
</div>
);
}
```


Using the states` liked` and` currCount` , we can now conditionally render the button to provide feedback to the user.


The last requirement we have to meet is the one vote requirement, so let's do this next.


## One vote requirement


To prevent users from liking the same post multiple times, and to visually indicate whether they have already liked a post, we'll leverage the browser's` localStorage` . This approach aligns well with our goal of minimizing friction, as it doesn't require user accounts.


Here's how we'll implement this:


**Storing liked posts:** We will maintain an array of liked post URLs directly within` localStorage` .


**State and useEffect:** We will use the existing` liked` state variable to track the button's status and the` useEffect` hook to check` localStorage` when the component loads initially. This lets us determine if the user has already liked the current post and update the button's appearance accordingly.


Let's dive into the code changes needed for this functionality.


```text
useEffect(() => {
// Fetch initial like count
// ...


const likedPosts = JSON.parse(localStorage.getItem("likedPosts") || "[]");
setLiked(likedPosts.includes(pageUrl)); // Check if already liked
}, []);
```


Here, we read the` likedPosts` array from the browser's cache or set it to an empty array if it does not exist when the post loads. Then we set out` liked` variable to` true` if the current URL is in the array.


To mark the current post as liked by the user, we add the URL to the brower's cache. If a user has already liked the post, we exits early here. This works because when the page loads, we set the like state to true with the code above.


```text
async function handleLike() {
if (liked) return;  // Early exit if already liked


setLiked(true);
setCurrCount(currCount + 1);


try {
await incrementPostLikeCount(pageUrl);


// Read from localStorage or initialize an empty array, then add the current post to it.
const likedPosts = JSON.parse(localStorage.getItem("likedPosts") || "[]");
likedPosts.push(pageUrl);
localStorage.setItem("likedPosts", JSON.stringify(likedPosts));


} catch (error) {
console.error("Error updating like count:", error);
setLiked(false);
setCurrCount(currCount - 1);
}
}
```


For the optimistic update we set` liked` to` true` and increment the count before we attempt to increment the counter in the` try` block. If this fails, we revert these two changes in the` catch` block.


## Final button


And that is it. Putting it all together, we get the following final code for the button.


```text
"use client";


import { useState, useEffect } from "react";
import {IconFilled, Icon} from *;


const LikeButton = ({ pageUrl }: { pageUrl: string }) => {
const [liked, setLiked] = useState(false);
const [currCount, setCurrCount] = useState(0);


useEffect(() => {
const fetchData = async () => {
try {
const count = await getPostLikeCount(pageUrl);
setCurrCount(count);
} catch (error) {
console.log("Error fetching data:", error);
}
};
fetchData();


const likedPosts = JSON.parse(localStorage.getItem("likedPosts") || "[]");
setLiked(likedPosts.includes(pageUrl));
}, []);


async function handleLike() {
if (liked) return;


setLiked(true);
setCurrCount(currCount + 1);


try {
await incrementPostLikeCount(pageUrl);


const likedPosts = JSON.parse(localStorage.getItem("likedPosts") || "[]");
likedPosts.push(pageUrl);
localStorage.setItem("likedPosts", JSON.stringify(likedPosts));


} catch (error) {
console.error("Error updating like count:", error);
setLiked(false);
setCurrCount(currCount - 1);
}
}


return (
<div>
<button onClick={handleLike}>
{liked ? < IconFilled /> : <Icon />}
<p >{currCount}</p>
</button>
</div>
);
}
```


# Conclusion


We have built a robust like button for Next.js applications, powered by TypeScript and Firebase. By using simple techniques like optimistic updates and browser-side storage, we have ensured a seamless, frictionless user experience. This is important because even seemingly small features like this can significantly enhance the way people interact with your website.


And finally, if you found this post helpful, give it a like below!


# References


- [Firestore Data Model](https://firebase.google.com/docs/firestore/data-model)
- [Firestore add data](https://firebase.google.com/docs/firestore/manage-data/add-data)
- [Firestore read data](https://firebase.google.com/docs/firestore/query-data/get-data)
- [React hook useEffect](https://react.dev/reference/react/useEffect)
- [What are optimistic updates](https://medium.com/@kyledeguzmanx/what-are-optimistic-updates-483662c3e171)
