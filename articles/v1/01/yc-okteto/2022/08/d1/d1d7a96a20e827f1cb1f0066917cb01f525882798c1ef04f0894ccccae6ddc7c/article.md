---
schema_version: "1.0.0"
document_id: "d1d7a96a20e827f1cb1f0066917cb01f525882798c1ef04f0894ccccae6ddc7c"
company_key: "yc-okteto"
company: "Okteto"
source_id: "yc-okteto-rss-a64bce3f80ea"
canonical_url: "https://www.okteto.com/blog/what-is-the-kubernetes-release-team-and-why-you-should-consider-applying/"
published_at: "2022-08-25T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:32.770894+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:fefd4428e4d71a96f2f586c754570b4410c109f95035c6936b6e22fc1bc3c151"
---

# What Is the Kubernetes Release Team and Why You Should Consider Applying

# What Is the Kubernetes Release Team and Why You Should Consider Applying


## Learning Kubernetes (7 Part Series)


1. [Kubernetes Basics: Kubernetes Tutorial for Beginners](https://www.okteto.com/blog/kubernetes-basics/)
2. What Is the Kubernetes Release Team and Why You Should Consider Applying


3. [Beginner’s Guide to Kubernetes Deployments](https://www.okteto.com/blog/beginners-guide-to-kubernetes-deployments/)
4. [Kubernetes Network Policy: A Beginner’s Guide](https://www.okteto.com/blog/kubernetes-network-policies/)
5. [Kubectl Cheat Sheet Commands & Examples](https://www.okteto.com/blog/kubectl-cheat-sheet/)
6. [Containerization and Kubernetes: A Guide](https://www.okteto.com/blog/kubernetes-containerization/)
7. [How To Speed Up Container Image Builds](https://www.okteto.com/blog/how-to-speed-up-container-image-builds/)


Kubernetes 1.25 just got released. I consider myself very lucky to be able to help with the release as the CI Signal Lead and get to work with the amazing folks in the K8s community! If this is something that sounds interesting to you, but you’ve no clue how to get started, then keep reading, and I’ll try to answer most questions people have when getting started with the Kubernetes release team.


## First Things First: What Is the Kubernetes Release Team?


Kubernetes has grown exponentially in the last few years and has seen adoption by countless companies. But at the end of the day, it is still an open source project being maintained by the community! For each new Kubernetes release, there is a team of community members assembled that is responsible for the day-to-day work which goes into successfully releasing the next version of Kubernetes. This is the Kubernetes Release Team.


The release team is further divided into various sub-teams to ensure the smooth functioning of all horizontals. Each release has a Release Lead, and they are helped by 3-4 Release Lead Shadows. The same is true for each subteam: there is the subteam lead, accompanied by 4-5 shadows. You start in the release team as one of the subteam’s shadows and can eventually grow your way up to the Release Lead!


If you’d like to know more about the various roles on the release team, you can check out the[official role handbooks](https://github.com/kubernetes/sig-release/tree/master/release-team/role-handbooks) .


### My Personal Experience With the Release Team


I started with the Kubernetes 1.23 Release Team as a CI Signal Shadow. The CI Signal Team is responsible for monitoring the CI throughout the release and ensuring any test failures get fixed before the release. Then in the 1.24 cycle, I shadowed under the Release Notes team. This team is tasked with making sure the release notes for the upcoming release are proper. In the recent 1.25 release, I returned to the CI Signal Team but as the role lead this time.


My every time on the release team, I learned a lot. Being a shadow in the first two cycles gave me the time to understand how the release process for a project as big as Kubernetes works. One of my biggest learnings, and one of my biggest surprises as well, was watching and learning how to collaborate effectively when you’re working on such a big open source project, with so many contributors, distributed across multiple timezones! I’m, to date, awestruck looking at what all the team manages to accomplish. My time leading the CI Signal team in 1.25 also taught me how to mentor and set others up for success. I feel this is something very important for the health of open source projects because if present project maintainers fail to enable others: the project will eventually die. Open source is always a team effort, and ensuring you’re mentoring new contributors to lead in the future is as critical as fixing issues!


## Why Should I Consider Applying?


One of the reasons you should consider joining the release team is if you were already looking to get started with contributing to the Kubernetes project. Kubernetes is HUGE. While full of helpful people, jumping in and figuring out how the open source project works can be scary and overwhelming. Joining the release team, in my opinion, is one of the best ways to get started with contributing to the project. Joining as a shadow allows you to directly get mentored by the role lead and the broader release team. Not only do you end up learning about the release process, but you also get to learn a lot about how the open source K8s project is structured. Thanks to your time on the release team, you’ll have a much better idea of all the verticals that exist in the project.


Other than that, being on the release team itself brings with it a lot of learnings. Depending on what area you want to grow in, there’s a lot to learn in every role. For example, if Documentation interests you, you could apply to be a Docs shadow. As part of this, you’ll be chasing pull requests to the Kubernetes documentation and will, in turn, get exposed to various things one needs to consider when maintaining and writing documentation for a project as big as K8s. Similarly, if you want to learn more about how the CI and testing are done for K8s releases, you could apply to be on the CI Signal team. Regardless of the role you apply to, I can assure you there will be a lot of opportunities for you to learn and gain experience!


## Okay, I’m Sold: How Do I Apply?


Now that you know how cool and exciting being part of the Release Team is, I know you probably want to apply right away. But how do you do that?


A few weeks before each release cycle begins, an application form is floated to apply to be on the release team. You can receive notifications for this if you join the Kubernetes dev mailing list:[https://groups.google.com/a/kubernetes.io/g/dev](https://groups.google.com/a/kubernetes.io/g/dev)


> You can find the application form for the current 1.26 Release team[here](https://bit.ly/k8s126-shadow-form) :) Open till September 2nd, 2022


The application form does a pretty good job of explaining what is expected in your responses, so I won’t get into that in this blog. But I do want to point out something which I’ve seen discourage a lot of people. It is okay to not be selected the first time, and you should totally keep applying again if you’re interested! You have no idea the number of applications the release team receives each cycle for its limited number of roles.


I personally know folks who got accepted on their fourth application. I, too, didn’t get accepted the first time I applied. I won’t sugarcoat and say that it doesn’t feel demotivating to get your application rejected, because I know how it feels! But what I want to emphasize is that not getting selected isn’t a reflection of your skills at all! Having experienced selecting shadows for the CI Signal team in 1.25, I can tell how sad it feels passing on so many deserving candidates simply because of having limited bandwidth to mentor folks.


So please don’t lose faith if you don’t get accepted the first few times you apply. I would say that having had prior contributions to the project or just researching about your role properly definitely helps your application significantly!


All the best for your application, and I hope to see you grow in the Release Team. Also, if you’re looking to learn more about Kubernetes, checkout our[Kubernetes Tutorial for Beginners](https://www.okteto.com/blog/kubernetes-basics/) !


## Learning Kubernetes (7 Part Series)


2. What Is the Kubernetes Release Team and Why You Should Consider Applying


Arsh Sharma


Developer Experience Engineer / Emojiologist 😜


[View all posts](https://www.okteto.com/blog/authors/arsh-sharma/)


[kubernetes](https://www.okteto.com/blog/tags/kubernetes/)


[open-source](https://www.okteto.com/blog/tags/open-source/)


#### Share this:
