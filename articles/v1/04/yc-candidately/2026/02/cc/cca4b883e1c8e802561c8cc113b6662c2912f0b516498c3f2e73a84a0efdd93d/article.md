---
schema_version: "1.0.0"
document_id: "cca4b883e1c8e802561c8cc113b6662c2912f0b516498c3f2e73a84a0efdd93d"
company_key: "yc-candidately"
company: "Candidately"
source_id: "yc-candidately-rss-e1b1f86d062d"
canonical_url: "https://www.candidately.com/webinar/best-practices-for-implementing-a-deployment-platform"
published_at: "2026-02-24T18:52:58+00:00"
first_seen_at: "2026-07-25T01:07:55.188192+00:00"
fetched_at: "2026-07-28T22:19:11.584011+00:00"
content_hash: "sha256:9762164997bb428450c7c2271b0b1fafb1f23b2be2eb4e3d294bc96f7432091d"
---

# Best Practices for Implementing a Deployment Platform

**Brian Delle Donne:** Well, good morning and welcome to the first session of the World Staffing Summit today. Thank you for joining this particular track. We're really excited to have you along with us, you know, this session is entitled best practices for implementing a deployment platform, but in fact, it's a whole lot more it is going to talk about deployment platforms in some detail and besides just the best practices, we'll be unpacking just what they are, how they work.


**Brian Delle Donne:** Where they're applicable. Some of the challenges and some of the successes that they've all had. So of course we'll talk about best practices, but more importantly we'll educate you all on the importance of this really exciting emerging technology, which talent tech labs actually consider to be one of the most innovative.


**Brian Delle Donne:** Pieces of new technology in the market today, that's really additive for staffing companies, especially those staffing companies that are working with hourly workers in their placements. So deployment platforms today are actually a brand new take on old time and attendance systems and many hourly staffing companies are using systems like Kronos.


**Brian Delle Donne:** To, you know, track time log in, log out and and have the candidates get their time approved and things like this, but this generation of deployment tools. It's actually a digital interface it's being created between the candidate and the client. So both are getting the opportunity to experience an app-like capability to really digitize the way that hourly staffing is being deployed.


**Brian Delle Donne:** So we have a great panel today to take you through all of this. And I'm really excited to be joined today by a couple of people who are staffing executives who have experienced deploying these systems. And then a couple of the leading technology solution providers who have been bringing these solutions to market.


**Brian Delle Donne:** So I'm in a moment I'll have each of them introduce themselves, but great panel Keith Corrigan, who is the Chief Operating Officer of Integrity Staffing has deep experience in a nice, a little history in deployment platforms. Aram Hampoian who is working to get back in. He's had a little bit of a technical challenge getting onto the platform this morning, but we're expecting that he'll join us and he's the CEO for CoreMedical Group


**Brian Delle Donne:** and so he is New Hampshire based and has healthcare practice that has been leveraging deployment platforms. And then we have ErickaHyson, who is the President of WorkN. One of the biggest new comers in the space and Rohan Jacob, who is CEO and founder of TimeSaved. So I'll have them introduce themselves.


**Brian Delle Donne:** And take a couple of minutes to talk about not only the roller in today, but how they got there, of some of their career experiences that informed their views today, either as how they're using the tool or how they decided to build these tools. So why don't we start with you, Keith, if you could say hello and tell the audience a little bit about yourself.


**Keith Corrigan:** Absolutely. Thanks. Brian. As Brian mentioned, I am the COO of Integrity Staffing Solutions. We are a high volume staffer mainly in the light industrial space. I'm actually in my fourth year with integrity and I oversee all of the high volume operations along with technology procurement in our center of excellence team.


**Keith Corrigan:** That's charged with deploying best practices throughout our organization. As Brian alluded to, we've been down, going down the path of a deployment platform is a new term. Right. But we've been going down the path of this for many years. And we've we've taken and we'll talk, I think a little bit later on the builder by conversations, things like that.


**Keith Corrigan:** But we actually have seen the need for this technology for many years being in the high volume space. And so we actually built our own, so we'll talk about that throughout the presentation why we chose to go down that path and then where we are today. So is that good for now, Brian?


**Brian Delle Donne:** That's a good start.


**Brian Delle Donne:** Thanks very much. Maybe just a word or two about what percentage of your business, if you can describe it that way, is deployed through your deployment platform as a component of your total hours built.


**Keith Corrigan:** Yeah. Right, right now it's running about 17% of our business runs through the platform in the form mainly of high volume staffing.


**Brian Delle Donne:** Excellent. So, we'd love to introduce Aram and we'll give them that chance when he catches up with us. Hopefully he's in the green room backstage making his way to the front stage. But let's jump over to Erica. Erica, why don't you tell us a little about yourself and how you got here.


**Brian Delle Donne:** What you can enlighten us with about your great background.


**Ericka Hyson:** Thank you so much, Brian. And the World Staffing Summit for inviting me to share the stage this morning on this topic. My name is ErickaHyson. I'm the president of WorkN. We have certainly deployed online staffing platforms for the staffing industry since 2015.


**Ericka Hyson:** Prior to joining WorkN. I have a particular passion for improving the candidate talent experience as well as improving profitability. I spent 20 years as an operator and front office recruiting sales leadership for a very fast growing IT staffing firm called ettain group in my role of Chief Operating Officer for the last four years while I was there.


**Ericka Hyson:** I certainly understand the business cases and the need to. Impact productivity, profitability, ROI drive customer value on all fronts for both internal stakeholders, as well as external talent and customers which really has fueled my passion to leverage technology for an industry and helping industry really embrace technology and a new way to service their customers.


**Ericka Hyson:** Whether it's clients or talent and ultimately help their employees and recruiters make more money. So that's a little bit about my personal passion here at WorkN, we focus 100% on the staffing industry and I'm really excited to be able to help an industry embrace technology. That's actually not that new.


