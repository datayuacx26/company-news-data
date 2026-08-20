---
schema_version: "1.0.0"
document_id: "c0307e2d68db51d44c502565a9efb469162e839a6103cecb9d59207339dc2e5a"
company_key: "yc-gitstart"
company: "GitStart"
source_id: "yc-gitstart-news-import-dd59d8b4a88d"
canonical_url: "https://gitstart.com/blog/mpharma-cto-interview"
published_at: null
first_seen_at: "2026-07-21T21:42:29.769626+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:1d08fae08045c9978b94f9dca2862be79136b7d3ec2e8800f8ef332d8d480515"
---

# A conversation with Selasie Anani, CTO of mPharma

mPharma is a healthcare technology company focused on making high-quality medicines accessible and affordable in Africa by addressing supply chain inefficiencies through innovative financing and predictive inventory management solutions. With a presence in 150+ hospitals and 800+ pharmacies, as well as partnerships with drug manufacturers like Pfizer and Bayer, mPharma and has helped over 2 million patients across 9 countries in Africa.


James, Head of Remote Engineering at GitStart, recently sat down with Selasie Anani to discuss his experiences and perspective as the CTO of mPharma. Discussion topics include engineering culture, career building advice, AI in health tech, and the how working at a mission-driven company has shaped his leadership philosophy.


## Building a high-performing engineering team


How do you systematize a culture of direct and honest feedback? Is it just something you practice on a day to day? Or are there certain ceremonies you have with managers or developers themselves, to make sure there’s a structured way to give that feedback?


> For the people I manage, I speak to them about the importance of giving people direct feedback on a continuous basis. But then the way I lead the team is to try to be a good example of the type of things I would like to see, or the type of behaviors that I would like to see displayed on the team. So I try to be an embodiment of what I would like to see displayed on the team, where if one of the people that reports to me directly gives me feedback which is not positive, I try to take that and then work on it, and then come back with a response to them. I think that having the whole team see that actually informs critical behavior on the team.


In addition to having engineering metrics and business impact metrics, do you have employee engagement metrics as well?


> Yeah, so on a quarterly basis, we do an engagement survey just to gauge the health of the team. A couple of quarters ago, we ran one that exposed the need to actually have much more transparency around compensation. And then how we do promotions and then base pay increments. So we used that data to go back to human capital. I want to be as transparent as possible by sharing what our career levels are, what the salary bonds are per career level, to the entire engineering organization. And even though we couldn’t get permission to share our full salary levels, we got to a certain level, and I think the team was very appreciative of that.


When you’re hiring software developers, what qualities are you looking for apart from technical skills? What makes a candidate stand out for you as a great potential hire?


> I’ll say communication, a candidate who is able to communicate clearly, and then secondly, someone who understands fundamentals and how things actually work. We have a lot of interviews, what we’ve realized is a lot of times when we ask questions, there are so many libraries out there that people use to get things done, that they do not actually pay close attention to how those libraries work under the hood. There’s been instances where people work with databases, and they do not even know how the database works. I think it’s important that people pay attention to the fundamentals and then ensure that they understand how things work and are able to communicate those things effectively, in any situation.


## Career building advice


How did you learn programming? Did you take the route of trying things and figuring it out when it breaks, or …?


> I think you gain the knowledge by experience, but there’s the other parts where if you understand how the underlying architecture works, it kind of informs how you build a particular feature.


Are there any any resources or communities you recommend to stay informed about new tech trends or just to learn in general?


> I recently chanced upon this page on Twitter. called Byte Byte Go where they share how to craft an architecture, how organizations are building resilient architectures. So I read a lot of those, I watch a lot of YouTube to just keep myself abreast. So a couple of channels I follow, I think InfoQ, I think there’s a channel called GoTo, that I follow, just trying to understand how people, how other companies are building much more resilient architecture.


Are there any books in the tech domain that have been influential on either how you manage, how you architect systems, or just how you develop code?


> Domain Driven Design is one of the books that I’ve read from just an engineering perspective, but I think Elegance Puzzle is one of the nicest books I’ve read when it comes to managing an engineering team. I think for you to have a high functioning engineering organization, culture is important, where you respect the people that you work with, you are honest with them. So I read a book called No Rules Rules, which was very helpful in actually changing my perspective in terms of how I manage the team. It changed my perspective about even compensation, trying to ensure that you are paying above what the market range is. I do not want money to be the focus point of any of the engineers on my team. I want them to be solely focused on the impact that we are making. And as the company grows I’ve gotten permission from the CEO where he’s open to helping us meet the resource needs of the engineers specifically, so that is actually not a challenge for them.


What advice would you give to software developers who are looking to build a career in tech and are early, early on in their careers?


