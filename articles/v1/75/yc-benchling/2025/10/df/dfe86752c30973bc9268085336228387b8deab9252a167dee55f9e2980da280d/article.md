---
schema_version: "1.0.0"
document_id: "dfe86752c30973bc9268085336228387b8deab9252a167dee55f9e2980da280d"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/behind-the-model-building-boltzgen-with-hannes-staerk"
published_at: "2025-10-27T07:00:00+00:00"
first_seen_at: "2026-07-20T03:30:03.260200+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:97c695a201f32bcf4d6857479e0c0d8c9d103c403deb2b695245ada819f7ac66"
---

# Behind the Model: Building BoltzGen with Hannes Stärk

***Summary:*** *BoltzGen is a new generative protein design model built by MIT researchers and integrated into Benchling. This post goes behind the scenes on how it was built and why transparency matters as much as performance.*


-


*BoltzGen is designed to generate protein binders of any modality, including nanobodies, peptides, and cyclic peptides, against virtually any biomolecular target, validated across 26 wet labs from pharma and academia.*


-


*Against novel targets with less than 30% sequence similarity to known binders, BoltzGen achieved low nanomolar results roughly 66% of the time, on problems most models don't attempt.*


-


*The team's guiding principle: be honest about what the model can and can't do. Scientists should treat BoltzGen as an iterative design partner, not a black box, and start small before scaling up experiments.*


---