**Ericka Hyson:** It's just that the staffing industry is really kind of waking up to realizing that in this market where talent is very hard to come by focusing on the talent experience is paramount. And those that are doing that and doing it well. Are going to be leaders and those that are going to fall behind or going to be laggards.


**Brian Delle Donne:** Thanks, Erica. Rohan. Tell us a little about yourself.


**Rohan Jacob:** Sure. Erica. Great to be on a panel with you again, and thanks for having me on here Brian. Nice to meet you, Keith. A little bit about me. I started off in tech as a builder, and then I ended up working in retail for Canada's largest. So I was actually on the client side.


**Rohan Jacob:** Prior to getting into here. So we worked for a retail operation in Canada subsidiary of best buy, managed their largest operation in Canada, the flagship here. And then essentially back in 2015 started a talent platform ourselves. Very quickly found that there was going to be a convergence.


**Rohan Jacob:** We saw this sort of happening and moved into serving staffing agencies in 2016. When we realized that, you know, the online platform Ericka alluded to the, this tech isn't new you know, it's been. It's been around for almost 20 years. You've seen Amazon Turk Upwork or before Upwork elance, oDesk, all of those sort of working in the virtual world, the white collar space and sort making that work more accessible.


**Rohan Jacob:** And now we're starting to see that shift into on-demand. Talent for onsite labor in platforms like Uber and Wonolo and you mentioned blue crew. What we had seen is that those platforms oftentimes classified workers as independent contractors as 10 90 nines. And we saw that there would be a convergence into staffing, which is heavily regulated.


**Rohan Jacob:** And so essentially we decided to build the technology for staffing so that they could leverage some of the Essentially all of the operational efficiencies that come from having a deployment platform. So we've been doing that for about seven years now.


**Brian Delle Donne:** That's great Rohan. And you're situated in Toronto.


**Rohan Jacob:** We're in toronto yet our teams across Canada and the US.


**Brian Delle Donne:** Cool, cool. Aram, I'm glad to see that you could make it glad you could battle your way in. We're just wrapping up the introduction. So, why don't you talk a little bit about how you built the company and you know what your relationship is with the deployment platforms.


**Aram Hampoian:** Right. I apologize for the technical difficulties here. I love talking about technology and having technical difficulties, but overall the core medical group. We are a national US healthcare staffing organization. We specialize in travel nursing. Locums and perm placement. I've been with the company for 18 years.


**Aram Hampoian:** And most recently we just started a partnership with TimeSaved over the past six months and really excited to share what we've learned over that period of time and how we think it's already transformed our business in a very quick event or events we should say. So really excited to talk about this. Thank you for having me.


**Brian Delle Donne:** That's great. Thank you. So just for our listeners, you know, there's a chat over on the right-hand side of the screen. So if you would have any engaging points you want to raise, we'd love it to be interactive. So if you serve up some questions, we'll either direct them to the speakers in line or address them at the end.


**Brian Delle Donne:** But please don't hesitate to, at, to use that chat feature. And then for your colleagues in case you're feeling like they should listen to this as well. You can let them know this session is being recorded and they can, you know, go to the World Staffing Summit website after this event and listen to any of the sessions that they may not have had the chance to participate in first hand..


**Brian Delle Donne:** So, let's jump into this, you know, as you heard in some of these introductions deployment platforms in one form or fashion are somewhat not new. You know, but this conversation is not about the old school, our granddad's deployment platform. Like the Kronos of this world.


**Brian Delle Donne:** This is the next generation. Which is really the digital interface that as Ericka touched on in her own opening really has a tremendous appeal in creating great candidate experience and making a sticky experience for the companies that deploy these platforms with their candidates.


**Brian Delle Donne:** And by making it really easy to use for the client. It embeds the staffing company really deeply into that organization and makes you sticky there. So it seems like a really winning proposition, but these are a little bit complicated to install. And so during this conversation, some of the questions we'll talk about you know, how do we perfect the implementation and what does it really mean to have you know, the, both the candidate, the client and the staffing company on the platform and can take full use of it.


**Brian Delle Donne:** You know, getting back to the notion that it's not entirely new. I would say that this generation of tool started to make an emergence probably with the big splash that that true blue implemented back in about the 2019 timeframe. And what they reported was that in just one quarter, they put almost a million hours of shift work through.


**Brian Delle Donne:** The platform that they had built with swipe jobs. And so a couple of case points there, one is an astronomical number of hours being put through the platform and a platform that was built by a third party that they were able to white label and make their own some of the key features that that we'd like to unpack today as we get into the discussion.


**Brian Delle Donne:** So why don't we start with you Rohan. Let's talk about from a technology perspective, what is a deployment platform? And of course these are late people on the call, so we don't need the code level, but if you could talk about what the tech actually does, that'd be really helpful.


**Rohan Jacob:** Sure. Yeah. So yeah, when we started back in 2015, the world of work was already shifting.


**Rohan Jacob:** People were using Uber for taxis, Tinder for dating, Amazon for shopping, and you can do all and you can do your banking on your phone. But the world of work very much, still relied heavily on desktops recruiters. If you watch them have to be behind a desk with multiple monitors, you know, you couldn't, if you were to look at them, you kind of see them as.


**Rohan Jacob:** Day traders, right? Like they have these multiple screens, they had all these different windows open on their screens and they look very much like bankers workers have to come into an agency office and fill out reams of paper for an application. And then clients had to send in these requests well in advance in order to make sure that their staffing needs were met today.


**Rohan Jacob:** Mobile tech can take care of these needs using real-time data that's available in pretty much everyone's pockets. Most folks look at this type of tech and think at the surface. I need an app for my candidates. So knowing that, you know, we were going to have a light industrial client on your Keith I figured, you know, Aram could shed a little bit of light to get some, a wider experience, but ultimately it's, you know, it's a different candidate experience.


