---
schema_version: "1.0.0"
document_id: "09984a0618f2bfd9479e3210c8ba1f5abdf356cead74120b2002e79c0dcf6cc7"
company_key: "recursion-pharmaceuticals-inc-class-a-common-stock"
company: "Recursion Pharmaceuticals Inc."
source_id: "recursion-pharmaceuticals-inc-class-a-common-stock-news-import-f0ced5d1ab91"
canonical_url: "https://www.recursion.com/news/improving-wet-lab-automation-with-computer-vision-a-hack-week-breakthrough"
published_at: "2024-12-04T00:00:00+00:00"
first_seen_at: "2026-07-22T11:05:14.614366+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:737dca55c4adfa546e69f9ca45e15735b1943f0eb2c0f60700ec0ccd858911b7"
---

# Improving Wet Lab Automation with Computer Vision: A Hack Week Breakthrough

Automation brings massive advances to laboratory processes, but what happens when the machine makes an error?


In the case of an echo machine, which is used in drug discovery to precisely transfer small volumes of liquid from one container to another using acoustic technology, if the robotic grippers catch too early and the plates are slightly misaligned, the liquid will miss the holes in the wells. The machine, however, is not aware of the error – and human scientists aren’t either, until they collect the data a week later and discover that 20-30% of the wells are unviable.


That’s a big loss – both in terms of data and cost of materials.


During this year’s Hack Week at Recursion, research scientist and software engineer[Matthew Viglione](https://www.linkedin.com/in/matthew-viglione-963b79a5/) decided to apply a technology he had a particular interest in – robotic vision – to catch these machine errors and rerun the tasks without human intervention.


The Hack Week team (R to L): Matthew Viglione, Moroni Chalidi, Lindsey Beecher and Tyler Browning.


‍


With two lab contractors – Moroni Chalidi and[Lindsey Beecher](https://www.linkedin.com/in/lindsey-beecher-phd/) - and two automation engineers – Viglione and[Tyler Browning](https://www.linkedin.com/in/tyler-browning-a835a4187/) – they got to work installing a simple camera with masking tape that could capture what was happening in the lab in real time and writing code that could teach the machine how to autonomously recover from missteps.


They focused on solving a couple potential issues – identifying when caps were improperly off or on vials, and detecting when cell plates were misaligned. Either one of these errors could lead to significant damage and costs, Viglione says.


They started by running a series of tests using the cameras. First, vials with all white caps, and some caps missing, to ensure that the algorithm could detect the missing caps. Then they added additional cap colors – “lab techs will often draw on the vials with Sharpies – there are a lot of things that could throw off the vision algorithm,” Viglione said. They continued to train the algorithm – from correctly identifying that caps were missing to determining which specific caps were missing.


Training the algorithm to identify missing vial caps from camera images.


‍


To solve the problem of plate misalignment, they used the attached camera to take extensive photos and videos of echo plate orientation and used those to train the algorithm. “When that alignment is off by even a millimeter, the compound will miss the wells and the cells will die or not receive enough compound,” Viglione says. They included plates with all wells full, with some wells full, with live cells, and with chemicals.


Although misalignment happens just 5% of the time, Viglione says, it has serious consequences. “When it happens, we have to throw away one third of that data because of one little bug. There are 1,500 experiments in a plate – that’s a lot of data.”


Lindsey Beecher prepares to catch a plate.


‍


Using the camera and algorithm, the machine can be prompted to instead detect the misalignment, reflip the plate, and autonomously recover.


This is the first time computer vision has been applied to improve efficiency in Recursion's automated wet lab -- it’s exciting, says Viglione.


“Hack Week appeals to my entrepreneurial and research side,” says Viglione. “Robotic vision is something I did in grad school – it requires specialized expertise, but once you have it there’s so much you can do.”


The team has already received approval to integrate their capping solution into the Recursion operating system. They are manufacturing camera cases and mounts, and installing them into the part of the RecursionOS that runs the robotics so it can autonomously detect when an error has happened and correct it – no human interaction required.


‍


*Author:*[Brita Belli](https://www.linkedin.com/in/brita-belli/) *, Senior Communications Manager at Recursion.*


‍