> I would say early on, always find a mentor and be less focused on how much you make in the beginning. And be much more focused on developing your skill or your craft. Finding an organization that will help you develop those skill sets will be very important. Because once you are the best at doing that particular thing, you will actually get offers that will blow your mind. So I think just, focusing on your craft, at the beginning stages will be very helpful. One thing that really helped me was I found myself in a community of people, so I think I would encourage people to join our community. Because we were in an incubator with different companies, different engineers, that was a large group of people that you could learn from. And, when it comes to software engineering specifically, there’s no single person that knows it all. You actually need to be able to reach out to other people within the organization to find out how to solve that particular problem or if there’s a specific technology that helps them do a particular thing really well.


What’s your most memorable tech war story? Like a production failure or a demo gone bad. Are there any ones that you can share?


> Yeah, one happened quite recently, while we were doing a migration. We were doing a migration from Marathon Mesos to Kubernetes. That project was dead in the water because there was not a lot of support for it. We needed to do a migration to Kubernetes. But this was at a time when we had a lot of users as well, so we could not afford downtime. When we did the migration, there was a misconfiguration in terms miscalculated resource usage for the individual pods. So what ended up happening was everything kept timing out, because the services were running out of memory, because we had a lot of users. Then it started stacking up, and it was a complete nightmare, nobody could use the app, everybody was on our support page saying what’s happening. CEO was on my neck., the product team was on my neck asking what’s happening, and then we had to reconfigure. But we are at the point where we believe, even if we were to add more users today, we should be able to easily scale with the infrastructure.


## Q&A from the GitStart community


This question comes from the GitStart community. Sochima on Team Spartan asks, “what trends do you foresee in the tech industry and emerging markets? And how is mPharma positioning itself to ride these trends?”


> I think, as we are all aware, there’s been a rise in using artificial intelligence to improve a number of processes. At mPharma, because we are primarily focused on healthcare, the only way we’ve been trying to use this technology is on helping MCAs (medicine counter assistants), which are the people that you find located in the pharmacies that we work with. We are trying to better educate them on how to prescribe specific medications to patients. We’ve seen incidences where these MCAs do not understand how medicine should be prescribed to a patient, and what we’ve done is to use AI to transcribe the directions of use of a particular medication and then display that information to the MCA at a time that they are dispensing that particular medication to a patient.


How do you make sure that this LLM is giving the right information and not hallucinations?


> We do drug registration or product registrations through our procurement team, which is constituted by pharmacists. The idea is to have them review the data that the AI generates before we present that information to the customer. We don’t just go from displaying that information directly from the AI, it goes through a formal review process simply because it’s medicine. Once it’s approved by a qualified pharmacist within mPharma, that particular information is shown to an MCA, which they can use to inform a patient on how to take a particular medication.


Another question from Sochima on Team Spartan is, “how would you say your journey and the diverse projects you’ve worked on prepared you for your role as CTO of mPharma?”


> I’ve been lucky enough to work in different cultures. I’ve worked for companies in Ghana, I’ve had the opportunity to work with companies in the US, in the UK, and I think just having access to the different types of culture has actually taught me how I think an engineering organization should work. Or how engineers should be treated on their own. I think engineers want you to be as honest with them always as possible. And I think, much more generally, unlike for the non-African companies, I’ve had opportunities where it’s been less micromanaging and more like, “this is what I’m trying to achieve, so try to figure out a way to actually achieve this particular objective rather than telling me specifically how to achieve those specific objectives,” and I think that has shaped how I approach things currently.


A good question to follow up with, from Raymond on Team Asgard, which is “Throughout your journey, what has been the key factor that has kept you motivated and driven, and enabled you to achieve the career and successes you’ve accomplished thus far?”


> Personally, I would say my focus on wanting to make an impact on the African continent. I think because I was obsessed with ensuring that Africa worked, that has actually propelled me to not take other lucrative offers and actually looking to build organizations on the continent that will actually be able to get to a stage to afford the higher wages, because it’s difficult. It’s difficult to be able to pay higher wages when your revenues do not match up to those higher wages. And I think just by focusing on that particular mission for me personally, has actually made me want to stay.


Do you have any parting words or pieces of advice that you would give to listeners as they’re building their careers in tech?


> Yeah, for anyone wanting to be in tech, I understand that a lot of times people are focused on money. Personally, I would say focus on your passion. You can decide to make money until a certain point so that you can follow your passion. I feel that you should focus on the things that drive you, that give you satisfaction. Focus on making an impact in the lives of people, in the lives of like a wider audience, look to join companies because you believe in their mission, and not because they give you the highest paycheck. Be focused on ensuring that the work that you produce is of the highest quality, whether someone is paying attention or not.


What are your thoughts on the social impact to be had in helping everyone else around you grow. How else can sharing how little you know be useful to everyone around you?


> One of the core pillars here at mPharma centers around teamwork. We have a big focus on teamwork, and we expect every single member of the team to be committed to helping the other person. We a lot of ways to communicate asynchronously, so one of the things that I do for every single engineer that joins my team is to ensure that I take them through it and be available to assist them. I think it’s important that as you continue to grow your career, you continue to make it a point to actually help other people that are also starting their career as well.