**Rohan Jacob:** It's pretty much what. You know, all of us are consumers and where we sort of have come to expect a certain level of service, a certain level of transparency, a certain level of communication. How do we enable that for a staffing agency? Whether it's a startup or a, you know, something, someone that's a hundred million or a few hundred million in revenue.


**Rohan Jacob:** You mentioned that Brian, TrueBlue was one of the early adopters of this and they still lit the path for us to to show what works, what doesn't work and, you know, for builders like us it's great to be able to kind of see that in practice so that we can actually look at it makes it easier, right?


**Rohan Jacob:** Because we can, they basically forge that path that we can learn from.


**Brian Delle Donne:** Thank you. That's great. So, Erica, why don't you, why don't you explain if you can. How do these operate and what does it enable the staffing firm to do differently?


**Ericka Hyson:** Yeah, I know we have to, we don't have all day. So, I think what's important to think about here is, you know, as an operator, you know, it keeps a COO, I know he's going to talk about this firsthand so that you're always thinking about internal, right?


**Ericka Hyson:** Like what am I going to? What's the impact on my business, going to be. Which is a great result of what Rohan was just talking about, which is focusing on the town experience. So really, you know, we help our customers look at the benefits. Threefold one is from a talent point of view, which Rohan just touched on. It's you know, how do we meet the needs of today's talent?


**Ericka Hyson:** Are really consumers thank people on Amazon and Netflix and shopping online and with two clicks of a button, right? So mobile is for everyone and the banking industry to this many years ago, airlines, you know, mastered it. And certainly hotels are starting to get into it right where you can pass, bypass your check-in and use your phone as a key card.


**Ericka Hyson:** And these are all things that make it really easy for your customers to do business with you. So the more friction, you know, you can remove. Process with talent the easier it is going to be for you to differentiate yourselves with talent in the marketplace and have, you know, be able to attract talent, retain talent, re-engaged talent.


**Ericka Hyson:** So that's certainly the talent point of view. Brian, specifically, you asked me about the staffing firm and I think, you know, as a COO, you always want to put your CEO COO hat on and think about the business impact. So I can speak to that briefly here. And it really is not one size fits all because each staffing firm that we partner with really has our unique strategy of what they are looking to accomplish, whether it's to really focus on the talent experience, improving the client experience.


**Ericka Hyson:** Both and or improving profitability which is honestly one of the biggest levers that we see the results in that our companies that we partner with can consistently measure is improving that productivity per producer, which I know Keith will be prepared to share it a little while. Certainly automation of manual processes.


**Ericka Hyson:** So where are there places where there are things in spreadsheets, maybe it's scheduling you talked about Kronos from a timekeeping perspective. Ironically. Customers don't actually use online staffing tools because of the and trying to educate their customers to convert to something. You know, unfortunately there's still a lot of manual time clocks out there and wall clocks.


**Ericka Hyson:** So, it's not necessarily a one size fits all component there. So certainly profitability, the ability to attract and retain talent, leveraging mobile technology, doing app based, recruiting, improving your NPS scores, driving referrals, increasing referrals is a huge component. Number two, and then lastly, improving revenue.


**Ericka Hyson:** Right? So how do you leverage the technology that you have? And Brian, I know you're passionate about this to take it in front of the chain, to your customers. They're always asking you, what are you doing differently? And how can you leverage your stack and your approach and your strategy to win that new customers.


**Ericka Hyson:** Even if they're not necessarily adopting online staffing and don't want or value customer self service, you can still deliver amazing differentiation as a staffing firm to your clients, to when new customers retain clients. And you know, if you're in a sea of sameness with 25 other vendors, how do you become number one on that vendor's list? How do you improve speed efficiency and service to your customers? Because that's what they're depending on you for.


**Brian Delle Donne:** That's great. Thank you for that. Keith, why don't you tell us about integrity's deployment platform journey, because as you alluded to in your intro it's a bit longer and started a little bit homegrown.


**Keith Corrigan:** Yeah, absolutely. Thanks, Brian. So. We entered the high volume staffing space about 25 years ago. And at that period of time, there was nothing out there, right. To create great efficiencies in, in staffing, tens of thousands of people. So we were faced with the very real challenge of either we use the technology that's available to us in a less than ideal way, or we go build our own technology.


**Keith Corrigan:** So integrity chose that path. We chose to go down the path of building our own fruit. We started with our own ATS system. We built that out. It was very much designed to serve high volume staffing customers, and it did it incredibly well. Later on what we realized is, you know, more and more candidates were looking for an app based experience, right?


**Keith Corrigan:** So, so cell phones emerged people started downloading apps for everything right now. Now we're at a place where, you know, we would literally say there's an app for everything. And so that digital experience, that online experience was something that candidates wanted. So we started moving down that path of creating our own.


**Keith Corrigan:** We actually created our own technology called flex force. And it proved to be very advantageous to us, but also challenging. So now we've got our own ATS system in our own app based hiring platform. If you will deploy a platform and we're a staffing company and you're, you got to make some choices there.


**Keith Corrigan:** Right? So how much time, effort and investment do I put on these platforms? To keep evolving them, maintaining them and so on versus just evolving the actual staffing business. So fast forward to about two years ago. We made the decision, which was tough for us, right? To, to abandon that side of the business, that software side of the business, if you will, and develop deeper partner relationships with technology partners out there.


**Keith Corrigan:** So a central ATS system, and then, you know, hub and spoke concept. Of marketplace partners that would add onto that ATS system that provide opportunity to improve certain aspects of the candidate experience, the deployment platform being one of them. It's been an interesting journey on the deployment platform side.


