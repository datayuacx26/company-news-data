---
schema_version: "1.0.0"
document_id: "d1da9fdf8815a2c150b58f9191b4cb9d31a324552366de2567704f9d134e713d"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/engineering-spotlight-tay-nishimura/"
published_at: "2022-06-06T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:73f4816dd644369bf3c715a91c510f07f3a041617765bd14e87de0fe2c16f6bf"
---

# Engineering Spotlight: Tay Nishimura

Cecilia Saixue Watt


Tay Nishimura


*In this edition of the Datadog Engineering Spotlight, Cecilia from the Community team sat down (virtually) with Tay Nishimura, infrastructure engineer on the Distributed Caching team, to discuss her journey through the tech industry.*


The past few years have seen an upturn in programs dedicated to guiding women towards careers in tech. But once you’ve begun a career in tech, where do you go? The tech industry is a vast and varied place, and there’s often little guidance on how to find your niche.


Tay Nishimura has found her calling as a site reliability engineer—but her journey hasn’t always been straightforward.


## Approaching tech from the outside


“I was and will always be a mathematician and visual thinker,” said Tay. She had started college as a math major; she was primarily interested in real analysis, which deals with proving calculus—in other words, a branch of mathematics that is especially distant from computer science. On some career advice from a professor, however, Tay took on a computer science double major and prepared for a job in the tech industry, starting with internships at Amazon and Google.


Of course, while there is a conception that a university degree in computer science might prepare students for a career in the industry, academia and business generally have different priorities.


“In school, they reward you for being theoretically skilled,” said Tay. “I got to the industry, and it was no math and all programming. It’s all practical—when you run into a problem, there’s a lot of pruning you have to do: you have to pick the answer that’s most practical to pursue. I was used to being rigorous, but that wasn’t really conducive to efficient agile development.”


For Tay, another factor was feeling like an outsider: many of her peers had been immersed in technology since childhood, but this was not the case for her. Poor eyesight runs in her family, and she had been discouraged from spending too much time looking at screens. “I had little exposure to computers,” she recalled.


As she began her first full-time job, Tay found herself struggling to reconcile her visual thinking style with the task of software development.


“I didn’t like coding,” she said. Thinking in code didn’t come naturally to her. To better understand what she was doing, she would translate code into drawings, make modifications to the drawings, and then translate those drawings back into code. It satisfied her desire for rigor, but also added overhead to her process.


“I had a lot of performance issues when I started working,” she said. “I was always told I was a great team member, and that I tackle the problems that other engineers avoid, but performance feedback was always speed related. Sometimes I’d push faster, and the quality of my code would suffer.”


One of Tay’s managers noticed that she wasn’t thriving in this environment, and she introduced her to two alternatives: product management and site reliability engineering (SRE).


Tay soon realized she was much better suited to either of those than her designated role at that particular company—especially SRE: “My slower pace was useful when making critical changes, because I would think through every failure mode,” she said. But there wasn’t space to grow into either of those roles at that particular company.


“I wasn’t meeting the bar for what my cookie cutter shape was supposed to be,” Tay recalled.


After three years, the promotion cycle approached, and Tay could only foresee her stress levels increasing. She was convinced she no longer wanted to be a software engineer. Tay switched jobs and took on a role on a product team at a commercial real estate company.


## When one door closes, another opens


Technically, Tay’s new job title was still “software engineer,” but the work was more similar to what might fall under a product management role at another organization. She also soon realized that the product management work she’d enjoyed at her old company was closer to technical program management (TPM). “I realized all these titles could mean different things at different companies,” she said. “Every company is different, and not every company works for you.”


Work-life balance at her new position was better than before, and with her newfound free time, Tay found herself gravitating towards computer networking. She studied it every weekend, creating visualizations of switches and routers and how packets move between them. She envisioned each network entity as a person with specific responsibilities. She then turned these into learning modules.


