---
schema_version: "1.0.0"
document_id: "16f89df08cc7f8d6046161d46462536b24671dac320edc63fcb1c5b2844d3506"
company_key: "yc-docker"
company: "Docker"
source_id: "yc-docker-rss-8dbda93e62b5"
canonical_url: "https://www.docker.com/blog/from-the-captains-chair-mohammad-ali-arabi/"
published_at: "2026-07-16T17:15:44+00:00"
first_seen_at: "2026-07-20T23:20:22.390152+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:6f965ac20907192449927ba8a1fd0f23bd64f4ea015e6e684d218aa9da2f98d6"
---

# From the Captain’s Chair: Mohammad-Ali A’râbi

*Docker Captains are leaders from the developer community that are both experts in their field and are passionate about sharing their Docker knowledge with others. “From the Captain’s Chair” is a blog series where we get a closer look at one Captain to learn more about them and their experiences.*


*Today we are interviewing Mohammad-Ali A’râbi, a Docker Captain based in the sunniest German city, Freiburg. He is the author of the book “*[Docker and Kubernetes Security](https://www.dockersecurity.io/) *,” a Best DevOps Book of the Year finalist in 2025. He is also a software engineer, public speaker, and community builder, organizing Docker meetups in Freiburg since 2022.* *Mohammad-Ali is originally from Iran and has a BSc in Mathematics and an MSc in Computer Science.*


*Caption: Docker Captains Summit in Istanbul, I’m the one with a red hat*


## Can you share how you first got involved with Docker?


In 2015, I was working at Cafe Bazaar, a tech company in Iran, as a backend engineer. Our backend was running on Django, so for a whole week, I listened to[Django Reinhardt](https://open.spotify.com/playlist/6BrieO5rNOHHeYStMdcCFk?si=7Y7eBUCeQhutU9-mTrg7ig) while trying to spin up the project. I was failing because of the dependency hell.


A colleague casually mentioned, “You can perhaps try using Docker; we’re using it in the CI.” Docker was 2 years old at the time, and I had never heard of it before.


So, I disappeared for one week, learning Docker, and next thing you know, I was creating CI pipelines for other projects.


*Caption: Cafe Bazaar in Iran, I’m the one in the red T-shirt (middle)*


## What inspired you to become a Docker Captain?


Between 2018 and 2019, I was working in Amsterdam. We had tech meetups quite often there, and I loved it about Amsterdam. We moved back to Freiburg in 2019, and I started working at a smaller company, where I introduced git, CI/CD pipelines, and Docker. People would come to me with their git and Docker questions. So, I decided to write them down on a Medium blog for my own later reference. But I learned the content is useful for the community, so I kept on writing. At some point, I was writing a blog post on git every week.


When the pandemic hit, I got depressed, so I decided to start a meetup group in Freiburg, because otherwise, there was none. I attended an online Docker Community All Hands and an online KubeCon, and in the meantime, I was looking for venues to host my first meetup.


> ***I will bring my coffee!***


In 2022, I got a LinkedIn message from a CEO trying to hire me. I told him, “I just got a new contract, but we can talk about other collaborations.” We set up a meeting, and I wrote, “I will bring my coffee!” It was because their office was in the same building as where I live. I went down there, having a Docker-branded mug filled with coffee (caffè crema with a stain of milk), saying, “Hello, neighbors!” They agreed on hosting an in-person Docker meetup.


Our first meetup was in November 2022, and we had only one attendee, who came all the way from Strasbourg, France. In the end, it was him, my wife, me, one of the founders and her boyfriend, and an engineer from the company.


Our second meetup was a watching party, watching Docker Community All Hands. By that time, I had two blog posts published on Docker’s blog, I had a talk at that particular event, and I won the title of best Docker Community Leader.


When I applied to become a Captain in early 2023, many already knew me at Docker.


## What are some of your personal goals for the next year?


I want to double down on education and storytelling.


I recently published[Black Forest Shadow](https://www.dockersecurity.io/amz/bfs) , a fantasy story set in 1865 Freiburg that teaches container security through narrative. It’s part of a bigger idea I’m exploring: making complex DevOps concepts memorable through story, visuals, and characters. One other project I’m working on is the workshop series[Docker Commandos](https://www.dockersecurity.io/blog/docker-commandos) , with which I introduce different Docker commands.


On the technical side, I’m working on the second edition of Docker and Kubernetes Security, especially covering[Docker Hardened Images](http://localhost:3001/blog/docker-hardened-images-are-free) .


*Caption: Docker Commandos Pack*


And on the community side, I want to grow the Freiburg meetup into something more consistent and connected to the broader ecosystem. It’s already a CNCF chapter as well, but I have been playing with the idea of starting a Java User Group (JUG) to attract a wider audience.


## If you weren’t working in tech, what would you be doing instead?


I would probably have become a mathematics professor researching logic. I did an unfinished master’s in Iran researching Categorial Grammar, which models natural languages using mathematical logic. My[master’s thesis](https://www.researchgate.net/publication/351853213_A_Basis_for_Event-Driven_Programming) in Computer Science was also basically mathematical logic.


Or I would have become a researcher in ancient languages. I can read Old Persian cuneiform and Book Pahlavi, which is currently not fully deciphered, to be added to the Unicode. If I weren’t doing tech, I would dedicate my time to answering[the remaining questions](https://www.unicode.org/L2/topical/bookpahlavi/) . ****


## Can you share a memorable story from collaborating with the Docker community?


Publishing the book Docker and Kubernetes Security would not have been possible without the Docker community. So, the story goes like this:


Shortly after I became a Docker Captain, Packt, the tech publisher, reached out to me and suggested that I write a book with them. I declined at first, as I didn’t feel I was knowledgeable enough to write a book. But they were very persuasive.


Two years later, I finished my manuscript and threw it over the fence. As I was waiting for them to do their magic, they went through a reorganization, and they finally said they can’t prioritize my title. They wrote to me, “You can find a new publisher.” I found a new publisher, and that was me.


*Caption: Docker booth at WeAreDevelopers conference*


I started asking Docker Captains to review the work. I gave beta versions to our little Freiburg community. And when it came out, many Docker Captains, Docker employees, and members of the Docker community supported me by buying the book or spreading the word.


## What’s your favorite Docker product or feature right now, and why?


Docker Hardened Images, because Shai Hulud is lurking in the deep, and Jack the Bitcoin Miner is installing cryptominers on every vulnerable server, so the ecosystem deserves an open-source, CVE-free set of base images. And this should be available to everyone, not only the paying customers, because we’re all in this boat together.


*Caption: Jack the Bitcoin Miner fighting Gord the Guardian*


## Can you walk us through a tricky technical challenge you solved recently?


Tech problems are usually not tricky; designing the solution is. It’s tricky to understand if you’re overengineering or if your solution is too simplistic and not future-proof. Last week, I was designing a new microservice, and I created a few rules for myself to guard-rail my solution:


1. **Decisions should be able to be postponed.** Don’t lock in on a decision yet. I introduced interfaces for our repository and injected its implementation, so that if we decided on using a different database later, all we have to do is add a new implementation and change one line of code to inject it into the service.
2. **There should be one way to do things.** If you have three different ways to run the project locally, they will eventually go out of sync, and all end up broken. Choose a main solution, don’t do Docker Compose and Devcontainers and local npm start all at the same time.
3. **Automate everything.** If things are manual, they are more prone to error and more time-consuming. If your deployment is SSHing into a server, changing a commit hash, and restarting the Docker Compose service, you’re doing it wrong.
4. **Don’t trust AI.** I use Claude Code, and I have to correct it half of the time, saying, “Don’t do that, do this.” If you’re letting the AI write your code while you’re drinking coffee in the kitchen, you’re in for disaster.[Research](https://medium.com/@lewis_75321/new-study-finds-ai-models-write-code-with-security-flaws-18-50-of-the-time-f4515736b2dc) shows that a significant portion of AI-generated code is insecure.
5. **Test everything.** Add CI checks for everything. I had jobs for formatting, linting, running tests, checking the coverage, checking Docker image vulnerabilities, and even the commit messages. Now, based on the commit messages, I bump the version automatically using semantic versioning and trigger a new release.


## What’s one Docker tip you wish every developer knew?


You can generate SBOM attestation upon build very easily, it’s just passing a flag on CLI, setting a new argument on the CI job, or two lines of code if you’re using Docker Bake.


*Caption: SBOM attestations make it easier to find CVEs*


Using the CLI:


```text
$ docker buildx build --sbom=true -t <image> .


```


If you’re using Docker Bake:


```text
variable "TAG" {
default = "latest"
}


variable "REPOSITORY" {
default = "mithra-backend"
}


group "default" {
targets = ["backend"]
}


target "backend" {
context = "."
dockerfile = "Dockerfile"
tags = ["${REPOSITORY}:${TAG}"]


attest = [
{
type = "provenance"
mode = "max"
},
{
type = "sbom"
}
]
}


```


Then you can build by:


```text
$ docker bake


```


And in the CI:


```text
- name: Build and push with docker bake
uses: docker/bake-action@v5
with:
files: ./docker-bake.hcl
push: true


```


If you’re not using Docker Bake yet, it’s worth looking into. It makes Docker build more delicious.


*Caption: Docker Commandos doing a bake-off competition in Asgard*


## If you could containerize any non-technical object in real life, what would it be and why?


I would create snapshots of the world so that I can choose which version to live in. Sometimes I play Fallout: New Vegas to escape reality, which is ironic. But at least it has[Big Iron](https://open.spotify.com/track/0AQquaENerGps8BQmbPw14?si=ff016544115b4a3b) in it.


## Where can people find you online?


I have a website with all my links:[aerabi.com](http://aerabi.com/)


LinkedIn is my main social media platform; follow me there:[/in/aerabi](https://www.linkedin.com/in/aerabi/) .


And when I miss the good old Twitter, I sometimes write on BlueSky:[@aerabi.com](https://bsky.app/profile/aerabi.com) .


## Rapid Fire Questions


### Cats or Dogs?


Homo Sapiens


### Morning person or night owl?


Vampire


### Favorite comfort food?


Fesenjān, but if you don’t know what that is, sushi


### One word friends would use to describe you?


Crazy


### A hobby you picked up recently?


Writing dark fantasy. Though honestly, lately I just call it “non-fiction.”