**Keith Corrigan:** And I would say to anybody that's going down this path or thinking about it, you got to understand what it is that you, the company want to do with it, because I think we're talking about, you know, kind of the scheduling aspect of it in the customer interface, but there's a lot of ways to, to use the deployment platform, like quote unquote, that term.


**Keith Corrigan:** I can use it as just an app based hiring platform. I can use it as a redeployment tool. I can use it as on demand staffing and on-demand has its own nuances. Rohan alluded to earlier, is this just a 10:99 relationship where I'm simply providing a person that says I can work at this supplier.


**Keith Corrigan:** Most of our customers have very specific requirements to onboard and orientate and train. Things like background texts, drug tests, things of that nature. And you have to be able to understand the needs of your customer and match those up with what candidates offer and what they have, and actually perform that processing.


**Keith Corrigan:** And I think the space that it's evolving into now with Erika is alluding to, these platforms now have to be able to cross that bridge, right? They have to be able to serve what the customer needs or the customer quickly gets frustrated. That it's actually, we're actually providing people that don't meet their needs.


**Keith Corrigan:** And we have taken a lot of time because we so firmly believe in the candidate experience. We would never want to match a candidate to a customer, have that candidate show up and then, really the job isn't a fit for them. Or they're not fully qualified in one way or the other for what the customer needs.


**Keith Corrigan:** And now the customer's frustrated with. We've got to create a great experience. So, it's certainly something that you got to think about, but for us, that evolution was really about just making sure that we were working with the best platform available. And we felt that continuing to try to develop our own and invest the time and resources into that.


**Keith Corrigan:** Wasn't the best use of our time staffing agency now. Now there's enough technology out there that you can leverage marketplace partners and form those good partnerships to take this in the direction that you want to take it. You just got to figure out what is that direction? What do I want to do with this?


**Brian Delle Donne:** Thanks Keith. That's really helpful, great journey and probably a great outcome recognizing what you want to be when you grow up. Right. Focus on what you can do really well, use partners, what you can't. So, so Keith, why don't you, I'm sorry, Aram, why don't you take a different take on this.


**Brian Delle Donne:** You know, you've heard others refer to the ATS and the deployment platform as a separate things. Maybe you could discuss in your use case how your deployment system works. Oh, the pieces of technology in your tech stack, what does it touch? How does it work with them?


**Brian Delle Donne:** Is it integrated as a standalone? Tell us how it all works inside your company.


**Aram Hampoian:** Sure. So similar to Keith and his organization, we have a hub-and-spoke model as well. And we hear a lot from our internal staff that, you know, I have to be in this system or that system. I have to live in seven different systems all at the same time during the day.


**Aram Hampoian:** So when we started partnering with TimeSaved, although they have a very robust database themselves, we actually didn't want that to be turned on for our staff. And we wanted it to all live in our ATS. And so anything that comes between TimeSaved and our ATS will sit in that ATS. They automatically get updates from everything that a candidate will do within the mobile app.


**Aram Hampoian:** We've had good adoption so that people don't feel like we're adding more onto their tech stack and they have to learn something new. They just have to keep doing what they've been doing within our current ATS. Has really taken off in terms of what we're getting on a per desk growth basis over these last few months.


**Aram Hampoian:** Now we also have this opportunity in that we have our own database that we use internally here at core medical group, where we have our own functionality. And we really want to connect that in. So we've gotten the ATS and TimeSaved to work really well that we feel is on a great track. And now we want to.


**Aram Hampoian:** More layers onto it to enhance that candidate experience that Keith was talking about. That's really. You know, I think when we started our journey with TimeSaved, we had like this perfect mobile app in mind and we were just going to launch it all together and that same day, and we realized that probably will take a little longer than we wanted.


**Aram Hampoian:** And so we've started with the ATS. It's going really well. The next step into our current internal database that will feed into the back office from a payroll billing perspective. And then it'll enhance the candidate experience to have our payroll provider connected into it. So, we're really excited to have that total.


**Aram Hampoian:** Solution just like our internal staff. We'll say that they have to live in a lot of different databases. So do our external clinical colleagues who might have to sign into this website or that one to complete something. We want that all to live within a core connection. And we're really excited about that.


**Aram Hampoian:** So that's far, I would say if you're on that journey, you're not sure which way to go try to limit the amount of systems that your internal staff need to be in. And you should see an increase on a per desk performance basis.


**Brian Delle Donne:** That's great insight. And from that comment, I think our listeners can appreciate that it's going to be different in every company's deployment as to how these systems fit, but maybe Rohan, you could take a shot at them suggesting generally, how recruiters or sourcers responsibilities change because of the implementation of a deployment platform.


**Rohan Jacob:** Sure. I think, well just looking at Aram, so part of what we do is we always do interviews with every stakeholder. So, you know, who's actually using the system. Oftentimes when we start engaging with a staffing firm, we're engaging with the COO, the CEO, the CFO, those are the decision makers. They're the visionaries, they're the ones who want to take their organizations forward. They're not always the same people.


**Rohan Jacob:** Most often they're actually not the people that are actually going to be using it. And so we need to make sure that as we build it we're building it to solve the real problems that recruiters, candidates and clients are facing. And so we, you know, every week there's an interview section where we go through and our user experience team is researching by interviewing actual candidates and it's, you know, candidates, clients recruiters.


**Rohan Jacob:** And what we hear from recruiters is it changes the conversation. It changes their workflows. It changes the amount of administrative work that they need to do. And over time they spend less time on that administrative work, like making sure that, you know, if a job is coming from this system, it needs to have this information communicated out versus another system.


**Rohan Jacob:** And they can instead just focus on, you know, the personal growth and development of the person that's right in front of them. And how do we help them build that relationship? Rather than focusing on, because ultimately if you talk to folks in the staffing and recruiting industry, the majority of them, the vast majority of them did not go to school to be a recruiter, to be a staff like to be in staffing.


