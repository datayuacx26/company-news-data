---
schema_version: "1.0.0"
document_id: "8dcf9e1061b2c4727f6c35d2fce8fa6d23f6625f240e9772b47196e054f5089c"
company_key: "yc-tumult"
company: "Tumult"
source_id: "yc-tumult-rss-8c75f78361aa"
canonical_url: "https://blog.tumult.com/2022/12/09/leveraging-chatgpt-for-animations-workflow-improvements-in-tumult-hype/"
published_at: "2022-12-09T15:27:45+00:00"
first_seen_at: "2026-07-20T23:24:19.039958+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:d8c8b47d7fc7ddd3fcfe5de4e9733c07d4eada6290cad603f575d4e87f1c8157"
---

# Leveraging ChatGPT for Conversions and Pair Programming

Over the past week, we have been experimenting with[ChatGPT](https://chat.openai.com/chat) , a highly advanced chatbot developed by OpenAI. Some may have observed ChatGPT’s ability to generate animations and visualizations in response to prompts related to animation libraries, or its capacity to produce lengthy code snippets based on simple prompts. Given these capabilities, we’ve been considering the potential benefits ChatGPT could offer to Tumult Hype users.


## Teaching ChatGPT about Hype’s JavaScript API


Max Ziebell, a Tumult Hype power user and creator of several extensions, has posted a detailed method of training the ChatGPT mind on Hype’s JavaScript API. This method allows the AI to ask difficult questions and compose code for you. Max has also provided easy-to-copy snippets for teaching the API and encouraging the use of modern JavaScript practices and correct function formatting. In addition, he has requested that the AI include comments in its code responses.[Check it out!](https://forums.tumult.com/t/using-chatgtp-to-answer-hype-coding-questions/22574)


## Easing Function Conversions


There are so many ways ChatGPT may improve your life as an interaction designer, animator and programmer. One thing ChatGPT excels at is conversions: as long as it understand what format you’d like your code in and can give some guidance, the system can write code with pretty high accuracy. **Let’s say I want to add a new easing function to Hype** … something like[EaseInOutBack](https://easings.net/#easeInOutBack) , but you don’t want to use the easing animation editor, or figure out the complex mathematics. Hype’s easing function uses this format:


```text
function (t, start, dur) {  return t / dur;  }
```


Where` t` is the current time,` start` is the beginning of the animation, and` dur` is the total time. If we check out[https://easings.net](https://easings.net/) , a great resource for finding a wide range of functions for easings, we see that the easeInOutBack function typically uses this format:


```text
function easeInOutBack(x: number): number {
const c1 = 1.70158;
const c2 = c1 * 1.525;


return x < 0.5
? (Math.pow(2 * x, 2) * ((c2 + 1) * 2 * x - c2)) / 2
: (Math.pow(2 * x - 2, 2) * ((c2 + 1) * (x * 2 - 2) + c2) + 2) / 2;
}
```


If you want to convert this to Hype format using ChatGPT, you would simply ask this question:


If you paste this selected code into Hype’s easing function editor, we get the same **easeInOutBack** animation:


And depending on whether you apply this to the ‘top’ or ‘left’, you’ll get something like this:


For a deeper dive into easing functions in Tumult Hype, read this post:


> [Advanced Timing Functions and Easing for Web Animations with Tumult Hype](https://blog.tumult.com/2018/04/02/advanced-timing-functions-and-easing-for-web-animations-with-tumult-hype/)


## Converting from jQuery to Vanilla JavaScript


This example of converting an easing function to something usable in Hype’s timing function editor can extend to other useful conversions. Let’s say you wanted to use a jQuery function to select all elements with the class ‘myClass’ but want to avoid embedding the full jQuery library. Simply ask for a ‘vanilla JavaScript’ version, and ChatGPT will deliver a snippet you can paste in Hype’s JavaScript editor to run on the scene:


## A learning partner


Beyond converting code for you, ChatGPT can help describe parts that you may not understand by adding code comments. If you encounter a useful code snippet on the[forums](https://forums.tumult.com/) or in our[documentation](https://tumult.com/hype/documentation/) and need help understanding and adapting it for your own purpose, ChatGPT is a great learning partner. Once you load the Hype API[using Max’s instructions](https://forums.tumult.com/t/using-chatgtp-to-answer-hype-coding-questions/22574) , you can ask it questions like the following:


The above explanation is absolutely perfect, and shows a correct knowledge of the Hype API. We hope ChatGPT helps take your animations to new and more interesting places as you leverage this tool.


### Related posts:


[Hangout with Hype: Face to Face tutorials on Google+](https://blog.tumult.com/2013/06/19/hangout-with-hype-face-to-face-tutorials-on-google/)[Tutorial: Create Parallax effects and single-page scrollable experiences in Tumult Hype](https://blog.tumult.com/2014/08/06/tutorial-easily-create-parallax-effects-and-single-page-scrollable-experiences-in-tumult-hype/)[New Hosting Option: Tiiny Host](https://blog.tumult.com/2022/02/02/new-hosting-option-tiiny-host/)
