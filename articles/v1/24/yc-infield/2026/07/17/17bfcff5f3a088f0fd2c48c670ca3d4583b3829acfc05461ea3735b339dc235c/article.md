---
schema_version: "1.0.0"
document_id: "17bfcff5f3a088f0fd2c48c670ca3d4583b3829acfc05461ea3735b339dc235c"
company_key: "yc-infield"
company: "Infield"
source_id: "yc-infield-news-import-3dde9d4b3c10"
canonical_url: "https://www.infield.ai/post/once-a-maintainer-marc-andre-lafortune"
published_at: null
first_seen_at: "2026-07-25T09:33:54.576450+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:6498476bb9a1f7b7a516cacdf252ae0748d36094401c6cd765f28861fb83303d"
---

# Once a Maintainer: Marc-André Lafortune

Welcome to Once a Maintainer, where we interview open source maintainers and tell their story.


This week we’re talking to[Marc-André Lafortune](https://github.com/marcandre) , a longtime contributor to the Ruby and Elixir communities, member of the Ruby and rubocop core teams including the core[rubocop-ast](https://github.com/rubocop/rubocop-ast) engine, and creator of the[backports](https://github.com/marcandre/backports) gem. We spoke with Marc-André from Montreal.


**How did you get into programming?**


I was really, really young, beginning of the 80’s. My father always really liked new gadgets. I think we were one of the first people in Montreal to get rollerblades at the time, and the computer was the same kind of thing. He was like, well, we could go on a ski vacation like we did last year, or we could get a computer, and my sister and I were both like, well, we don't know what a computer is. So it's difficult to answer the question. But he had the answer. He wanted a computer.


He ended up not actually using it himself much, but at the time you just had a prompt to start with, right? So it was very easy to get programming and just, I don't know, have fun hacking the machine. I learned programming by myself in BASIC on an[Apple II Plus](https://en.wikipedia.org/wiki/Apple_II_Plus) that had a huge memory card upgrade and a whopping 64 KB of memory. I've always loved programming. I've basically been programming all my life.


**Professionally, did you feel like this was always going to be the area that you were going to work in?**


I tried resisting. I actually studied mathematics and physics, I did a time in engineering, and I was trying to do something else just because I kind of already knew how to program in a way. But after my studies I was already working part time for a startup, and then I started full time. I ended up working there for like 16 years.


**What was your first exposure to open source?**


At the startup we weren't using open source, and it was a huge issue because we didn't get the support that we needed. We were stuck in an all-in-one environment where it was very difficult to get out of any bug we would encounter. We could file a report, but there wasn't any public database of bugs. There wasn't even an issue number, so a new version would come out and we had to double check if the bug had been fixed or not. So we kept an internal database of bugs we encountered, and every new release we had to figure out if they had been fixed or not. Usually the answer was no. The bug was still there so we still had to circumvent it. This was pre-Internet.


The web came about and we were like okay, our clients need something but there's no solution. So we basically had to use a super low level networking extension and write our own web server and our own parsing of HTML to figure out the tags that we wanted to replace dynamically and stuff that. Basically we had to write our own super small version of of Rails and and erb. It was extremely painful, and at that point I was like, anything I choose as a technology afterwards, we have to have control. So it wasn't necessarily the cost, or even the contributions we wanted. It was an issue of control. I cannot get to the bottom of this. I cannot fix that bug because the culprit is out of sight.


**What was your first exposure to Ruby?**


Like most people, I got exposed to it by DHH and his “Let's write a blog in 30 minutes” thing. And I was like, my God, Rails is great, but it was the flexibility of Ruby that was really behind it. And then I really fell in love with Ruby.


**Are you still on the Ruby core team?**


I am not very active, but at the time I was, I was really interested in Ruby and I got kind of lazy and didn't want to upgrade my local Ruby. This was before[rbenv](https://github.com/rbenv/rbenv) , and I was like, well, I don't really need to, but I want to use the new features and I can actually write new features in Ruby so I don't have to upgrade. And that's when I started writing my backports gem.


Writing that library was great because I had to go in the C code and see how are they doing it exactly. And then realized that sometimes there were bugs or corner cases they hadn't thought of. So I ended up fixing the language itself for those corner cases, writing more tests so that the test would actually work with my implementation in Ruby and the built-in C code.


**How do you think you got comfortable jumping into actually working on a programming language? That's very technical, kind of intimidating work for a lot of people.**


Well the thing is I don't have an \[academic\] background in computer programming. So the idea of writing a parser and and a compiler and stuff are kind of things I had to learn on the go. So I wouldn't say I'm comfortable with them, but I find them super interesting. When there's a good reason to do so, it's like, why not?


**What did you use to ramp up?**


Well, at the beginning it was books and just, experiment, experiment, experiment but I started really, really early. So I have over 40 years of programming experience. You don't need to go really fast. I really like to go and dig and see exactly how things are made and how they're done. And then you basically ask stupid questions that become not so stupid as you go.


**Do you think that parallels the experience with**[rubocop](https://github.com/rubocop/rubocop) **as well? Because it seems like rubocop-ast is the part of rubocop that’s really how it works. It's like the underlying engine you express your rules in.**


Yeah, so the unasked question there is how I got involved in Rubocop. That is really the fault of a friend of mine,[Maxime Lapointe](https://github.com/MaxLap) , who's a really good programmer. He works in Ruby. And he had this idea of writing a code coverage tool for Ruby.


Code coverage is basically you run a bunch of tests on your code base and you ask the question, “Hey, of my code base, which lines of code were actually run?” or more importantly, which ones were not run. Because if there’s lines of code that were not run, it means that I have no idea if these lines of code are actually good. They might have a really big bug, and because they're not run, I wouldn't know each time I run my test base. So at the time, there were some coverage utilities, but they were very crude in the sense that they would tell you if a particular line of code had been executed, but not if every bit of that line had been executed. So an example is, if in a line of code you have **condition ? something : something_else** right? But in your tests if it happens that the condition is always true, then the first part of that expression is always going to be evaluated, but whatever is after the colon is just never executed in your tests.


So there's no way to know from your code base, you could have 100% line coverage and it still won't execute. So he was like, well, there's this branch coverage thing that asks, whenever you have an if or some kind of condition, do you actually go into all of those conditions? And I thought it was super interesting. How could you possibly do this? You could instrument your Ruby code to just add something that says hey, I've been here, hey, I've been there. Basically you add code to check boxes of where you've been, and at the end you have an idea of what you've done. So the idea really is to take the Ruby code and just rewrite it.


It's really ugly, the resulting code, it’s just basically writing check marks everywhere. And I was like oh my God, that looks like a super interesting project. And he was like, yeah, yeah, I'll start working on it whenever. And I'm like, no, no, no, I'm starting today. I really wanted to see if it was possible to do it.


So Ruby comes with a parser, but for our purposes it wasn't all that great. So a parser, you know, is you take text that is completely unstructured and you basically get a structured understanding of what the code is. An example is if you write 1 + 2 * 3. You know these are a few characters and a few spaces, and we understand what it means, we can calculate in our heads. But the idea of a parser is OK, well it's a multiplication of two and three, and that's the second term of an addition, right? So you get this tree saying I want to add 1 and something. That something is a multiplication between 2 and 3. So the built-in parser is kind of cute. I mean it does the job, but it's very very low level. It's not semantic I would say. If you add parentheses around the 2 and the 3, it will say, “There are parentheses around 2 * 3”. But to me, it makes no difference. I don’t want to know that you wrote 1 + 2 * 3 instead of 1 + (2 * 3).


So there was this really awesome gem called[parser](https://github.com/whitequark/parser) written by someone not on the core team that gives you a super clean understanding of the Ruby code. Not only does it not care if the parentheses are there or not, but there's a really well structured and precise mapping of where the information comes from and it is completely semantic. So if you've got parentheses or not, it's not gonna make any difference in the structure of your abstract syntax tree, but you can actually ask where are the locations. That is taken care of, but the understanding of the code, what's going on in the code is completely independent of if you wrote those parentheses or not.


And that's really where it all started. We started using that parser gem and we wrote this tool called[deep-cover](https://github.com/deep-cover/deep-cover) , which was at the time the first branch covering tool. And it was a little bit crazy. Like if you raised an exception it would actually track that. It was pretty cool and about the same time, the core team was working on a similar tool in C so they both came out around the same time.


So that was super interesting to write because we really had to understand the code and the different possibilities of Ruby execution. And then we wanted to color it to tell you this has been executed, this we don't really care about because it's just syntax, this has never been executed, and things like that. And when we did that, the HTML we were generating was not correct, and I was like, what's going on? I realized that the way we were using this rewriting tool wasn't how it was meant to be implemented. So I went to the parser team and said hey, I've got this idea for how to write the rewriting tool that would happen to solve my problem with HTML generation and they were like wow, that sounds really cool. So we ended up doing that.


Then I looked into Rubocop, which is the main gem that uses parser, to see what are the consequences for Rubocop? Would this work for that as well? Because Rubocop also does auto-correction, so that’s kind of complicated. When I started digging into the code to do corrections I realized the API wasn’t great and it was also in the way of my new API. So I said hey, how about this new API? And they were like sure, but I didn't realize that they would actually want to change every single existing cop that was doing auto correction to use my new API. So I didn’t end up doing it, but there's so many people contributing to Rubocop, it was actually done really quickly. Some of the contributors are working like crazy. I don't know where they find the time.


**Rubocop is fascinating because you want to express some things over the AST because of what you're describing, but in other cases you actually do want to do syntax corrections, like you want to find all of the arrays that were declared.**


You're right, there's this interplay between what you're doing in the code and how you're writing it. Sometimes you're just playing with the abstract syntax tree, but very often you actually want to know how it's actually written. And the parser gem is just amazing again, because when you actually have that information, you can ask where is it? How has it been written?


**I think that users love Rubocop because I think the project has done a great job of putting a lot of this work on the maintainers to make the cops work, even if it means it's extra code to write. It has a feel to it that it cares that it doesn't break your code.**


I think it's a love and hate affair, because for most projects people don't have control over which rules are applied. Lots of developers are like, oh I hate this rule. So it really depends which rules you enforce or not, but it is pretty powerful.


**I guess my last question would be, what are you using right now that you think is cool?**


Well, about two and a half years ago I was like, I've been doing Ruby for 13 years, and I still love the syntax, I love so many things about Ruby. But there are things that really bother me. I wanna check out Elixir. So for the last two years and something I've been doing exclusively Elixir professionally, but I'm still maintaining my Ruby projects, including backports.


Every year when a new version of Ruby comes out, I'm like OK, what are the new versions? I’ve not played with the cops myself for years, but since I wrote most of the base building blocks like the AST treatment, whenever there’s a new type of node like the language has evolved, I approve the PRs. But it’s very stable. I don't know what percentage of contributions are because someone actually needs it, or it’s not because they need it, it's just because they're attached to a project. They love doing it.


**Like how you got into it with your friend, where you just find the concept so intellectually interesting that you almost can't help yourself.**


Yeah. In my case, it wasn't a real need, it was more like curiosity and just having the luxury of saying, “I'll work on that.”


‍


‍