**Rohan Jacob:** They got into it because they sort of fell into the fact that they really liked talking to people. And how do we as technologists, like, as builders, how do we use that technology to sort of empower that and power that relationship building rather than you know, and remove as much of that administrative work off of that person's plate.


**Brian Delle Donne:** Thank you. That's great. You know, I think that the way that these systems touch on what we'll call the trifecta, the candidate, The client and then your own staffing, company's internal staff makes it a an interesting change management initiative to, to get underway.


**Brian Delle Donne:** So maybe Keith, you can discuss what were some of the biggest challenges on any of those fronts that, that you might briefly just sort of throw out? We can unpack them a little more, but what do you think the biggest challenge is to overcome getting this kind of assistance? Where we are.


**Keith Corrigan:** Yeah. As I talked about earlier, just understanding where you want to use it, how, what is the way that you want to use it?


**Keith Corrigan:** Rohan actually touched on something really that is probably at the crux of all. This is with recruiters, right? How are they going to use the tool? What does it look like for them? What does it do? How does it change their work? And so defining that becomes important, right? Because it will change.


**Keith Corrigan:** And if we use technology to augment, right. What staff do, and I love what you said, Rohan is really what we're trying to do is take away some of the more tedious tasks that a recruiter has to do that are repetitive, you know, matching and searching databases and all those sorts of things that they have to do processing paperwork.


**Keith Corrigan:** Getting big, you know, signatures on documents, all those sorts of things. If we focus on eliminating that work or reducing the network, then they can better focus on the person sitting in front of them, right. Working with them. So some of the hurdles are actually just defining what is it that the technology is going to do?


**Keith Corrigan:** And what is the recruiter going to do? And if we have a good understanding of that, then the two can work nicely together. Right? So if you go into it and you don't know what that's gonna look like, it can present a lot of challenges and we cross some of those hurdles ourselves, right. We had to go back and redefine some of those job responsibilities so that we clearly knew because the technology is going to take over certain.


**Keith Corrigan:** And the recruiter's going to be maybe responsible for things that they weren't responsible for, or that they weren't spending enough time or the time that they would like to spend with the candidates spending, you know, spending time getting to know them. So that's certainly a hurdle is just understanding that definition.


**Keith Corrigan:** And then. Other things that can present challenges is, you know, depending on your team and your team's abilities from a technology standpoint Aram alluded to earlier ideal state is having these systems integrated with all of your other systems. So if you have a digital onboarding platform where all of your documents are coming through, the ideal state is that those documents are being served through this technology.


**Keith Corrigan:** And that requires a high level of integration. Being able to feed jobs back and forth between the ATS, the candidate information. So all of it flows seamlessly so that there isn't one interface for your staff because you don't, nobody would want staff jumping around five different systems and trying to figure that out.


**Keith Corrigan:** And that is a hurdle, right? Cause there's a lot of information around candidates and there's a lot of sensitive information that you don't want, you know, just floating around in the ether. So, right. You know, So taking the time to, to work with marketplace partners, that you're going to be a partner that is going to be a part of your technology stack, if you will, to ensure that they're all working seamlessly together.


**Keith Corrigan:** So understanding what those relationships are and building those integrations is super important. And it all starts with the biggest thing is a plan, right? If you go into this without a plan, You're going to play it. You're going to fail. What's that old saying? Right. Failing to plan is planning to fail.


**Keith Corrigan:** That's so true of this, right? So technology as a part of any, really any business, but staffing industry in particular, since we're, you know, this is our conversation you got to go into this with a good firm plan March to that plan and then make changes as needed. If you don't it's a recipe for failure and frustration for sure.


**Brian Delle Donne:** Thanks Keith. So in those couple of responses about the changes that are going to be affected with these kinds of solutions and both of those answers. So far, sort of looked a little bit inside the staffing company, recruiters and software they have to use and so forth, but you've also got behaviors of the candidate.


**Brian Delle Donne:** And the guy on the shop floor, that's filling a shift. And so, those are relationships that are owned by the staffing company, but maybe Erica, you could take a shot at you know, what is it up to the staffing company to, to get those change management things solved or. Do you, as a provider of these interfaces you know, add anything to that conversation that assists in the change management with the candidates, as well as with the people that are filling, you know, that are ordering up the workers.


**Ericka Hyson:** Yeah. I would love to be funny. I would actually love to hear Keith and Aram's perspective on this because, you know, at the end of the day, I think you know, for us, we are thinking every day, what can we do to improve the talent experience and for talent, there are any trained to want to do things super easily on their mobile device.


**Ericka Hyson:** Right. And so, the lift for talent is pretty simple. You don't need a complex training manual to figure out what buttons to push in the apps to engage with your firm if we're doing our jobs well. Right. So for talents to be able to understand. From the staffing firm, why you're investing in this technology, what it's going to do for them.


**Ericka Hyson:** Again, every staffing firm has their unique strategy. Maybe there is onboarding as part of it or keeping it so, you know, catering your messaging to that talent audience and what you believe you can give them in the app and also having a strategy around, do I want it to be all self-service? How much do I want to make?


**Ericka Hyson:** Self-service you know, we talked about TrueBlue earlier much of what their strategy is full on. Self-service there are many staffing firms who believe that. The secret to success is having a combination of servicing talent when and how they want to be serviced. Right. So, I would say, Brian, it really answered your question.


**Ericka Hyson:** It really does depend on your strategy of how you want talent to engage with you. Or if it's simply you, we want to make sure that you can connect with us anytime. It's this idea of having this like a digital recruiting clone. That's all night, you know, talent might not be exploring jobs until after hours.