With[BoltzGen](https://boltz.bio/boltzgen) launching this week and coming to Benchling, scientists can generate proteins directly within the platform, exploring a new frontier in generative biology. But as excitement for models builds, transparency, validation, and realism are just as important as innovation.


In this conversation,[Hannes Stärk](https://hannes-stark.com/) , MIT researcher and co-creator of the BoltzGen model, and[Mihir Trivedi](https://www.linkedin.com/in/mihirt) , engineer on Benchling’s Scientific Models team, discuss how BoltzGen was built, what makes it different, and why setting clear expectations for these models is key to moving the field forward.


**Mihir Trivedi: Let’s start with your background. How did you find your way into this intersection of machine learning and biology? And what led you to build BoltzGen?**


**Hannes Stärk** : My background is in informatics and computer science, not biology, but I’ve always been fascinated by the natural sciences. Machine learning gave me a way to combine both: using math and computation to explore the complexity of biology.


As for how BoltzGen came to be, it grew out of working hand-in-hand with more than 26 wet-labs across academia and pharma. Each lab brought a concrete, often unsolved challenge in drug discovery; these were highly specific challenges across a wide-range of modalities. Those labs directly shaped what we call the **‘** design specification language’ behind BoltzGen, ensuring the model was built for practical scientific use, not just to perform well on benchmarks.


**Mihir: What problem were you hoping to solve with BoltzGen?**


**Hannes** : We wanted to build a general model for protein binder design, one that could generate proteins of any modality, including nanobodies, peptides, cyclic peptides, etc., against biomolecular targets of any kind.


Boltz-1 focused on structure prediction. Boltz-2 added binding affinity. BoltzGen *generates* new proteins of any modality while reasoning about structure and interactions at the same time. This binder design problem is what we want to solve in its full generality while providing a ‘design specification language’ that constrains and controls the model outputs to adhere to the diverse needs of real-world drug discovery campaigns.


The goal is a single model that can flexibly design against a broad range of targets, all guided by user-defined constraints.


**Mihir: What’s unique about BoltzGen’s architecture?**


**Hannes:** There are three main innovations.


1.


**Unifying structure prediction and design:** We use a purely geometry-based residue representation that allows the model to perform design and structure prediction simultaneously. This leads to expressive features capturing structure and their interactions from the folding task, which allows for designing tight and precise interactions in the design task.


2.


**Multi-task training:** Our architecture allows us to train with a wide variety of tasks, ranging across all biomolecules. This not only leads to a single general model, but it also exercises the model in different contexts and enables transfer learning between them to extract more information and generalizable patterns from the data.


3.


**Design specification language:** This lets users encode the constraints they care about — like sequence length, binding region, or flexibility — so the model’s outputs better reflect the realities of drug discovery.


**Mihir: What have you learned from experimental validation so far?**


**Hannes:** Since the experiments come from 26 wet labs from pharma companies or academic labs, the validation is very diverse. Each collaborator is at the frontier of drug discovery or biological research, their real-world problems are impactful and hard.


In one project, collaborators tested just six sequences designed for a disordered peptide target and found binders. In another, we saw in vivo evidence that designed peptides could modulate disordered regions inside cells. That kind of validation which is functional, not just structural, is what we care about most.


We also benchmark BoltzGen on 9 novel targets that have less than 30% sequence similarity to any protein for which there exists a bound. Against two-thirds of those targets we achieved low nanomolar binders, roughly a 66% success rate on problems that most models don’t even attempt.


I think our field often tests models only on cases where we already know they’ll succeed. That doesn’t actually tell us where they fall short or how to make them better.


The real progress comes from testing in the hard places, where the models fail, where they surprise us. That’s how we learn, and that’s how the field moves forward.


**Mihir: It’s great to hear how intentional the team at BoltzGen has been with model development. Especially as this is such an emerging field, what are the principles guiding your approach?**


**Hannes:** Two key things stand out **.**


First, BoltzGen moves beyond the narrow test cases that dominate model validation today. We focus on new, unseen targets, and we pair that with wet-lab validation through real collaborations. That combination of harder science and experimental grounding is what helps the field mature.


Second, we’re careful to stay transparent about what the model can and can’t do. There’s a tendency to oversell breakthroughs. BoltzGen performs well, but it’s not magic. It’s a powerful tool when used thoughtfully, and it’s most effective when scientists understand both its capabilities and its limits.


**Mihir: What should scientists keep in mind as they start experimenting with BoltzGen?**


**Hannes:** No model, including BoltzGen, is plug-and-play. You can’t write a specification, click “run,” and get the perfect design. That idea sounds great, but it’s not reality.


Treat BoltzGen as an iterative design partner. Start small, inspect your results, and adjust parameters. Don’t just go with the pre-sets. Explore different binding sites or constraints, rerun your designs, and compare. We’ve intentionally exposed many control options — binding-site flexibility, sequence length, exclusion zones — so users can see how changes affect outcomes.


Models like this reward experimentation. The more you interact with them, the more intuition you build.


**Mihir: It seems like BoltzGen will enable scientists to design things they couldn’t before. How do you see it changing their work?**


**Hannes:** I think it expands the frontier of binder design to targets that are more novel than what was previously possible, so better generalization. Also, this capability is now available for more binder modalities and more types of targets.


However, we want our paper to be a grown-up version of what we see in the field, we’re not making “this model can do everything” claims. BoltzGen brings stronger validation and broader generalization than many previous efforts, but it’s not without limits.


Having a high affinity binder does not mean having a therapeutic candidate – there is a lot more to that. We still have challenges in selectivity and developability, and while BoltzGen starts to address some of them, there’s more to do and we are on it! If we’re not transparent about those gaps, we risk setting expectations too high. When people get disappointed, they don’t just abandon one model, they lose confidence in the entire idea of using models in biology. Transparency builds trust and lasting progress.


**Mihir: Agreed. That balance between ambition and honesty is critical whenever we release new technology.**


**You’ve mentioned the importance of open collaboration. How are you thinking about the community around BoltzGen?**


**Hannes:** For me, the central motivation in building these models is to help humanity discover new biomolecules. I won’t be the one making those discoveries, it will be the scientists and companies using these tools.


AI isn’t an end in itself; it’s a means to an end. The real value comes when it leads to something useful, a new therapeutic, a new mechanism, a new way of understanding biology. That’s why openness is essential. When people can access, test, and improve these models, the whole field moves faster.


And when these tools become available on platforms scientists already use, like Benchling, they’re immediately at the fingertips of biologists. They don’t need to be ML experts to start designing and testing. That accessibility is what will make AI a real part of everyday science.


**Mihir: Exactly. That’s our goal too, to democratize these tools and meet scientists where they work.**


*Boltz-2 is available on Benchling today. BoltzGen will be available in Benchling in beta within the next few weeks.*
