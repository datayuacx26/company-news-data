---
schema_version: "1.0.0"
document_id: "a9acefc6223744a7e1834b0d182bc6031619cf73f52fcd6239f58679548378df"
company_key: "logitech-international-s-a-ordinary-shares"
company: "Logitech International S.A."
source_id: "logitech-international-s-a-ordinary-shares-rss-9883ecf3a078"
canonical_url: "https://www.logitech.com/blog/2026/03/03/how-i-created-a-smart-home-solution-using-the-mx-creative-console/"
published_at: "2026-03-04T02:00:57+00:00"
first_seen_at: "2026-07-20T03:30:07.888502+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:034708ab992f49d6ffb159461a649874ef44df7c4d770c394b86e67ffdbeae99"
---

# How I Created a Smart Home Solution Using the MX Creative Console

As I began my engineering internship at Logitech, I quickly sought to combine innovation, community, and the growing world of smart home technologies. From the very beginning, my focus was clear: explore how Logitech peripherals, like the MX Creative Console, could seamlessly connect with smart home environments to make everyday tasks faster and easier. I looked into


[Home Assistant](https://www.home-assistant.io/) , an open-source smart home project that describes itself as “open source home automation that puts local control and privacy first” to explore how Logitech’s MX Creative Console could be adapted to control smart home lights.


**The Smart Home Gap**


As someone passionate about smart home technologies, I saw an opportunity to fill a unique gap. While smart homes have grown increasingly popular, there’s still room for improvement in how users interact with them. Imagine sitting at your desk in your work-from-home setup. Your smart lights are on, but you want to adjust them to focus, dim them for relaxation, or toggle appliances. You could use a web interface, but that means opening a browser and disrupting your workflow. The Creative Console felt like a natural alternative. The console is an intuitive tactile control surface for fine-tuned adjustments, and using it is simpler and faster than browser-based interfaces.


**Why Home Assistant Powers This Integration**


After considering multiple options, the choice of Home Assistant as the hub made the most sense. Home Assistant is open source, highly customizable, and backed by a community of tech-savvy users who value privacy and control. It’s the kind of platform that resonates deeply with people who care not just about functionality but also about freedom and adaptability.


Home Assistant’s modularity allowed me to explore how Logitech’s MX Creative Console could be adapted to seamlessly control smart home devices such as lights. With the infrastructure provided by Logitech’s Logi Actions Software Development Kit (SDK), I developed a plugin that integrates functionality directly into the Options+ ecosystem. For my project, the SDK served as the bridge between the MX Creative Console and the smart home plugin login. Instead of building a control interface from scratch, the SDK handled key elements such as reading inputs from the console, managing actions, and integrating with the Options+ ecosystem. This allowed me to focus on designing the smart home behaviors themselves. It also ensured that the plugin would feel native to the Logitech experience, offering users a polished and consistent interface.


**The Solution: An Options+ Plugin**


This Home Assistant plugin focuses on controlling smart lights, and its potential extends far beyond lighting in the near future. For now, here’s how it works:


- Customizable Smart Actions: The plugin offers configurable actions that allow users to personalize their environment. For example, you can assign an action to the smart ring or creative console tiles that toggles the lights in a specific area, changes their brightness, or sets custom colors.


- User-Friendly Integration: Once the plugin is connected to Home Assistant, users can tailor these settings to their liking, making adjustments with just a tap or spin of the creative console.


- Expanding Possibilities: This is just the beginning. Future iterations of the plugin will support more smart appliances, such as switches, buttons, or blinds/covers.


- Through intuitive design, tech-savvy solutions, and robust customization, this project demonstrates how Logitech peripherals can amplify the smart home experience.


**Logitech and Open-Source: Strengthening Community Connections**


We were proud to launch the plugin as open-source under the MIT License. Open-source tools empower users to tweak, expand, and personalize the technology to meet their needs, fostering collaboration and innovation.


The Logi Actions SDK is designed to be open and invites developers to build plugins like mine. By making this plugin open-source, we hope to inspire open-source developers to explore ways to integrate products into their homes and workflows.


For anyone interested in exploring the beta plugin in detail, the full project page, including documentation, installation instructions, and source code, is available on


[GitHub](https://github.com/Logitech/cto-HomeAssistantPlugin-OptionsPlus)


**What’s Next?**


While my personal project focused on smart lights, I am next looking at adding support for smart switches and covers , enabling even greater versatility. As a community designer, I’m excited about the possibilities that lie ahead. From integrating additional smart devices, such as thermostats and door locks, to enabling more sophisticated multi‑device configurations, there is still so much untapped potential in connecting peripherals with smart home environments. I’m optimistic about what the Creative Console can achieve for both my personal use and users around the world.


**A How-To Guide along with the beta plugin page can be found here \[**[link](https://github.com/Logitech/cto-HomeAssistantPlugin-OptionsPlus#:~:text=Power%2Duser%20Notes-,Quick%20Start,the%20Configure%20Home%20Assistant%20action%20from%20your%20layout%20.%20The%20settings%20persist) **\]**


**Our legal department made us say this next part: this software is not distributed by Logitech, it is a personal project by Cristian Safta and Logitech makes no representations or warranties regarding the project.**