**Ericka Hyson:** They may not be looking for activities with you and your firm during your office hours. And the best analogy I can provide for those of you that are joining us to learn about online staffing is really the banking industry used to have a branch on every corner. I lived in Charlotte for 25 years. Like I'd walk into a bank of America.


**Ericka Hyson:** And they were everywhere and over time slowly, but surely there are still branches out there and you can still go into them, but you know, how many things do you want to do in a branch as a consumer, you know, and certainly COVID has accelerated this, but you know, if you want to go into a branch.


**Ericka Hyson:** And you can have your high touch service there perhaps, or maybe even in the branch is fully automated. But I think it's a lot about meeting your talent, where are you with your strategy? Right. And it's very terrifying to a lot of folks that are probably on this call to think about. What has been historically defined white glove service, high touch.


**Ericka Hyson:** I want to see you in person. I want to talk to you. I want to bring you into my branch or office to think about the world in a different way saying, oh my gosh, actually talent may not value having to come into my office. That requires a change of thinking. So I, you I think that's just, you know, for the folks that are listening here, it really does depend on, you know, how you want to service your talent, but hopefully it's the easiest lift you have and is getting talent to use the app.


**Ericka Hyson:** And the bigger lift is really helping your recruiters understand why this is going to help them. Why their data might be more important. So if they're putting fields into their ATS, they never cared about pay rates before all of a sudden they might care about populating the data in the ATS. And you know, I just wanted to comment on what Aram and roll-on and Keith have all said a big key to the success here is letting.


**Ericka Hyson:** Recruiter have minimal change, right? Having them continue to work in the systems that are used to making it easy for them to work out of their ATS has kind of the core system of record and give them that lift so that talent can by serving the talent in a new way, it reduces the inbound calls.


**Ericka Hyson:** Little things. And it's not just recruiters. I think Aram was talking about this too, where, you know, if you put pay and hours in the app and access to looking at your pay statements, you know, how many inbound calls are you reducing to your back office team? We've seen clients have significant lift.


**Ericka Hyson:** They had no idea. It was actually not a goal that they had set out to do. It was kind of a happy accident. It's like, oh wow. You know, we can focus on other things and not just reacting to questions from talent.


**Keith Corrigan:** Okay,


**Brian Delle Donne:** Thank you. That's great. So, so Aram as a staffing executive, the light bulb goes off like, wow, this makes perfect sense.


**Brian Delle Donne:** I'm going to create a better candidate experience going to get them really went to my brand, going to make my recruiters more efficient. Then you've got a customer and maybe that customer is just used to picking up the phone and calling you. Getting a shift filled. What did you do? Or have you done on the client's side to have them see the value of this kind of an offering and what is that a heavy lift or do they get it and just jump on?


**Aram Hampoian:** Yeah I wish I could say that. We're at that point on our deployment, we have not gotten on the client side, given the state of healthcare in our country right now, we thought. If we're going to make an impact to COVID and the problems we want it to first focus on the candidate side and try to get as many qualified clinicians as possible into our client's hands and our clients right now, I don't think are loving, changing how they're operating and doing business as they're putting out fires each and every day.


**Aram Hampoian:** So we didn't want to add more stress to them. We wanted to alleviate their problems by trying to get them the most talented as possible. When we go down this path, I would say we're going to take a similar approach, just like we have here internally. We want all stakeholders to be involved in the process.


**Aram Hampoian:** We're not just going to go talk to the C-suite. Obviously we will talk to the C-suite at the beginning. But we want to make sure everyone. That tree, you know, is on board with that and make sure that it is the right decision for them and not just getting the buy-in from the tops, you know. We need everyone to make that change.


**Aram Hampoian:** And so, for us, when we do go down that path, it's going to probably be a slower burn than we had on the uptake. As Ericka already pointed out, talent. You flip that on there pretty quick to figure it out. I think it'll be a much heavier lift on the client side. We know that can add value to them.


**Aram Hampoian:** We just don't think right now it's the best time to be talking to them about those changes only because of what's happening in the healthcare space.


**Brian Delle Donne:** Right. So your issue is somewhat sector specific. Maybe Keith can add a little bit of light to what it was like on the client side, and maybe your clients knew about this capability because you had your own play in the market.


**Brian Delle Donne:** And, but what was it like to get them to embrace this as the way to do business?


**Keith Corrigan:** I, you know, I would say that's an ongoing challenge, truthfully it's education with a customer. I mean, some customers are used to things like VMS systems, right? So they're used to putting orders in a system.


**Keith Corrigan:** Others are not. And so it really depends on where they're at and what they've been doing and that, so at that point, we have to educate them so that they can get involved in this process. The on demand side of the platform is probably the easiest way to explain that I've got a shift that opens up in the middle of the night.


**Keith Corrigan:** Somebody called out for the morning shift. And I have the ability instead of waiting for my staffing company to come to the office in the morning, calling them up, and placing an order. I have the ability to go into the system and put those in, make those shifts available to my workforce. Right. And have somebody claim those shifts.


**Keith Corrigan:** Those are very specific use cases, but they're ways that a customer can understand how it benefits them. And then they're more likely to use that, right? There's that with them in it, for them, like I can fill those shifts where before highly likely those shifts would have went unfilled or certainly later in the day before us, before the staffing company could react.


**Keith Corrigan:** Now I can interface with the candidates and their qualified pool. So I know that they're going to show up. They're the folks that are vetted to whatever I need them to be vetted for because the technology is helping me with all that. So I really think it's, it depends. And I will say customers are all over the spectrum on this.


**Keith Corrigan:** Some haven't even started down that path. They don't want to do it. They just want to call somebody up. They're still in that boat all the way to the ones that, that really do just want to be able to pop an order in online, set it and forget and let it get filled. And so there's somewhere in that spectrum, but understanding your customers, speaking with them, educating them is a big deal..


**Brian Delle Donne:** That's great. Thank you.


