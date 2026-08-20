---
schema_version: "1.0.0"
document_id: "2695133edf9e222f99d183804a4765fbf620bbae894de397e039ab3b809b3d45"
company_key: "yc-runway"
company: "Runway"
source_id: "yc-runway-news-import-fe61d44f24da"
canonical_url: "https://www.runway.team/blog/avoiding-release-anxiety-part-ii"
published_at: null
first_seen_at: "2026-07-22T12:25:56.018278+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:02b39f9298129c4808dd5e093aa8806f42e8c9e3319c7fbcc20f7fc63bb9f817"
---

# Avoiding release anxiety, part II

Welcome to Part II of a topic I’ve been wanting to cover for a long time (and one which isn’t covered enough!): how to avoid burnout on a tough project, make building with a team fun again, and ship better products in the process. (You can check out[Part I](https://www.runway.team/blog/avoiding-release-anxiety-part-i-testing-automation) here, where we talked about testing effectively and efficiently, and setting up automation and ‘damage control’ to help reduce stress on you and your team.


Regardless of the tools and processes you're using, you'll find that the best resource you can have to make a project anxiety-free are the people working with you. You can have the best testing practices, the best architectural concepts and the world's most finely-tuned CI, but if you're not aligned with your team, things will probably not go as intended.


To be aligned, you need to **communicate** . Given that our jobs are highly technical I'd imagine that many people reading this are introverted, and as an introvert myself I can attest that communicating properly isn't easy. But if there's one thing I've learned, it’s that knowing how to talk to people is probably the **most useful ability you can have** , regardless of who you are or who you work with. It might be weird to broach this topic – after all, *what does this have to do with mobile development* ?


The answer? **Everything!** Unless you work completely alone, you can’t accomplish anything without talking to people — and in the context of work, I can safely attest that there is a wide range of problems that can emerge purely due to communication problems.


## Humane Code Review


If a person is not motivated or not comfortable with an aspect of a project, the quality of the product will be affected, and I find **bad** **code reviews** to be a major cause of this. We've already talked a little about how to better structure the *automatic* portion of pull requests, but we should also talk a bit about the *human* aspect of them, as this is where you're exposing your ideas about the future of the project. There is no true consensus on how code review should be done. Every project is different, so everyone does it slightly differently. But there's one thing they all have in common: code reviews are done by a human, and that human needs to be careful.


I have a couple of thoughts regarding how **demanding** ** you should be in code reviews. First, if you're not demanding at all **,** you're probably screwed unless your team is really, really good. On the other hand, if you're too demanding **,** you'll likely be seen by others as a toxic individual, which is potentially even worse as it will make people not want to work with you.


For me, the balance that works best is somewhere in the middle. That means that, although you want to be certain that the code works and adheres to the project's style, you need to know how to differentiate between what's important and what's not.


I believe that code quality is directly related to someone's motivation for maintaining quality, and there are few things more draining than a reviewer nitpicking irrelevant details. Here's an example:


```text
let purple = RGB(218,112,214)
```


<chat-blue>johndoe **:** This property name is wrong. The official HTML color code name for this is orchid, not purple.<chat-blue>


As ridiculous as this comment sounds, it's scary how common it is for some reviewers to pointlessly nitpick things like this. It can be useful for big projects to have a style guide for these details, as it helps you navigate other people's code, but it's absolutely pointless to actually **fight** over these issues. Unless the code is *completely* out of style, a small point like this is utterly irrelevant and requesting its change will achieve nothing but frustration for the person writing the code. If you're not working on the sort of project that can get people killed if details like this are wrong, your relationship with your colleagues is ultimately more important. That doesn't mean you should never nitpick, but if you do, make it clear that it's just something that you noticed as opposed to an actual order that the committer has to fulfill.


The same thought process should be applied to the tone of written comments. It's common for newer developers to be afraid of code review, and there's a good reason for it: It's difficult to perfectly convey emotions over text, which can occasionally make it look like you're attacking someone. Given the following code, consider the difference between these 3 comments:


```text
func isUserSubscribed() -> Bool {
if let subscription = currentSubscription() {
return subscription.isActive
} else {
return false
}
}
```


<chat-blue>You should make this a one-liner.<chat-blue>


<chat-red>Make this a one-liner.<chat-red>


<chat-green>We could actually write this in one line: return currentSubscription() ?? false<chat-green>


These comments are similar, but they signal very different tones. In the first example, the usage of the word "you" makes the comment come across as very personal, which will usually put people on the defensive regardless of how good your intentions are. People don't like having fingers pointed at them, so this is usually not a good term to use if you're just making a suggestion.


The second comment has a very indifferent tone, and although it might not sound too bad when spoken, the text version sounds incredibly aggressive.


On the other hand, the third comment that uses *"we could"* sounds more empathetic and friendly. It demonstrates that the commenter is not accusing the committer of making a mistake, but instead showing interest in assisting the person in making the code better fit the project's style expectations.


Everything written in code is an opinion. There is no right or wrong for anything that we do, making it largely important to have an open mind for discussions as opposed to simply imposing your thoughts. That's an especially critical idea for large teams, because the more people involved in a task, the harder it is to reach a consensus. Think about the trade-offs and preferences and use them to find a conclusion that makes sense for the project (and not just to you), but never **assume** things. We're there to provide feedback, and not to be gatekeepers.


Interestingly, the same applies to the reverse situation. If you're the one opening a pull request, it's important to be very clear about what you're doing and to have a good relationship with the ones reviewing it. Try not to throw away suggestions that you disagree with — have empathy for the person performing the review and search for a solution together.


## Style Guides


We've mentioned style issues as one common reason why reviewers nitpick, but I've found that having a consistent style in a project can also be of great help in reducing the release anxiety of a project.


It's impractical to wait for *all* of your team members to review your code when your team is too large, so some division of responsibilities must be made. The issue is that if your team has differing opinions over how a specific piece of functionality should be architected, you can rapidly end up with a project where no one can maintain components built by other people. In this case, a style guide is often a good solution.


Style guides can be seen as an anti-pattern when the goal is speed, and it truly can feel weird to add on overly bureaucratic processes, but a good code standard is beneficial because it can be really hard to scale a project without one. It's easier for human beings to understand things that are familiar to them, which is why most people tend to complain about code written by others. When someone says "the architecture of X sucks" or "the code here is really badly written" it's not that the code is truly bad, it's that this person is so used to what they've been doing that everyone else's work will appear weird in comparison. The code *might* truly be bad, but these opinions would likely still stand even if it's not.


Now, imagine a project with over 30 developers. This is a situation where the company is probably already divided into multiple smaller teams, meaning that it's already impossible for you to review everyone else's code. But even if you could, you're going to be pitching your opinion to dozens of other developers, so you're bound to lose some arguments. If you don't have some sort of pattern in place, this can easily devolve into a situation where each team's contributions feel like a *completely* different product, making it really hard to review and work with other people's code.


In an organization like this, a pull request containing changes to code owned by multiple teams would be too complicated to understand fully, increasing the possibility of bugs and crashes landing in production. And that might end up requiring a hotfix that is even more complicated to understand than the original pull request, further reducing the quality of the codebase and opening the door for future issues. We've mentioned this situation before – it's the infinite loop of release anxiety disasters!


Not only is this situation not made up, but I’ve also actually made it look a lot less bad than it actually was. The lack of a standard can really get in the way of a product's progress, because when the entire code base is written a style that is familiar to you, it's as if it truly was written by you. The presence of a guide in past projects meant that I always knew where to look to find something because it truly felt as if I was the one who actually coded it.


In general, you'd want to make a guide that covers cases that people would often disagree with. How the code should be architectured is perhaps the most common of them – in my past projects, we tried to define as deeply as possible how different pieces of the app should interact with each other, to ensure that people wouldn't have trouble understanding features created by other teams.


Clean coding practices are also often something good to mention; especially how to name properties and functions. This is particularly useful for search – if you're browsing the code of someone who names things similarly to you, you'll have an easier time finding what you're looking for even if you’ve never seen it before, because you can deduce what it'll be called based on your team's standards. That can be quite useful, but as mentioned before, it's also something that you shouldn't take too seriously during code reviews.


In the past we had to maintain consistent styles manually, but this is yet another area where modern linters can be extremely helpful. Most if not all linters can automatically apply styling rules after saving or as a pre-commit hook, making it so you don't even have to review for it.
