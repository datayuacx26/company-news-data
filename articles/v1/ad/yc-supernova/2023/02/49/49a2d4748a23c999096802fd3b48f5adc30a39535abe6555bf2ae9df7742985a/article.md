---
schema_version: "1.0.0"
document_id: "49a2d4748a23c999096802fd3b48f5adc30a39535abe6555bf2ae9df7742985a"
company_key: "yc-supernova"
company: "Supernova"
source_id: "yc-supernova-rss-864f3bee1480"
canonical_url: "https://www.supernova.io/blog/how-userlane-kickstarted-their-design-system-using-supernova"
published_at: "2023-02-21T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:40.084347+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:95740393fdd8e885284d575cbede896bfa8a658a4c61ff6c05baa98b78fed507"
---

# How Userlane Kickstarted Their Design System Using Supernova

In late 2022,[Userlane](https://www.userlane.com/) shared its design system[Burrito](https://userlane.supernova-docs.io/burrito/1/get-started/welcome-to-burrito.html) , with the public. The Userlane platform is designed to make any software application easy to use by guiding people through processes in real-time and offering on-demand support directly within applications. We chatted with their design system coordinator,[Anna Chojnacka](https://www.linkedin.com/in/annach/?originalSubdomain=de) , to tell us all about Burrito, how it came about, and how they used Supernova to create it.


### **Tell us a bit about the team at Userlane.**


We're at that startup going into a scale-up stage, with around 100 people at the moment. We're a digital adoption platform and part of the home office/digital workplace movement. The company is six years old already, so we were really well-positioned to capitalize on the huge changes companies have undergone moving to remote or hybrid setups over the past couple of years. The product team is nine people, which includes us designers. There's also a larger engineering team of around 20 people, and the rest of the team is mainly around the business side of things. More and more people are starting to use Burrito with improvements to the engineering documentation and the addition of brand guidelines.


### **Talk to us about the design team and how your design system Burrito came about.**


From the beginning, we have been a remote-first team. We have a physical office, but many people are all over Germany or Europe. To support that way of working, we definitely needed to have all our assets digitized on the cloud so that we could work together efficiently and with access to the same information simultaneously.


We, as designers, kicked off the idea of a design system, so the process was design-driven. But the team also realized how it could improve our work. The goal was to make us more efficient by creating a centralized source of truth that designers and developers could go to quickly for what they needed.


Userlane was always aware of the value good design brings to customers, so when the moment came where we had an actual stable team of designers, we wanted to increase our design maturity and quality of the product. Building a design system was a way to feed two birds with one seed.


### **What were the problems you faced while trying to create a design system?**


Our team is using Figma and that’s where all our components and libraries live. We started from the design side, trying to create consistency, using components that don't need constant recreation. Then, we encouraged developers to also do it on their side. We realized doing it in Figma wouldn't be enough because a design system that is just in Figma isn't really a design system. After all, it needs the implementation part, as well as documentation. Meanwhile, we had components in Figma, Storybook stories for developers, some stuff on Confluence, and other information living quietly in different people's heads.


We decided to bring this information together somewhere, and it needed to be transparent and accessible for developers to use and contribute to the design system. Centralized documentation was a no-brainer.


We don't have a dedicated design system team because we're not big enough yet to do that. We adopted a federated model on the design side, where we all contribute to the library and documentation. While on the engineering side, we had experienced devs help kick off the effort, and now continue building upon that with a couple regular contributors.


### What led you to Supernova as a solution?


Once we knew the problem of wanting to increase the quality and consistency of design and make it more efficient for everybody to work together, we started looking at how to do that. We looked at different tools, including the ones we were already using, like Notion or Confluence, but they're not really designed with design systems in mind. So we did some research.


We looked at Zeroheight and Knapsack, for example, but we needed something that would provide a nice balance of designer and developer-oriented features. It also needed to be easy for designers to drive because we had no delusions of getting a fully-fledged design system team anytime soon, and we wanted to avoid making unnecessary demands on developers’ time. We wanted something that would be easy for us to use and allow developers to integrate into their process and use effectively. Supernova came out on top.


### **How did Supernova help solve your problems?**


When it came to Supernova, the setup was super easy. The barrier to entry to use it and start creating your documentation was low because it's all quite simple. And for any issues that came up, we had a direct line of communication and definitely appreciated how communicative you all were.


We imported tokens from Figma. The components overview table is something we love. It's super handy to have it all in one place. Because beforehand, we had a very sizeable, complex, and fiddly spreadsheet. If you want to bring it into any documentation, it just doesn't work unless you start inserting PNGs. With Supernova, you can embed things like a component health indicator in specific documentation pages, which is super nice.


The fact that you can embed Storybook also helps us a lot, and obviously, embedding Figma, which was one of the main reasons we went with Supernova.


### **Any other useful features?**


We also customized the design with a couple simple CSS overrides. It made me quite happy that I didn't need to get a front-end developer to help.


Product design wasn't the only thing we started using Supernova for. We started documenting our brand guidelines in Supernova, which previously also didn’t have a single source of truth. It's still in the early stages, but things like color palettes and centralized documentation make it helpful to have everything in one place.


### **How did adopting Supernova impact how you work?**


We had a design system implementation in place before starting to work with Supernova. Building the documentation is what made us look more in-depth at standards and collaboration best practices. For example, we started cleaning up our Storybook and information architecture to ensure consistency with designers and developers across the board. We definitely raised our standards of what is good enough to work with. Even though the new design system is still in the V1 stage, it helped us think differently.


We have to think about how and when we document our work. We figured it's best to think about the documentation when you develop or design the component. We found out that thinking about what we'll be documenting actually verifies what we need to build, a self-serving loop. For example, if I'm thinking about a new component, I will have to document its sizes. Standard sizes would be S, M, and L, but if we won't have L in use anywhere, I don't have to build it. Not needing to develop it means it saves us time on both the dev and design sides. It makes us more pragmatic in that way, more efficient.


### **What's next for Burrito?**


The next steps will likely be around documenting patterns and big picture things — not necessarily adding more components — but standardizing how the product behaves and growing the developer side of the documentation.


### **What advice would you give to other teams going through the same process?**


I encourage teams to look at their resources (in terms of people and time) and how they want to organize them. The most important thing is to come up with a workflow that will produce the desired results. Overall, using Supernova is very easy; it's deciding who and when can produce the content that presents the challenge for most teams (especially small ones).Templating your documentation also goes a long way in the beginning. And, of course, bring in developers as soon as possible to ensure what you build is beneficial for everybody involved.