**Ericka Hyson:** Brian. I'd love to add one more thing since we have a curious audience here. But I just want to caution that the staffing firms that are on the phone here are calling, joining us. You know, if you're, if you don't think your clients are exploring this right now, you're missing out.


**Ericka Hyson:** Right? So I think Keith hit the nail on the head. It's worth a conversation because. You know, in RFPs, we're seeing customers coming to us saying help us respond. Right? What we, more and more clients are looking for this type of technology and the opportunity there. And I believe it is part of the future. You know, traditional ways to, to engage talent, you know, 40 hour workers.


**Ericka Hyson:** I want Sally to be there every day and I want the same worker every day for 40 hours. You know, part of the opportunity for staffing firms, just to help clients have access to larger pools of talent. And maybe that client would be interested in. Sally in the morning and Keith in the afternoon.


**Ericka Hyson:** So these platforms can be a great vehicle for them to be able to access talent, cast a broader net and not just sit there on open requirements that are putting a lot of pressure on your firms to fill. So really having that dialogue with your customers and their openness to be serviced in new ways.


**Ericka Hyson:** I would encourage you guys with urgency to be having those discussions, because if you're not someone else.


**Rohan Jacob:** Yeah. I'd like to echo that we've certainly seen a growth in folks looking at direct sourcing platforms and you know, whether it's in healthcare or in the light industrial space, essentially, a lot of companies are looking at how they, you know, everyone's got this war for talent.


**Rohan Jacob:** That's why, you know, you've seen this growth here and that experience is paramount for staffing agencies, but we're seeing a lot of end clients also trying to adopt solutions like this. Right. And so, for staffing agencies that aren't looking at this, you know, hospitals are reaching out and asking for direct sourcing solutions, right?


**Rohan Jacob:** Massive warehouses are looking to do that as well. And part of the reason that they want to do that is they want to reduce their reliance on staffing agencies, because there is a perception that staffing is expensive and that it's behind when it comes to technology adoption. So if there's the opportunity for that end client to leapfrog and do it themselves they certainly would.


**Rohan Jacob:** You know, if that staffing agency can truly continue to be that partner and get ahead and help the company, you know, it's just like Keith said he realized like kudos to you, Keith. I mean, a little bit late for your haircut. You're pretty much like me. But kudos to you for realizing that.


**Rohan Jacob:** Going down this path is going to mean that we have to split our focus and we have to be a tech company and a staffing company. And we realized that we want to be a staffing company first and foremost. I think for a lot of those end clients, given the option of having a staffing partner that can truly provide them the labor that they need when they need it, where they need it, the right time, the right cost.


**Rohan Jacob:** Yeah. That they're not going to go down that direct sourcing or, know, perhaps they'll push that decisioning off that planning off. But you know, if we continue as the staffing industry to try doing things the way that we always have, because it's always worked that way then that's going to, that's going to force that change for the end client.


**Brian Delle Donne:** Rohan you bring up


**Keith Corrigan:** Brian, if I could just add one thing real quick. So we got to realize that the deployment platform of this whole technology group that we're talking about, doesn't magically put candidates into the candidate funnel, right? So for an end user, for a client, it's not now I'm taking on the technology side and I've got to, I've got to adopt and implement technology, but I still have to fill the funnel.


**Keith Corrigan:** I still have to process, I still have to do these. Right. So it's a question of who I am similar to the decision we had to make technology staffing is, am I a manufacturer, might be a distributor or my staffing company and how much time. So it's really, you know, it's that old, still that same old conversation of how much of this do I want in house and how flexible can I be with it?


**Keith Corrigan:** Do I have a great partner in this space? So it's still an opportunity for the staffing companies, right? That's not gonna, that's not going to go. Right. It's really not. You can, you, you can have all the platforms you want to match talent and opportunity. There's still a lot of work to be done in the middle and in filling in the locating or sourcing of those candidates.


**Brian Delle Donne:** So, Keith, that's a great point and it fit very nicely after what Rohan had opened up, which is the competitive threat that the industry is facing right now. Either temp marketplaces, which are a little bit slow at catching up because of some unique features of them. But the ease of use has established new expectations on how easy it should be to dial up a contractor, but more of a threat.


**Brian Delle Donne:** We believe at Talent Tech Labs. Are these direct sourcing platforms, which give the client the notion that they can just reuse the talent that they already know. And so it's always been our belief that a tech only solution there is not going to do very well because there's a service component that's needed to make that work.


**Brian Delle Donne:** And what you're referring to. Is that agencies do have a natural advantage of knowing the candidates, knowing how to source and getting them onto these platforms that then they can start to behave. Almost like a, as Rohan was talking to the direct sourcing strategy or direct sourcing solution, there's actually talks later in today's tracks that addressed both of these areas.


**Brian Delle Donne:** And so we were trying to shed light on the competitive threat. That's going to come from direct sourcing. Because clients are starting to say, Hey, do I need to be paying these markups? It just seems to be a clunky process by adopting this digital interface that deployment platforms afford them actually are in a great position to, to have that client never want to think about a direct sourcing strategy because you have an effect that brought them to that digital solution already.


**Brian Delle Donne:** So, great for suggesting that Rohan we happen to think. This is the most enabling piece of technology in the market lately to allow staffing companies to really play as digital players and get a break from the old bricks and mortar perception that some of these competing technologies like estaffing and template marketplaces are suggesting that they can enjoy a totally digital experience.


**Brian Delle Donne:** So, hats off to you developers who are building these platforms. Cause I think they're very timely. For the market. So let's in the few minutes that we have left. I'd love to just go around and you know, share back with our audience because our title was best practices. So if we could just go around and I'll start with Aram you know, one thing that, that you would like to share with the audiences, as something less than that, you learned that you'd consider to be a best practice and you'd want to pass on to others and we'll just go around and have each of you share from your own perspective, what a best practice might be.