These modules eventually became[Project Reclass](https://www.projectreclass.org/) , a nonprofit vocational program aimed at teaching technical skills to the incarcerated as well as military veterans. Tay and her friend, Project Reclass co-founder Kunal Jha, began teaching computer networking classes at Rutledge State Prison in Columbus, Georgia.


“There’s rules in prisons, stuff you can’t bring in—so for an exercise, we’d have fake routers and fake switches, and they’d build a redundant network for a hospital in an army base,” she said. “It was fun for them, which made it fun for me.”


While this was highly rewarding, she didn’t feel as fulfilled at work. Her stress levels had improved, and her job involved architectural layouts and was highly visual—but it still wasn’t what she really wanted to do.


Then came the COVID era, and Tay’s entire office was laid off.


“I slept for three days straight,” she said. “I got out of bed and was determined to make a career change,” she said. She knew, now, after a year of “nerding out” in her free time, that she wanted to pursue SRE. She worried that she didn’t have enough experience, but began studying regardless.


> I would rather run out of money trying to find a role I loved than spend another year pretending to be something I already knew I wasn’t.


The pandemic also impacted Project Reclass, as many prisons suspended in-person vocational services. “We got kicked out,” said Tay, “so the choice was either give up on this project, or make it digital. I’ve always thought it’s frustrating that we teach computer networking, but they aren’t allowed to touch real routers. I wanted to make a simulator, so they could see what we were talking about.”


Tay architected[ToyNet](https://toynet.projectreclass.org/) , an open source learning platform with network emulation. Built on React with a Flask backend, ToyNet spawns containerized[Mininet](http://mininet.org/) instances, which simulate enterprise network traffic. Users can connect routers, switches, and hosts; configure IP addresses; and run system administration commands like` ping` and` arp` . Critically, ToyNet can be used by prison inmates with restricted internet access.


ToyNet also had a secondary function. Many SRE job listings asked for more experience than Tay had, especially when it came to cloud computing, but she had deployed ToyNet in a cloud environment, and it served as proof of her capabilities. “It was a split,” Tay recalled. “Companies that didn’t care about ToyNet didn’t get back to me, but every company that thought ToyNet was cool took me all the way to the last round.”


One of these companies, of course, was Datadog.


## A room of one’s own


In her first month at Datadog, Tay learned a number of technologies that were new to her: Kubernetes, chaos engineering, network traffic control, and Go. She also realized something else: in this particular role, her rigor and visual thinking style were major assets.


When she open sourced[Chaos Controller](https://github.com/DataDog/chaos-controller) , Datadog’s gameday automation tool, she was faced with the task of understanding a large codebase in a new language. So Tay did what she’s always done—draw a picture. For every file of the codebase she read, she drew a box, and drew arrows between the boxes.


> I actually implement by drawing. I translate this behemoth code base into a picture. I draw the relationships, I visualize these systems, and then I have a very clear understanding of what in those systems needs to be changed. I make my changes there, I show people for feedback, and then I translate it back into code.


In the past, this approach had taken a lot of time, which her previous managers didn’t love—but in this case, the extra rigor was well worth it. The task of translating a complex system into visual representations enables an especially deep understanding of what is actually going on in that system. Datadog’s Chaos Controller is, as the name implies, a Kubernetes controller, and deals with many state nuances. State management can be convoluted, and keeping track of it all is often challenging.


“Other engineers don’t need to \[draw,\] they can just work directly in the code,” said Tay. When she first made her drawings, she thought that she would be the only person to benefit from them. She thought that her colleagues already had a deep understanding of Chaos Controller and wouldn’t need the visualizations—but after presenting the drawings to her team, she realized that she wasn’t alone in finding the codebase daunting.


From Tay’s drawings of Chaos Controller. From Tay’s drawings of Chaos Controller.


“Going into it, I assumed it was just me. But I presented my drawings to other people, and they were like, ‘now I totally understand.’ I learned that not everyone felt comfortable touching the code because it was so complex.”


Tay began to realize that her visual thinking style wasn’t a deficiency–just a difference. And when she honed it more, it became highly useful to others.


“I started to understand that the very tendencies I used to find inconvenient or impractical were helping me succeed here,” she said.


## Beyond the job


Tay is now a member of Datadog’s caching platform team, responsible for building and maintaining a high throughput production system. Her experiences in chaos engineering and application development, her ability to break down complex systems into forms that can be better understood, and her teaching and communication skills have made her contributions invaluable.


> What I love about working here—I am allowed to work on whatever I believe in most at the time. People are interested in and benefit from the way I think about software. I feel valued as a human, even by strangers at the company.


“I have now achieved my peak nerd goals,” said Tay, “but the in-between wasn’t fun.”


There were times earlier on in Tay’s career when she wanted to leave the tech industry completely. She was unsettled by her first job, a highly competitive alpha-male-dominated space where intense, heated discourse was expected and encouraged. Feeling like an outsider started on her very first day, when she and the other new hires were introducing themselves: “I think I was supposed to be name dropping my school or showing my professional proficiency in that intro, but I was the first to go, and I totally missed the vibe. I said I was a dancer, and that has never landed so poorly. That was just not what they wanted my ‘extra thing’ to be.”


Tay had difficulty reconciling her communication style with that of her colleagues, and found that she would become stressed more easily than others. “Stress would go up, and I would just blank.” She struggled with impostor syndrome, developing confidence, and connecting with many of her peers. But she did find comfort among other women in tech.


“Throughout my career, it was women consoling me in the bathroom, forcing me to take pantry breaks, or visiting me on my mental health days,” said Tay. “It was women messaging me in the middle of the day reminding me I’m awesome before a dreaded performance review. It was women because many of them saw themselves in me—they saw similar communication and expectation challenges at the workplace. They happened to be my tribe.”


> Belonging is important to humans. Feeling understood is important to feeling safe. Not finding that feeling of safety drains us. Every woman who left tech entirely that I know mentioned they didn’t feel they belonged.


The difficulty of integrating into a new world is also the focus of much of Tay’s nonprofit work. Both her partner and her co-founder are military veterans, and there are unique challenges in transitioning from highly structured institutions like armed forces—and prisons—into life in the free civilian world. In fact, Project Reclass was founded specifically with incarcerated veterans in mind.


Balancing a full-time job with volunteer work isn’t easy, but Tay feels obligated to face the challenge.


Presently, the tech industry is generally a socioeconomically privileged field, and as the industry continues to grow, technology will make more jobs redundant and subsume more spheres of everyday life; without an understanding of power, privilege, and justice, tech workers can find themselves exacerbating the already vast social inequities that plague the vast majority of people on this planet.


“I would meet someone who’s lost their job, or seeing a throttled career path, and I’d know it’s me and my friends coding it away. I couldn’t sleep at night. I feel like I could spend my extra time making it worse, or I could spend that extra time trying to make it better.”


-
-
-
