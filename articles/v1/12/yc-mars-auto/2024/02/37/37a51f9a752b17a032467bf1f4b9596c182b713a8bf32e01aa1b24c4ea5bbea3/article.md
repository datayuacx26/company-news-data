---
schema_version: "1.0.0"
document_id: "37a51f9a752b17a032467bf1f4b9596c182b713a8bf32e01aa1b24c4ea5bbea3"
company_key: "yc-mars-auto"
company: "Mars Auto"
source_id: "yc-mars-auto-rss-2183801a7a92"
canonical_url: "https://blog.marsauto.com/superhuman-fuel-efficient-self-driving-ce937e433470"
published_at: "2024-02-03T10:19:55+00:00"
first_seen_at: "2026-07-25T13:21:51.802210+00:00"
fetched_at: "2026-07-28T22:26:16.511012+00:00"
content_hash: "sha256:51396132679f77a8b4dd8c02459100dc88ee8bf159d99e5dcd8522ad2ea70cf1"
---

# Superhuman fuel-efficient self-driving

# Superhuman fuel-efficient self-driving


[Cong Jie](https://medium.com/@ncjlee78?source=post_page---byline--ce937e433470---------------------------------------)


8 min read


·


Feb 3, 2024


--


Press enter or click to view image in full size


In


our previous[article](https://blog.marsauto.com/high-speed-self-driving-truck-is-hard-38b1d4162e86) , we explored the challenges encountered by high-speed self-driving trucks, where the vehicle dynamics are frequently overlooked. In this article, another aspect that is often neglected in self-driving is fuel efficiency. It’s crucial to recognize that the capability to drive autonomously does not automatically equate to efficient driving. Surprisingly, majority of the self-driving companies do not focus and emphasize on fuel efficiency. In the freight shipping industry, it has a very significant importance. Notably, fuel expenses constitute the second largest cost in the trucking industry, accounting for up to 28% of the total average marginal cost in the trucking operational cost (according to[ATRI 2023 data](https://truckingresearch.org/wp-content/uploads/2023/06/ATRI-Operational-Cost-of-Trucking-06-2023.pdf) ). Improving fuel efficiency involves various strategies, including non-driving related strategy by over-inflating tires to reduce rolling resistance and implementing structural modifications for enhanced aerodynamics to reduce air resistance; Cooperative driving strategies, like[platooning](https://en.wikipedia.org/wiki/Platoon_(automobile)) , where several trucks align closely to minimize air drags; As well as the non-cooperative driving strategies, which will be explored further in this article.


Individual human drivers exhibit different driving habits, and the presence of poor driving practices can result in a significant reduction in fuel efficiency, with a potential decrease of up to[30% on highways](https://fueleconomy.gov/feg/driveHabits.jsp) . This variability places operational costs dependent on individual human drivers. To gain insights into how humans approach fuel-efficient driving, we conducted experiments where human drivers would do their best to achieve their optimal fuel efficiency without any time constraints. Our finding revealed a prevalent strategy among human drivers, characterized by maintaining low engine RPM and avoiding aggressive acceleration. While this approach led to slow-speed driving on highways, approximately 60km/h, it resulted in a remarkable ~30% improvement in fuel efficiency compared to their normal driving at 90km/h. Despite its effectiveness, this strategy is impractical for commercial freight shipping as it’s crucial to keep up with the shipping schedule. Moreover, drivers reported experiencing **fatigue** as they had to remain highly focused on fuel economy and engine RPM management.


### Fuel efficiency with better engine utilization


Press enter or click to view image in full size


BSFC map (engine torque vs rpm) plotted with an example of a small truck maintaining the same driving speed (40hp) with different gear selections. The BSFC map credit:[EcoModder](https://ecomodder.com/forum/showthread.php/bsfc-chart-thread-post-em-if-you-got-1466-25.html#post403636)


> The x-axis is the engine rpm; The y-axis is the engine torque and throttle position; The colorful contours indicate the fuel consumption (red is the best, black is the worst); The blue lines indicate the fixed horsepower produced by the engine.


From the engine efficiency standpoint, however, it’s not the best strategy and it’s suboptimal. Efficient driving involves a more intricate approach. It requires a thoughtful combination of the engine RPM, gear selection, and throttle position for which we can resort to the engine[BSFC](https://en.wikipedia.org/wiki/Brake-specific_fuel_consumption) map ( **B** rake **S** pecific **F** uel **C** onsumption) as shown in the figure above along with its corresponding descriptions. To illustrate how this interplay contributes to fuel efficiency, let’s consider an example using a small truck¹. Assume the truck requires 40hp (blue line) to sustain a driving speed of 90km/h. The engine RPM would depend on the gear selection, a lower gear will result in a higher RPM and vice versa. Simultaneously, the throttle position is adjusted to maintain the driving speed. For this specific instance, the gear selections are plotted as stars on the BSFC map above. Notably, the **5th** gear at 2500 RPM exhibits the best fuel efficiency ( **red** zone), the **4th** gear at 3250 RPM is less efficient ( **orange** zone), and the **2nd** gear at 6000 RPM is the least efficient ( **purple** zone). Maintaining the same driving speed, opting for the 5th gear can result in saving 80% more fuel compared to the 2nd gear. This highlights the correlation between engine RPM, gear selection, and throttle position in influencing fuel consumption. For fuel-efficient driving, the goal is to operate the engine in the red zone by sustaining low engine RPM and high throttle position (by pressing the gas pedal more deeply). It’s essential to note that in manual transmissions, the driver has full control over the gear selection, while in automatic transmissions, the gear selection is usually optimized by the manufacturer to maximize engine efficiency.


### Fuel efficiency by reducing the drivetrain energy loss


Press enter or click to view image in full size


Pulse and Glide (PnG) strategy. Image credit:[Vehicle Inertia Impact on Fuel Consumption of Conventional and Hybrid Electric Vehicles Using Acceleration and Coast Driving Strategy](https://vtechworks.lib.vt.edu/items/47ac3935-53c1-4388-9e3b-036bdf247534)


In addition to better engine utilization, reducing the drivetrain energy loss can further improve the fuel economy. A widely adopted strategy, particularly among hypermilers, is known as[Pulse and Glide](https://en.wikipedia.org/wiki/Energy-efficient_driving#Pulse_and_glide) , or PnG. It involves alternately accelerating (Pulse) to a speed higher than the desired speed and coasting (Glide) in neutral gear with the engine off to a lower speed, subsequently repeating this process. To illustrate, if a target driving speed is 50km/h, rather than maintaining a constant speed at 50km/h, the driver accelerates the car to 60km/h and allows it to glide down to 40km/h, effectively averaging out to 50km/h. In real-world driving scenarios, this approach is reported to save up to 20% more fuel than driving at the same speed consistently, all while preserving the total driving time.


## Get Cong Jie’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


During the **Pulse** stage, a deeper press of the gas pedal is necessary for the engine to operate more efficiently, as discussed in the previous section. This contrasts with steady speed driving, where less throttle is applied, resulting in less efficient engine operation. The **Glide** stage involves declutching (neutral gear) the engine from the drivetrain to avoid engine friction, as well as turning off the engine entirely to reduce energy losses. To ease the process, the **Glide** stage can be executed without turning off the engine which would lead to minor energy losses due to engine idling. This strategy is the combination of better engine utilization and reducing the drivetrain energy loss. However, its benefits diminish as the desired driving speed increases because the aerodynamic drag force increases proportional to the square of the vehicle speed.


### Fuel efficiency by minimizing the braking energy loss


Excessive use of brakes can adversely impact fuel efficiency. Optimal brake control is essential to prevent the dissipation of kinetic energy into braking heat². In the dynamics of real highway driving, the road terrain may feature inclines and declines and traffic can be unpredictable. On an uphill, kinetic energy derived from fuel is converted and stored as the gravitational potential energy. Conversely, on a downhill, this stored energy is transformed back into kinetic energy. Coasting downhill in neutral gear is the most effective method to reduce the energy loss from the drivetrain. Of course, some energy loss is inevitable due to the aerodynamic drag and rolling resistance. However, coasting downhill may lead to speeds reaching the legal speed limit, it would require us to apply some braking to slow down. This situation can be avoided by ensuring that right before the downhill segment, the vehicle speed is slow enough to have enough buffer for the downhill coasting from exceeding speed limits. Also, due to traffic and unpredicted events braking is needed to keep a safe distance and to avoid the collision. Fortunately, on a highway with multiple lanes, strategic lane changes can often circumvent the need for braking by avoiding slower lead vehicles in the lane. The same principle applies to heavy traffic situations, where a good strategy is to minimize the acceleration and braking and maximize the coasting.


### How to execute the strategies optimally?


Up until now, we have explored various strategies to enhance fuel efficiency, but we have not discussed how these strategies can be optimally executed. For example, in the **PnG** strategy, what would be the optimal acceleration time for the Pulse stage? What should be the ideal speed differential between the high and low speeds for the Pulse and the Glide stages? When driving on a hilly road, what should be the appropriate driving speed just before transitioning downhill transition to avoid the need for braking? To answer that there’s no one-size-fits-all solution applicable to all scenarios. Factors such as truck mass, aerodynamic drag, tire rolling resistance, engine BSFC, road slope (grade) spanning several hundred meters ahead, and the lead vehicle's future motion trajectory all come into play. Executing these strategies optimally in a joint manner presents a significant challenge for a human driver, not to mention the total driving time should be maintained too. Achieving such optimization requires years of experience in hypermiling practices.


Press enter or click to view image in full size


The accel-brake strategies from our fuel-efficient control algorithm for hilly road driving.


To address this challenge within our self-driving system, we have developed a hybrid state estimation machine learning algorithm. This algorithm is designed to estimate the necessary vehicle and environment states mentioned earlier. Subsequently, our control algorithm optimizes the best strategy for acceleration, braking, and coasting in a given scenario. The primary objective of this optimization is to maximize fuel efficiency while maintaining the driving time. An advantageous aspect of our approach is the absence of the need to explicitly program each strategy discussed earlier; instead, the control algorithm discovers the optimal strategy. The accompanying figure above illustrates the driving strategy employed by our self-driving system on a hilly highway. Notably, it minimizes energy loss by strategically coasting in neutral on a downhill segment by taking into account the future road slope. Additionally, notice that the braking control signal is minimized while still being able to keep the driving speed within the specified limit. For hilly road scenarios, our system consistently demonstrates a fuel efficiency improvement of up to 20% compared to human drivers. In all other scenarios, it achieves a 10% enhancement in fuel efficiency, all while maintaining the shipping time.


### Final remark


One last thing that is worth mentioning is, that fuel-efficient driving is not entirely independent from the steering control performance. Poor steer control handling that introduces oscillation in driving would also result in poor fuel economy. This is because oscillations in steering control can lead to a vehicle traversing a slightly longer distance in a wavy trajectory. In addition, repetitive steering movements can contribute to higher tire rolling resistance, further impacting fuel efficiency.


Despite numerous driving strategies and considerations, training and programming our system to adopt each strategy is suboptimal. The fuel efficiency performance would be capped by the best human capabilities and cannot go beyond that. Instead, we address this challenge by creating a general-purpose fuel-efficient driving algorithm to operate optimally in all scenarios.


At Mars Auto, our approach to self-driving sets us apart from the majority of companies in the industry. Numerous captivating challenges within this domain remain unexplored, we welcome talented engineers to[join our team](http://marsauto.com/careers) to solve the self-driving!


\[1\] The BSFC map is based on a spark ignition engine whereas a truck is usually equipped with a compression ignition engine, for illustration purposes the spark ignition engine BSFC map is used here.


\[2\] Conventional braking systems, like drum brakes or disc brakes, dissipate kinetic energy into heat which is used by most heavy-duty trucks. In contrast, electric or hybrid vehicles, equipped with regenerative braking systems can recover about 50% of the kinetic energy back into the electrical energy.
