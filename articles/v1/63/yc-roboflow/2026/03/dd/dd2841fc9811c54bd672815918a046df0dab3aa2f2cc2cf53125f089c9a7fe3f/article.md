---
schema_version: "1.0.0"
document_id: "dd2841fc9811c54bd672815918a046df0dab3aa2f2cc2cf53125f089c9a7fe3f"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-news-import-01e8e48f5676"
canonical_url: "https://blog.roboflow.com/designing-augmented-reality-computer-vision-apps/"
published_at: "2026-03-09T01:40:00+00:00"
first_seen_at: "2026-07-25T22:08:11.425113+00:00"
fetched_at: "2026-07-28T21:26:25.193690+00:00"
content_hash: "sha256:0bdd7c2aed1650b6c5d0410a82ce4734537c51b0a189b78addb2b27b22a733e7"
---

# Behind the Design of an Augmented Reality Board Game App

[Amanda Morrow](https://blog.roboflow.com/author/amanda/)


Published Mar 9, 2026 • 6 min read


Summary


**Designing an augmented reality app took three iterations: a scorekeeping interface that underused AR, a version built around visualizing letter data on the physical board, and a final pass that pared the interface back so the AR could lead. This case study of BoardBoss, the Boggle companion app that uses computer vision to scan the board, covers ARKit and SwiftUI constraints, testing in messy real-world conditions, and a neon '80s brand chosen to stay legible over a camera view.**


During the summer of 2019, I received a Facebook message from Roboflow co-founder[Brad Dwyer](https://twitter.com/braddwyer?ref=blog.roboflow.com) asking me if I wanted to design a new mobile app he was working on. His previous app,[Magic Sudoku](https://magicsudoku.com/?ref=blog.roboflow.com) , had recently generated some buzz online and even won a[Golden Kitty award from Product Hunt](https://blog.producthunt.com/golden-kitty-awards-winners-7c2628e5f429?ref=blog.roboflow.com) , so he was excited to build another app using Apple's ARKit.


At the beginning of our meeting, Brad placed a Boggle box in front of me and said he wanted to experiment with augmented reality in the board game space. His vision was to blend the the experience of playing board games on your phone with the experience of playing a physical board game with friends.


### Research


There are so many ways to play Boggle.


My first step was to buy Boggle and start playing it regularly. I also began researching different ways you can play Boggle, including all the variations sold by Hasbro, both physical and digital. I even watched old clips of a[short-lived Boggle game show](https://www.youtube.com/watch?v=EbxlB67nnmM&ref=blog.roboflow.com) from the '90s on Youtube.


### First Design Iteration - Where's the Augmented Reality?


I have designed many mobile apps over the years, but this was my first foray into augmented reality. From our initial research and our time spent playing the game, Brad and I identified some pretty obvious pain points.


1. There’s no way quick way to identify words you’ve missed unless you type all the letters into an[online Boggle solver](http://fuzzylogicinc.net/boggle/?ref=blog.roboflow.com) .
2. It takes a significant amount of time to read your words out loud and score them.
3. It can be a pain to challenge words from other players if you aren’t familiar with the Boggle dictionary.


The main requirement was to use[computer vision to scan the letters](https://blog.roboflow.com/creating-boardboss-a-mobile-application-that-improves-boggle/) in the grid and provide a list of all possible words. I was still learning the capabilities of ARKit so, initially, I didn’t spend a lot of time designing the AR experience. I played it safe and designed a simple Boggle app that listed all the possible words, provided a 3-minute timer, and gave users the ability to enter their scores after each round.


My first iteration was too light on augmented reality.


At this point, my designs were fulfilling Brad’s main requirement of including a computer vision component, but the app wasn’t really differentiating itself from other scorekeeping apps. Brad’s feedback was to go all in on the AR experience and come up with something he wasn’t even sure he could build.


### Second Design Iteration - Let's Visualize Some Data


During my initial research, I found a great article[full of tips for improving your Boggle score](http://thebogglers.blogspot.com/2008/06/my-boggle-strategy-fwiw.html?ref=blog.roboflow.com) . It instructed players to look for common groups of letters and memorize all the various words that stem from those groups.


My AR concepts were inspired by data visualization imagery I found online.


With AR, I realized we could use the letters on the grid to do more than just list out all the possible words. We could visualize how often letters are connected by words or draw attention to common groups of letters. I mocked up several concepts showing various ways we could visualize the letters identified in the grid.


Concepts for the augmented reality portion of the app.


Conceptual views from above including a heatmap and letter connections.


### Third Design Iteration - Minimal Interface


We decided to take one of the 3D concepts and experiment with how we could render it in the app. Brad began building this in Reality Composer, while I mocked up screens for the rest of the user flow. We wanted to keep the interface as minimal as possible.


We finally found the right balance of interface and augmented reality.


The first 3D rendering we created of the Boggle letters.


### Refinement


When building interfaces for mobile apps, it’s typically a good idea to use SwiftUI's native interface elements as much as possible. This allows you to build faster and will cause less of a headache when it comes to long-term maintenance and device support.


How does SwiftUI affect my design process? Well, I rely more on a developer to explore what’s possible and we end making decisions as we go. For instance, can we customize the built-in picker or should we build our own?


Animations were a great example of this. The app needed to do more than just show the user where the word was on the grid. We wanted to animate it for them. Brad provided various animation options and I was able to pick which ones would work best. This part of the project was a lot of fun because I never fully knew what Brad was going to come back with.


We experimented with different ways to animate the words.


### Testing, Testing, Testing


To be expected, locating the Boggle board was the most finicky interaction in the app. From table color to lighting and position, the environment can widely vary. We spent a lot of time testing in various conditions with different Boggle games and devices. Based on this, we adjusted the interface to provide guidance to users.


A lot of weird stuff can happen during testing.


### Branding


With a name like BoardBoss, the personality of the app needed to have some attitude. During my discovery phase, I noticed many augmented reality apps were using neon colors and dark backgrounds. These colors seemed to contrast nicely against busy backdrops, which is important when overlaying a camera view.


[Pharos AR](https://ar.pharos.earth/?ref=blog.roboflow.com) , an interactive journey by Childish Gambino, used neon colors throughout.


For the color palette, I drew inspiration from movie posters I found online that were direct callbacks to the '80s. The third season of Stranger Things had just been released so this aesthetic was popping up everywhere.


Some of the retro posters that inspired our color palette.


Animation concept for the scanning screen.


The logo and icons were styled to look like neon lights.


Iterations of the BoardBoss logo.


### Launch


Final screenshots of the app.


BoardBoss was launched in the[App Store](https://apps.apple.com/us/app/boardboss/id1476841910?ref=blog.roboflow.com) on October 8, 2019, and was immediately featured on[Product Hunt.](https://www.producthunt.com/posts/boardboss?ref=blog.roboflow.com)


A few months ago, someone asked me what I’ve worked on recently that I’m really proud of. Of course, I’m proud of everything we've built so far with[Roboflow](https://roboflow.com/?ref=blog.roboflow.com) , but BoardBoss will always hold a special place in my heart.


BoardBoss allowed me to immerse myself in a new technology. I was able to conceptualize interfaces that I wasn’t sure we could build. We got to play around with 3D renderings, neon colors, and animation. And finally, it introduced me to the Roboflow team, whom I’ve had the pleasure of working with this past year.


*Published: October 26, 2020 Updated: March 9, 2026*


### **Cite this Post**


Use the following entry to cite this post in your research:


*[Amanda Morrow](https://blog.roboflow.com/author/amanda/) . (Mar 9, 2026). Behind the Design of an Augmented Reality Board Game App. Roboflow Blog: https://blog.roboflow.com/designing-augmented-reality-computer-vision-apps/*


Stay Connected


Get the Latest in Computer Vision First


### Written by


Amanda Morrow


Amanda leads design and user experience at Roboflow. She is always on the lookout for ways to improve our tools.


[View more posts](https://blog.roboflow.com/author/amanda/)
