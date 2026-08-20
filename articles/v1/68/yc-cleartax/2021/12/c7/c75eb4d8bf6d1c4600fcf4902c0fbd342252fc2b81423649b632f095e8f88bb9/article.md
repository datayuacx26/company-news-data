---
schema_version: "1.0.0"
document_id: "c75eb4d8bf6d1c4600fcf4902c0fbd342252fc2b81423649b632f095e8f88bb9"
company_key: "yc-cleartax"
company: "Clear"
source_id: "yc-cleartax-rss-3da8b6d91e7d"
canonical_url: "https://medium.com/cleartax-engineering/a-brief-introduction-to-black-apps-json-driven-ui-in-react-native-5fd4903d97ac"
published_at: "2021-12-15T10:50:41+00:00"
first_seen_at: "2026-07-27T13:12:04.161420+00:00"
fetched_at: "2026-07-28T22:26:34.525443+00:00"
content_hash: "sha256:ab46e852496bfe2ead2771fcc6a688cb737a8b231c406777cb53c237467bc960"
---

# A brief introduction to BLACK app’s JSON-driven UI in react native

React Native


Cleartax


Mobile App Development


Json Driven Ui


Black App


# A brief introduction to BLACK app’s JSON-driven UI in react native


[Praveena Poojary](https://medium.com/@praveenpoojary?source=post_page---byline--5fd4903d97ac---------------------------------------)


3 min read


·


Dec 15, 2021


--


How we created dynamic screens and components so that anyone in the team can change the layout of the screen.


Press enter or click to view image in full size


Black by ClearTax


In[Clear](https://clear.in/) we build almost all front-end products using some variant of[react](https://reactjs.org/) and on the same line, we have built our mutual fund app,[Black](https://cleartax.in/s/black) **,** using[react-native](https://reactnative.dev/) . If you use[codepush](https://docs.microsoft.com/en-us/appcenter/distribution/codepush/) with react native releasing any change is instantaneous but it again depends on the engineers of the team. So, we wanted to make a few screens that have more power on our conversion rate so dynamic that anyone in the team can add or remove components to it without the help of an engineer.


I will start with a small example of how a JSON structure of one small part of a screen looks like. The below screenshot is a part of the Black app’s home screen.


Press enter or click to view image in full size


Screenshot from Black iOS app


**In this part of the screen, we show a list of top-performing mutual funds to buy in each category like equity, Flexi cap, and large-cap in a horizontal scroll view and each block has a “View all” CTA which takes the user to a list of all funds under a particular category.**


And this is how JSON of the above part of the screen looks like


Black App home screen JSON


**Rows**


` rows` is an array of components that we will render vertically and each` row` can contain a` title` ,` cta` and` cards`


**Cards**


` cards` is an array of` card` and each card contains an` id` and it is mapped to a react component.


```text
const components = {       "1":  VerticalListBlock  ,       "2":  CardTitleThumbnail   }
```


**CTA**


Both` card` and` row` can contain a` cta` object which will contain screen name which we can pass to` react-navigation` and` args` will contain navigation params.


Below is the simple implementation to create a screen using the above JSON. Showing only the necessary parts for simplicity.


**Handling themes in the JSON-driven system**


We maintain a dark and light theme using react context.


In our JSON instead of passing a **color hex code we pass a name of the color** like primary, error, text1, text2, etc. The value of the actual color will change based on the current theme of the app.


**Handling navigation**


Every CTA will have a target screen’s name and required params in` args` key and we use[react-navigation](https://reactnavigation.org/) for app navigation.


Before calling` navigate` we check if the screen exists as there is a ** ***chance a new screen is added in the new version of the app but the user is on the old version*** (Though we use codepush there are always a situation where we add a new screen which in turn has a native dependency).


> If a screen does not exist we consider it as an opportunity to drive app update so we show a message like “ **update the app to use the latest features of the app”**


**Handling versioning**


There are situations where we wanted some JSON blocks to be rendered only on specific versions of the app. We handle it by adding` app_version` key to each component in the JSON and render it conditionally based on the current app version.


```text
app_version:{   max_version:23,   min_version:19  }
```


**Managing the JSON**


We internally use self-hosted[retool](https://retool.com/) to host and manage the JSON but as of now, that’s not the best way so we have to move towards a better way to manage to edit the JSON so that nobody mistakenly adds or misspells a key.


> This architecture needs few improvements like - fetch only when there is an update made to the JSON and fetch the diff rather than the whole JSON.


**Taking it to the next level**


One drawback to the above approach is the layout of the cards, the colors and fonts have to be predefined and it is sufficient in most cases as you can always release an app update during a major design system change. To extend this you can always maintain a view type and position prop inside the JSON itself and render a dynamic component by reading all the props like converting JSON to react components.
