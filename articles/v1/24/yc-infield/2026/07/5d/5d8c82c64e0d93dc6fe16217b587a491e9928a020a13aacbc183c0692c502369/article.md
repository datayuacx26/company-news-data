---
schema_version: "1.0.0"
document_id: "5d8c82c64e0d93dc6fe16217b587a491e9928a020a13aacbc183c0692c502369"
company_key: "yc-infield"
company: "Infield"
source_id: "yc-infield-news-import-3dde9d4b3c10"
canonical_url: "https://www.infield.ai/post/once-a-maintainer-bryan-housel"
published_at: null
first_seen_at: "2026-07-25T09:33:54.576450+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:8dcc8f502fce843644cfb651e62dc160dc3921a93be9f6cb0911bfb69f828dca"
---

# Once a Maintainer: Bryan Housel

Welcome to Once a Maintainer, where we interview open source maintainers and tell their story.


This week we’re talking to[Bryan Housel](https://github.com/bhousel) , maintainer of several widely used packages in the[Open Street Map](https://www.openstreetmap.org/#map=5/38.01/-95.84) ecosystem, including[Rapid Editor](https://github.com/rapideditor) and[OSM Lab](https://github.com/osmlab) . Bryan is an avid runner, which led to his interest in mapping the world around him. He spoke to us from New Jersey.


**How did you get into software development?**


I guess I’ve always been into problem solving. As a kid I played computer games like a lot of kids, and it seemed like going to study something that I already liked would be a good idea. I went to Drexel University in Philadelphia for engineering. And one thing that Drexel does that’s really cool is they make everybody go on these internships. So I got to do several internships while I was still completing my undergrad degree. And you work at real companies writing code, and that’s how I got started, working for some of those companies that I interned with.


**Was the first line of code you wrote at college, or had you dabbled before that?**


We had a Commodore 64 when I was a kid. And actually my parents had bought one that was broken. It was putting random characters up on the screen. And so when I was a little kid, probably 3rd or 4th grade or something, I figured out how to get those random characters to show up on the screen as a prank. My parents would get mad and think they had to return it to the store again.


So I was growing up in the 80’s with this Commodore. You could get magazines and books that had listings of code and you could go and type them into the computer and see it actually work. And I did that a few times. There was this brief bulletin board scene where if you had a modem, you could call into other people's computers, leave messages and play games with people. It was actually a lot more fun than the Internet we have now in a lot of ways, because nobody was making any money off of it. It was all just like bored kids chatting and playing games.


If you were running a board, you could actually modify the source code and have your bulletin board do different things. So that was really the first programming I did, hacking up my bulletin board. And once I hit like 10th grade, 11th grade, I realized hey, I have an aptitude for this thing and I really like doing it. I could go to school for it and, you know, turn it into a career.


**What was your first exposure to open source professionally?**


I would say in the 90’s, there was this kind of shared source thing. And Linux was sort of taking off. That was the beginnings of open source as a movement or a community. And I remember it wasn't so much that I was doing that kind of work directly, but the companies I worked for definitely benefited from it. Some of it involved graphics, some of it involved databases, some of it was just a server and e-mail and Linux and whatever. So I wasn’t directly working on open source, but I think if you were working professionally at that time, you were kind of aware of it.


My first company was a company involving computer graphics. There was a technology back then called[VRML](https://en.wikipedia.org/wiki/VRML) . It was sort of like this 3D graphics thing that never really took off. But I worked on that for a bit. I also worked on database software for lawyers and law firms and then became a consultant for it. That was closed source and like all things in tech you realize that eventually things will shut down, things will kind of go away. I realized that I'm not going to be able to be a consultant on this software forever. So I kind of turned to some of my other interests.


I sort of fell in with mapping because I'm a runner. And I would go do runs, you know out in the woods or on trails or things. I think I was using Strava or the Garmin tools and realized that some of the maps that I was using were out of date. And that threw me into[Open Street Map](https://github.com/osmus) . At first I just wanted to see my running routes show up on a map that was correct. But it also sort of dovetailed with my interest in programming, and so I got into maintaining the editor for Open Street Map around 2013. It’s a piece of software called[iD](https://wiki.openstreetmap.org/wiki/ID) . In 2012, iD launched and it was just this really cool piece of software. I think I read about it on Hacker News or someplace and I just had to dig in and see how it worked because I was just fascinated, and I ended up submitting some contributions that grew over time, and then I eventually took on a maintainer role.


**The open street map ecosystem is pretty big. Can you talk a little bit about the ecosystem from a maintainer's perspective? Are you pretty siloed in the piece of it that you work on?**


Yeah, it's a big community and it's very enthusiastic. I agree it is kind of siloed. I don't think that people working on the different pieces of software are necessarily having monthly meetings or anything. We do have some communication channels that the community uses. And it's a particularly challenging kind of software to work on, I think. Maybe that's what drew me to it.


Almost every app that you use probably has a map in it. And they have this saying that “spatial is special”. If your e-mail software crashes, you're just like, oh, well, I'll just restart it. But if you're navigating somewhere and your map tries to take you down a street that's not there, or you're out in the world and you just feel like, it's lying to me - it hits us at a different level. So I think people have a higher expectation of their location software than they do of other software and get more passionate about it in some ways. You know, the first thing that people do when they get on Open Street Map is they zoom in on their house, right? They want to see like, oh, there's my house and there's my street. And then almost immediately they're like, this is wrong. And they want to fix it.


Open Street Map is really supposed to be a map of people's local knowledge. Obviously nobody knows more about your house then then you do, so this idea that Google's going to make a map and they're going to drive all around with their cars and map the world and spend several billion dollars and it could be a pretty good map, but there will still be mistakes. What's cool about Open Street Map is if you're driving down your street and you know this business is closed, you can do something about it. You can walk around your town, you can survey, and Open Street Map encourages people to do this. Make the map your own.


**How do you think about mapping data and how it’s used? I noticed that you have a canonical data structure so you want everyone to input McDonald’s in the same way, for example. Why is that important?**


So the other thing about mapping is that there’s a big mapping business and companies that are making money all have an interest in the map being correct. So Meta uses mapping for things like the marketplace. Even though mapping is not their main business, it’s important. Or Uber’s business. Obviously mapping is very important to Uber, but they are not selling the map. So there's an incentive around having people collaborate on a shared map that is the best map that it can be, which requires the data to be correct. And maps are never done - the streets are closing, businesses are changing.


Even as a maintainer you can’t know all the ways that your map is going to get used or who it's going to be used by. Like I started out because I wanted my trails in the woods to look right. We have mapping conferences where we'll go and see what other people are using the map for. And some of the stuff is just amazing.


There's an organization in Tanzania that is mapping remote places where people are victims of violence or this terrible thing called female genital mutilation. They've actually gathered data about how as the map has improved, violence has been going down because they can get people out of situations. And that's the thing where I had no idea that mapping would be used for important things like that. The[Humanitarian Open Street Map Team](https://www.hotosm.org/) , or HOT, they’re doing mapping in places that are affected by natural disasters, to go and help people. There’s so much going on that as a maintainer I like to think about - it’s not just helping Uber.


**How do you think about from a technical perspective what’s next?**


That’s a great question. So the map is always going to be wrong in certain ways. It is always going to lack detail or be incomplete. Those are things we are trying to improve all the time. If you go back even 10 or 15 years, the best aerial imagery was all pixelated, blocky and blurry. And every year it’s getting better. It kind of surprises people when they look at their house how clear it is. And so as the imagery improves, we can use machine learning on that to detect things in the imagery. Parking spaces, building footprints, even the roads. We’re developing new ways to detect this information. And there’s a whole industry around taking images over time and telling how the world is changing. So that’s exciting.


Maybe 15 years ago, people weren't really thinking about mapping the sidewalks and the pedestrian accessibility features. You know, it's called Open Street Map, but it's more than just the streets. There are at least a couple different teams right now working on this problem. One of them is up at the[Taskar Center](https://tcat.cs.washington.edu/) at the University of Washington where they actually have an app for wheelchair accessibility. They can make detailed maps around Seattle of where it's safe to take a wheelchair. Like some of the curb ramps are good and some are not. So if you're in a wheelchair, it might make more sense to cross the street in some places instead of others. Or at a street crossing, is it marked or not marked and are the signals are set up with a button or something that a blind person can use? So you know we're just collecting more and more data. It's almost like every year the state-of-the-art improves, so we’re expanding what's even possible.


**You said that you originally got into mapping because of your interest in trail running. Are there any particularities about mapping the woods that are different than mapping the streets?**


A lot of people have a hobby or an exercise that they like to do and they want the map to support that activity. I don’t know if you remember Pokémon Go, which got really popular a little while ago. So it came out that they were using Open Street Map as the base map for that game. And that presents some interesting challenges because people were mapping fake rivers or fake beaches or whatever to catch river Pokémon outside of their house. We didn’t realize at the time that anyone was going to do that. So we had to develop essentially counter-vandalism, to have people review the map really well. And this is a totally unsolved problem in places where no one’s really looking at the map very closely.


**So it’s kind of like people are taking artistic license with the real world?**


It does feel like that at times. It’s just being used for so many different things. Like it came out a while ago that Tesla was using Open Street Map parking lots to make its Smart Summon work. That’s the feature where you can have your parked car come drive up to the front of the building to get you. After that got announced we got this influx of data from people mapping parking lots in excruciating detail.


And it can be very political. We’ve got people who are mapping on both sides of the Ukraine conflict.


**If people are interested in getting involved in open mapping, how would you suggest they start?**


If you’re waiting to get involved in open source, I would say please look at mapping and into Open Street Map. There are so many unsolved problems that are really interesting. And as maintainers we really need people who are willing to just show up and do the small chores every day. It’s a certain kind of person who wants to do that sort of thing. But if you like gardening, maybe you enjoy weeding. And it’s the same with open source. The weeding is the work. The software I’m working on now is called[Rapid](https://rapideditor.org/) , and it’s the map editor for Open Street Map. We could definitely use help there.


And anyone can go out into the world and contribute to the map. Write down what you see. You can take pictures and share those pictures with the world. Or help out with the humanitarian work. There’s always work to be done.


‍