**Aram Hampoian:** Yeah, I guess it'd be a little bit of a repetition, what I said last time, but you know, this is a massive change to your organization and it's going to touch everyone in it. So, you have to have total buy-in not just from your executive team, but your senior leaders, your mid-level leaders and your staff.


**Aram Hampoian:** So having cheerleaders throughout that process. Is going to really make or break it.


**Aram Hampoian:** And you know, I feel like we did a good job. We could have probably done better as we were going through our process. But we have cheerleaders now, you know, all the way up and down the organization who are really excited about what we're doing, and that just enables more followers there.


**Aram Hampoian:** And we've seen an uptake on usage because of those cheerleaders, whether it's in the back office, like Ericka was saying, and less calls coming in or less emails coming. To sales and more people saying, wow, that person's doing really well over there. What are they doing? And when they say I'm taking the technology and leveraging it more, that just gets more people buying it.


**Aram Hampoian:** So I would say just whatever it took to you to make that decision on your ATS and get the buy-in because that's the total transformation. This is the same thing. So, understand. It's not just maybe adding a technology on it's a lot bigger than that. That's what I would say. Is that what? Thank you.


**Brian Delle Donne:** Thanks Aram. Erica, why don't you take a shot at that?


**Ericka Hyson:** I just agree a hundred percent with what Aram said. That's one of the most important things. And I just could, as to Keith and the entire integrity executive team for being chief cheerleaders Todd Bavol is exceptional at this. And I think it's really probably the most important thing.


**Ericka Hyson:** So. Giving another seal of approval to Aram advice there. You know, I think having a great plan, can you talked about this earlier is really important. But also just being willing to just not feel like your plan has to be perfect and that you can have a place to start and you don't have to chunk it all off at one time.


**Ericka Hyson:** Right? You can basically say, I want to start here, let me focus on the talent experience and you know, and then let's build a roadmap together. Where do we want to go from here and then being able to review that plan with some key metrics and, you know, look at those insights, leverage the data to say, well, what could we be doing differently?


**Ericka Hyson:** Or what could we tweak on our plan? What's changed, right? This is a very fast moving industry. Customers' expectations are moving very quickly. And so just be really willing to tweak your plan as you go and be flexible.


**Brian Delle Donne:** Keith lessons learned best practice you want to share.


**Keith Corrigan:** Yeah, absolutely.


**Keith Corrigan:** You know, we've talked about the top-down approach and I think that for evolution in an industry, it takes vision, right? So it takes, it doesn't take the top down, but when it comes time to implement it, it should be cross-functional for lack of a better term. If you include recruiters, if you include people that have to work with these systems in the implementation and you take into their operating reality, like what really is going on in their world and how is this going to impact them?


**Keith Corrigan:** And you account for that throughout the implementation, you get the buy-in of your team. They understand how this is going to fit in and impact them. And they get excited about it. Probably the biggest thing I could say. Don't try to do an implementation where you've just literally you're bought in at the top, but that's all the implementation itself involves multiple people.


**Keith Corrigan:** Let people weigh in on it. Pick a decision and move to Ericka's point. You need to have a plan, but you also gotta be flexible with that plan. Cause you your thoughts will evolve as you learn more and more about the platform. Probably just that biggest thing, I've got a whole list of things, you know, thinking about what would be the best practices, but, you know, for us, in what we learn, we had a very good team and we still have a place to implement this technology.


**Keith Corrigan:** And it takes a lot of different minds thinking and getting involved so that we get maximum buy-in and maximum effectiveness while improving the customer candidate experience, such a big deal.


**Brian Delle Donne:** Thanks, Keith Rohan. I'm gonna give you the last word on this.


**Rohan Jacob:** Thanks, Brian. Yeah. And you know, I like to echo what Ericka and Keith said. It's great to have a plan.


**Rohan Jacob:** Not planning is definitely planning to fail, but the Tyson quote, everyone has a plan until they get punched in the face. And you're certainly going to get punched in the face many times as you go through this. And so, you know, it's it, but it's not worth it. The lift. Right. I think what you're hearing from Keith, what you're hearing from Aram is that yes, it's transformational.


**Rohan Jacob:** Yes. It's a lot of work. But you can take it off in chunks and you can do it, you can do it in little pieces and then it's totally worth it. And so you, and you can tell, you know, we, we got into the best practices of what, five minutes, six minutes left in the conversation. That's just from, you know, We're talking to folks that have been through and have a lot of these war stories to share.


**Rohan Jacob:** So that's why it took us this long. I think when we're looking at like Aram's team, for example, but what I liked about their approach is they had 17 different people in the room representing their 150 internal staff all the way through that decisioning process of picking the partner and then looking at how they want it to roll it out, what this meant.


**Rohan Jacob:** Like they had a PowerPoint presentation that went out to the entire team and then celebrated the folks who had, you know, 50% productivity increase as a result in showing the commission wins to the others because that's, you know, recruiters ultimately are Driven by that. And so, I think sharing the wins, having a plan, all of that is, is imperative.


**Rohan Jacob:** But ultimately it's just to get started and then you'll be able to figure it out. It's not as new anymore.


**Brian Delle Donne:** Thanks Rohan. Listen, panelist. You guys have been great. You've shed so much light on this area. It's an exciting space. Thank you very much for your preparation, for your sharing today for your candor.


**Brian Delle Donne:** I hope our listeners got a tremendous amount of value out of. Please feel free to connect with any of us after this call to explore more deeply. We'd love to share panelists. Thank you each and every one of you for the great work today. Really nice working with you. Have a great day, everyone.


**Aram Hampoian:** Thank you so much. Take care, everyone.


**Rohan Jacob:** Thank you. Appreciate it.


‍
