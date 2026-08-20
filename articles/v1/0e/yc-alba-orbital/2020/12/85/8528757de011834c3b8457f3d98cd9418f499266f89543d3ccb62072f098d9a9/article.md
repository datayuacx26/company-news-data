---
schema_version: "1.0.0"
document_id: "8528757de011834c3b8457f3d98cd9418f499266f89543d3ccb62072f098d9a9"
company_key: "yc-alba-orbital"
company: "Alba Orbital"
source_id: "yc-alba-orbital-rss-54d1f111c1bd"
canonical_url: "https://www.albaorbital.com/new-blog/2020/11/24/building-and-flying-the-worlds-smallest-spacecraft-attitude-determination-and-control-system"
published_at: "2020-12-14T21:51:02+00:00"
first_seen_at: "2026-07-24T15:02:38.589009+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:2c9574bfe99a66d49469718cd601546d8811f2b7b6406a135ccf484bb8994942"
---

# Building and flying the world's smallest Spacecraft Attitude Determination and Control System

Alba Orbital's Unicorn-2 picosatellite featuring the world’s smallest ADCS


Attitude determination and control systems (ADCS) have been studied and implemented in Cubesat size spacecraft for around a decade. This period brought great accuracy which in return allowed for attitude sensitive payloads such as earth observation hardware. It has been a golden age for the CubeSat standard, however, it may be time for certain applications to move on. The PocketQube standard (50mmx50mmx50mm) provides the next step in evolution for a wide range of space missions. This blog post is not going too much further into the details of the standard but if you are interested you can read more[here.](https://static1.squarespace.com/static/53d7dcdce4b07a1cdbbc08a4/t/5b34c395352f5303fcec6f45/1530184648111/PocketQube+Standard+issue+1+-+Published.pdf) In addition to the basics, you can read more about software development for PocketQubes[here.](http://www.albaorbital.com/new-blog/2020/7/23/how-to-write-software-for-pocketqubes)


An ADCS is a system that allows the satellite to determine and change its attitude in orbit. As mentioned previously, the determination actuation methods have been studied rigorously for CubeSat standard and they remain mostly unchanged for the PocketQubes. The one thing that has to be always kept in mind is the necessity for optimization. Anything that might require too much processing or power has to be either turned on only when absolutely necessary or has to be replaced by more simple and less demanding hardware.


The size of the satellite is both ADCS's best friend and worst enemy. Due to the small size and weight of the spacecraft, the ADCS can embrace the minimalistic dimensions with ease while maintaining enough force for reliable control. The complications arise from the necessity to optimize every single aspect of the satellite. In this case, the biggest limitation arises from the power budget. ADCS can easily become a black hole for the energy stored by the solar panels. For this reason, ADCS is only called for when required. This strategy induces certain uncertainty but it ensures the satisfactory lifetime of the satellite while delivering results when needed.


Alba Orbital has developed the world's smallest ADCS on board the Unicorn-2 platform. This State of the art ADCS is the first ever ADCS to be integrated onto a PocketQube class satellite, developed in collaboration by Dr. Matteo Cerriotti of the University of Glasgow and Alba Orbital’s leading spacecraft engineers. Their work delivered a capable ADCS that powers multiple missions. This system is based on magnetorquers, reaction wheels, sunsensors, light dependent resistor magnetometer and gyroscope all of which enables the satellite to perform exemplary detumbling as well as accurate inertial pointing. But the road does not end there. Alba Orbital is determined to perfect the Unicorn-2 platform to empower a constellation of PocketQube satellites that will deliver accessible, high-quality data sets.


Building on Unicorn-2’s[flight heritage from 2019](http://www.albaorbital.com/new-blog/2020/4/10/unicorn-2c-noor-1b-success-on-orbit) , three more Unicorns are manifested to launch on board a SpaceX Falcon 9 as part of the sold out[‘Alba Cluster 3’](http://www.albaorbital.com/integration) mission, which is the largest PocketQube launch cluster in history to date. If you would like to know more about how Unicorn-2 can support your in-orbit mission, feel free to get in touch atcontact@albaorbital.com or visit the[Unicorn-2 webpage.](http://www.albaorbital.com/unicorn-2)


[Learn more](http://www.albaorbital.com/unicorn-2)
