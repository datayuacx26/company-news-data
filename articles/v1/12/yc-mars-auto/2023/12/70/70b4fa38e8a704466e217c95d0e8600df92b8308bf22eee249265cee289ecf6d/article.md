---
schema_version: "1.0.0"
document_id: "70b4fa38e8a704466e217c95d0e8600df92b8308bf22eee249265cee289ecf6d"
company_key: "yc-mars-auto"
company: "Mars Auto"
source_id: "yc-mars-auto-rss-2183801a7a92"
canonical_url: "https://blog.marsauto.com/high-speed-self-driving-truck-is-hard-38b1d4162e86"
published_at: "2023-12-27T15:07:04+00:00"
first_seen_at: "2026-07-25T13:21:51.802210+00:00"
fetched_at: "2026-07-28T22:26:20.635269+00:00"
content_hash: "sha256:5fe4c694f8be15a9a305bfbecbb4abffce36ebe1da9cf004310f182e094aa9cf"
---

# High-speed self-driving truck is hard

In


the self-driving space, there are frequent debates about what is the right approach to self-driving. Whether it’s the usage of *lidar vs camera* , the necessity of *hd-map vs no hd-map* , or even a more recent discourse on the[reliability and safety](https://www.mobileye.com/opinion/are-we-on-the-edge-of-a-chat-gpt-moment-for-autonomous-driving/) of the *end-to-end learning* approach. Yet, another more fundamental challenge is frequently overlooked — *vehicle dynamics* . However, what exactly is[vehicle dynamics](https://en.wikipedia.org/wiki/Vehicle_dynamics) ? Vehicle dynamics in simple terms is about how a vehicle responds to various actions like steering, acceleration, and braking, under different conditions. These conditions stem from external factors like road conditions and cross-wind, or vehicle internal factors such as the vehicle’s suspension system, tire characteristics, and drivetrain system.


Why wasn’t the importance of it emphasized in the self-driving space? While vehicle dynamics is a well-studied area, it plays a very important role in driving performance and safety. The primary explanation is that the majority of self-driving companies focus on the urban driving environment, where the speed is limited (e.g., maximum speed limit of[30mph](https://www.dmv.ca.gov/portal/news-and-media/117199-2/) in San Francisco) due to safety considerations. Additionally, these self-driving cars are characterized by their small size and weight compared to freight shipping trucks. In the context of a lightweight and low-speed vehicle, the tire slips are less pronounced during cornering and heavy braking. Whereas, for a heavily loaded and high-speed truck, they become very critical for both safety and overall driving performance.


## Get Cong Jie’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


In Mars Auto, however, our focus is long-haul freight shipping, where a fully loaded truck can reach weights of up to 30 tons. The accuracy in vehicle dynamics modeling and estimation becomes more prominent. Although one could consider lowering the driving speed, in the freight shipping industry keeping up with the shipping schedule is crucial. Consequently, our system must prioritize both safety and timely delivery which requires operating at high speed (within the legal speed limits).


### Vehicle dynamics external factors


The first aspect that affects vehicle dynamics is the external factors. In the real-world driving environment, we have to deal with road conditions and geometries as well as ambient conditions.


Steering control becomes unstable after hitting potholes when part of the vehicle state estimation is disabled.


- When encountering **potholes** at high speeds, inaccuracies in vehicle dynamics modeling and vehicle state estimation can result in instability in the vehicle’s lateral control (steering), and potentially lead to a loss of control which can be catastrophic.
- As trucks have a **high profile** and a significantly **larger side surface area** , self-driving trucks can experience substantial impacts on lateral control stability from cross-wind gusts, influencing their ability to remain within the lane.
- Similarly, inside a **tunnel** , the turbulent airflow due to other vehicles as well as the air pressure difference can induce uneven aerodynamic force on the side surfaces of the truck, which can also affect the lateral control stability.


Press enter or click to view image in full size


Air flows around a semitrailer with a 30-degree wind angle. Image credit:[Fundamentals of Vehicle Dynamics](https://www.sae.org/publications/books/content/r-506/)


- For safety considerations on highways, the road is constructed with a[cross slope](https://en.wikipedia.org/wiki/Cross_slope) or **bank angle** . Specifically, on **curves** , the road is designed to be banked in alignment with the road curvature. This design enhances the lateral traction of the tires during cornering, ensuring the vehicle can safely navigate the curve.


Our self-driving truck is being tested on a test track with a superelevated curve.


- It’s also very common that even a **straight road** is designed to be **banked** for effective rainwater[drainage](https://en.wikipedia.org/wiki/Drainage_gradient) , mitigating the potential risk of aquaplaning caused by water accumulation. Ignoring the bank angle in road geometry within vehicle dynamics modeling can also introduce bias in driving behavior, where a truck cannot maintain its position at the road center or on the desired trajectory.
- In addition to the road bank, the **road slope** should also be taken into account. It has a direct impact on the acceleration and braking capabilities of the vehicle. In a downhill, more aggressive braking is required to maintain a safe distance from a lead vehicle, and less aggressive braking is required on an uphill due to gravitational force. Likewise, the amount of acceleration has to be adjusted accordingly based on the road slope.


Our 20-ton self-driving truck is performing hard braking on a downhill due to sudden heavy traffic and cut-in.


### Vehicle dynamics internal factors


The list goes on and it’s just part of the stories. In Mars Auto, we have to also support trucks from different brands and models as well as trucks of different sizes. The truck itself contributes to several internal factors that can also impact the driving performance.


- Trucks with different **numbers of axles** and **suspension systems** (e.g., air ride or leaf spring as well as their stiffness) exhibit different characteristics during cornering. For instance, a heavily loaded truck with a softer suspension can experience more outward weight shift, leading to increased roll during the cornering. It takes a longer time for the center of gravity to recover. Without accurate vehicle lateral state modeling and estimation in the self-driving system, a temporal oscillation can occur when the truck is transitioned back to straight-road driving after cornering.


Press enter or click to view image in full size


Vehicle basic scheme including roll motion **Φ** of two different suspension layouts (left to right): swing arm, double wishbone. Image credit:[The Science of Vehicle Dynamics](https://link.springer.com/book/10.1007/978-94-017-8533-4)


- The **power steering system** of each truck is also different. Most heavy-duty trucks are equipped with hydraulic power steering. The assistive torque provided by the power steering depends on several factors, most commonly the vehicle speed and engine load. For example, for a speed-sensitive power steering system as a vehicle is driving faster, less assistive torque is provided by the power steering system. Our self-driving system needs to be able to adjust the torque accordingly to steer the truck. On top of that, the payload weight needs to be taken into account too.
- The **braking** is another very complex system. The braking performance depends mainly on the temperature. It’s not just about the ambient temperature, but also the usage of the brake. In the scenario where aggressive and prolonged braking is applied especially on a downhill, the brake temperature will be elevated which causes the brake to have reduced effectiveness. As the weight of the truck increases, more heat will be generated during the braking. The mapping between the brake pedal and the desired braking force is not fixed and it varies depending on the temperature and time, which makes the brake modeling and mapping to be not trivial.


Press enter or click to view image in full size


Brake torque measured during a stop. Image credit:[Fundamentals of Vehicle Dynamics](https://www.sae.org/publications/books/content/r-506/)


- As **time progresses** , the steering wheel may lose alignment, the brake pad may slowly wear down, and the left and right side air suspension imbalance may occur. Our system needs to adaptively accommodate these changes over time without manual recalibration periodically.
- Lastly, the payload. The **payload weight** can affect the required steering torque for lateral control, it can also affect the required braking force and acceleration force for longitudinal control. Moreover, the payload itself can be **unevenly distributed** in the truck’s cargo compartment, resulting in a varying center of gravity for each drive — leaning towards the left, right, forward, or backward. This leads to a different vertical load on each tire and suspension. Without proper vehicle dynamics modeling and estimation, issues such as **lane centering bias** and **oscillation** can arise.


### The solution?


There are of course many more things to consider, and addressing each of them to support each truck poses a significant challenge. While we can manually calibrate each component and hand-tune some parameters periodically (say once a week). We could also set a strict rule for payload loading to ensure that the vertical force on each tire is well distributed and requires the operator to key in the payload weight into the self-driving system. This approach works fine for a small number of fleets where the vehicle type is homogeneous. However, when managing a large number of heterogeneous fleets, the solution is not scalable and it’s operational labor intensive.


### The scalability approach is the answer


The first principle of our engineering approach is scalability and automation. Although the self-driving technology itself is automation, most self-driving companies failed to be able to scale up the number of fleets in a very cost-effective way. With a much higher operational cost compared to human drivers, it renders the self-driving technology to be of no advantage. Some companies burn through close to[a billion dollars](https://techcrunch.com/2023/11/29/gm-to-slash-spending-at-cruise-by-hundreds-of-millions-of-dollars/) in just a single quarter. Without the cost-effective scalability in mind, the company is bound to fail.


To build a cost-effective scalable self-driving system, all the resource-intensive components (operational in this case) need to be removed from the equation. We focus our engineering effort on automating all the aforementioned works without the need for any manual calibrations. To deal with a fleet of heterogeneous trucks, we further modeled the truck vehicle dynamics to handle trucks of different mechanical systems and sizes. With a carefully designed hybrid machine learning sensor fusion algorithm, our system can reliably estimate the payload weight (without the weight sensor, or the tire pressure measurements), external disturbances such as the road bank, and road slope, as well as the truck's internal factors such as the steering wheel offset, steering wheel delay, etc on the fly during the driving.


### In conclusion


In this post, we discussed the challenges we encountered for a long-haul freight shipping self-driving truck from the perspective of vehicle dynamics. The self-driving technology is not just about perception and mapping, there are many more interesting open challenges to be tackled. In the future, we will dive into more challenges we encountered and elaborate on our engineering approaches. Please j[oin us](http://marsauto.com/careers) on our journey to win the self-driving!
