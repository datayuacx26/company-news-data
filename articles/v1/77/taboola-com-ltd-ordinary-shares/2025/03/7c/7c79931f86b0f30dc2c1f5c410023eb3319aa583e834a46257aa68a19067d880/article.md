---
schema_version: "1.0.0"
document_id: "7c79931f86b0f30dc2c1f5c410023eb3319aa583e834a46257aa68a19067d880"
company_key: "taboola-com-ltd-ordinary-shares"
company: "Taboola.com Ltd."
source_id: "taboola-com-ltd-ordinary-shares-news-import-1349b39c90c0"
canonical_url: "https://www.taboola.com/engineering/real-traffic-ci/"
published_at: "2025-03-04T08:00:27+00:00"
first_seen_at: "2026-07-22T15:33:14.584366+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:5f4a7b6836afe81b499106d75c10a2a1138d197e4d3ddf72ac45720196f0d9fc"
---

# A Key Component in Taboola CI/CD Pipelines for Achieving High Production Stability

## Automated Verification CI pipelines


In theory, testing in a controlled environment should cover all bases. In practice, nothing beats the accuracy of replaying real-world traffic in your CI/CD pipeline.


At Taboola, in our continuous integration (CI) pipelines, we employ a suite of automated tests utilizing real traffic. These tests are executed on a feature branch against our production branch to measure key performance metrics and ensure correctness. This service in particular is responsible for serving Taboola recommendations at the rate of 1 million requests per second from thousands of machines across 7 data centers. This automated CI pipeline not only helps to maintain its correctness but also allows all engineers to iterate fast on this recommendation flow.


In this blog post, we delve into how we incorporate real traffic within our CI pipeline and maintain high standards without traditional QA.


## Testing with Real Traffic?


Testing against real traffic significantly boosts the confidence of Taboola engineers, embedding accountability into their work. To successfully implement this testing methodology, several key considerations must be addressed:


- Impact on Production: Though there are great advantages of having the ability to test code against production traffic, we must also minimize the possible impact to services Taboola provides.
- Traffic Quality: To test the modified code effectively, we must be comprehensive about the traffic quality it receives. It should provide good coverage of our traffic, and address most features of Taboola service.
- Request Rate Stability: In order to measure the performance between the production branch and a feature branch, we must have the ability to send steady and consistent high quality traffic to both servers.


Our exploration led us to evaluate tools like the NGINX mirroring module and the GoReplay framework. Both tools can replicate traffic to different destinations, but we found that the effectiveness of these tools heavily depends on the stability and volume of the incoming traffic source. Notably, GoReplay provided traffic recording capabilities that became invaluable for our process.


## The Issue with Testing against Real Traffic


Ideally, we would like to have the incoming traffic at a consistent and decent rate that the system load is enough when we measure the performance metrics, and not too much that it overloads the system.


Before this tool was developed, we had issues with inconsistent incoming traffic rate from time to time, demonstrated by the graph below. The incoming traffic rate was not consistently steady enough. On top of that, in this case, it created too much load on a server (while at times it created too little load on a server), and when the servers were overloaded, we were not able to effectively measure and compare the performance differences between the production and feature branches.


## Recording Live Traffic


First, we regroup and define our requirements, understand the scope and what we set out to achieve. Here are our target goals:


- Specific Duration Recording: It’s important we have the ability to capture traffic over a specified time frame to mirror production scenarios because the traffic patterns vary at different times in different regions of the world.
- Guaranteed QPS: To accurately compare performance of two different JVM, we must ensure a consistent queries per second rate to two different machines.
- Customizable Headers: Allowing the insertion of HTTP headers to guide traffic flows because the traffic requests are wired differently inside of Taboola service, in which case we can effectively control the features being tested.
- Traffic Isolation: Focus on specific types of traffic as needed.
- Traffic Distribution: Disperse identical traffic streams across multiple targets for parallel testing. By doing so, our comparison metrics between two different JVM instances can be meaningful.


## GoReplay: The Backbone of Our Traffic Testing Tool


At Taboola, we have used both the Nginx rating limiting module and GoReplay for some time now. While both tools are quite amazing at what they do, there are still some parts of the features we wish them to have naturally. Based on our experience with both and the requirements defined early. GoReplay stands out as an essential tool for capturing and replaying traffic, offering features such as:


- Rate-Controlled Recording: Capture traffic without overwhelming the systems. However, GoReplay’s recording capability is limited to the volume of incoming traffic as naturally it can only record what it receives.
- Multi-Host Playback: Replaying the same traffic to multiple hosts to facilitate comprehensive testing is an essential feature of GoReplay.
- QPS Management: Without consistent source traffic QPS, achieving a steady final output is challenging despite GoReplay’s rate control options.


## Customizing GoReplay for Our Needs


To meet our business requirements, we decide to move forward with GoReplay and implemented custom enhancements on top of it:


- Timestamp Rewriting: Modify traffic timestamps in GoReplay’s output files to simulate a fixed 100 QPS rate, regardless of actual recorded rates.
- Percentage-Based QPS Control: With a stable 100 QPS source, control throughput by adjusting playback percentages with ease (e.g., 50% for 50 QPS).


## The Result


After the tool was implemented, we were able to control the right amount of traffic, consistently at 75 QPS, which produced sufficient system load, allowing us to measure the meaningful impact of both branches before releasing the new code to our production environment.


In addition, by utilizing the recorded traffic, we were also able to reduce the number of servers used in the CI pipelines from three to two when, in the previous setup with Nginx rate limiting module, it required one additional machine to forward requests to the other two servers.


## Exploring Additional Benefits


The application of this traffic replicating approach extends beyond just our benchmarking of new features into the production environment. It can also serve in other areas of our infrastructure:


- System Warm-Up: Emulate real-world traffic to effectively prepare our systems, particularly the JVM, for consistent performance.
- Load Testing: With this setup, we can easily produce load to our system with real traffic.
- Traffic for Local Environments: When Taboola developers write and test code locally, this approach can assist by sending real traffic to their local environments.


## Looking Forward: Future Enhancements


As we refine our methodologies, future enhancements can include:


- Enhanced Traffic Filtering: Fine-tune data captured to focus on specific publisher traffic.
- Time-Based Variation: Use traffic samples from various times of the day in different time zones to simulate different user behaviors and loads.


By adopting these advanced traffic recording and replay techniques, Taboola ensures robust testing environments that closely replicate real-world conditions, fostering more reliable CI pipelines, which contribute to our more stable production environments.
