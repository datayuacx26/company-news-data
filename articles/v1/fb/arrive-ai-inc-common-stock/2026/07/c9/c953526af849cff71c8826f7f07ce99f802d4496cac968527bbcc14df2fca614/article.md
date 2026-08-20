---
schema_version: "1.0.0"
document_id: "c953526af849cff71c8826f7f07ce99f802d4496cac968527bbcc14df2fca614"
company_key: "arrive-ai-inc-common-stock"
company: "Arrive AI Inc."
source_id: "arrive-ai-inc-common-stock-news-import-d3b20dec47bd"
canonical_url: "https://www.arriveai.com/blog/evan-furnish-autonomous-delivery-whataburger-order-dallas"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-27T21:57:57.072777+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:71591767184b0a2359798ddd4a01a644c28472c8b8bea6fd8fe84604a60e7230"
---

# What we learned about autonomous delivery from a Whataburger order in Dallas

*Evan Furnish, Arrive AI's Product Manager of Integrations, went to Dallas to observe drone delivery firsthand. What he found changed how we think about the whole industry.*


**TL;DR:** Autonomous delivery is fast once it gets moving. The problem is the handoff before it does. Arrive AI built the infrastructure that solves it, and it is already running inside a hospital.


The drone took less than three minutes. The order took 30 minutes to get inside the drone.


That is what Evan Furnish, our Product Manager of Integrations, found when he ordered a Whataburger through DoorDash with drone delivery during a research trip to Dallas. The delivery itself was fast. The handoff before the delivery was not.


Evan sat down with Dan O'Toole on The Dan Show to walk through what he observed, in Dallas, at Purdue with Starship, and across the broader autonomous delivery ecosystem. What follows is a summary of the key ideas from that conversation. The full episode is linked below.


# We already solved this problem


Before we get into what Evan found, here is what matters most: Arrive AI has a live deployment at Hancock Regional Hospital in Greenfield, Indiana. Two Arrive Point units are running on site, one outside the lab, one outside the cancer center. An autonomous ground robot handles specimen transport between them.


The results: a 50 percent reduction in lab transport time and a 98 percent improvement in on-time delivery.


It is the world's first fully asynchronous robotic medical delivery inside a hospital.


Evan lives in Hancock County. He drives past the deployment on his way to work. He is not just building the integration layer for this system; he is a neighbor to it. His family receives care at the same hospital his work supports. That is the kind of thing that does not show up in a product spec but does show up in how a team builds.


The problem Evan observed in Dallas and at Purdue is real. Our answer to it is already running.


# What Evan saw in Dallas


When Evan placed the order, he had two options: drone delivery or a human driver. He chose the drone. What happened next was not what he expected.


A person got in a car, drove to the Whataburger, drove back, unpacked the bag, repacked the food into a drone-compatible container, unlocked the drone area, loaded the drone, and waited for it to sync before it could take off. From order placed to drone in the air: 30 minutes.


Once the drone was airborne, it covered the distance in under three minutes. The technology worked. The process around it did not.


Evan also noticed that figuring out whether drone delivery was even available in a given area required a lot of searching inside the DoorDash app. For something that looks seamless in a product video, it was hard to find and hard to use.


# What Evan found at Purdue


After Dallas, Evan headed to Purdue University to observe Starship robots delivering food on campus. Starship has since announced it is pulling out of all 60 of its US campus locations. What Evan saw helps explain why.


The average order time was 45 minutes. The first 30 of those minutes were before the food ever got inside the robot.


Starship does not send the order to the restaurant until the robot is physically parked outside and waiting. So the robot travels to the restaurant, parks, and only then does the kitchen receive the ticket. The robot sits idle while the food is made. Then the food goes in. Then the delivery starts.


The robot leg of the journey was quick. The wait before it was not.


The pattern is the same in Dallas and at Purdue: autonomous delivery technology is fast once it gets moving. The problem is everything that happens before it gets moving.


# The infrastructure gap


When most people think about autonomous delivery, they picture the drone or the robot. What they do not picture is the handoff, the moment where the package moves from the building to the robot, or from the robot to the recipient. That handoff is where time gets lost. It is where humans get involved. It is where the system slows down.


Evan put it plainly: restaurants today have full optionality. You can order from DoorDash, Uber Eats, their own drivers, or all three. The infrastructure supports it. Autonomous delivery does not work that way yet. A property owner that wants drone delivery today has to pick one provider and lock in. No switching. No combining. No network.


That is the gap Arrive AI fills.


The Arrive Point is a secure, climate-controlled smart delivery endpoint that any robot, drone, or human courier can use. It is not tied to a single provider. When a robot arrives, it drops the package and moves on. The recipient gets a notification and picks up on their own schedule. Nobody waits for anybody. That is asynchronous delivery, and it is what makes the network scale.


# Watch the full conversation


Evan and Dan cover all of this in the latest episode of The Dan Show, including the 100 percent interoperability vision, what Starship's campus exit means for the industry, and how Arrive AI thinks about the problems that will emerge as autonomous delivery scales.


Watch it here:[https://youtu.be/1Th0-H14GmQ](https://youtu.be/1Th0-H14GmQ)


Subscribe to The Dan Show on YouTube to stay current as new developments happen at Arrive AI.


‍
