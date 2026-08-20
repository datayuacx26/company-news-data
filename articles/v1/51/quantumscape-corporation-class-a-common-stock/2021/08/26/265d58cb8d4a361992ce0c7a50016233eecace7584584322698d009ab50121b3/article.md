---
schema_version: "1.0.0"
document_id: "265d58cb8d4a361992ce0c7a50016233eecace7584584322698d009ab50121b3"
company_key: "quantumscape-corporation-class-a-common-stock"
company: "QuantumScape Corporation"
source_id: "quantumscape-corporation-class-a-common-stock-news-import-5a14dde55f0f"
canonical_url: "https://www.quantumscape.com/resources/blog/temperature-in-battery-development/"
published_at: "2021-08-27T21:22:15+00:00"
first_seen_at: "2026-07-24T13:14:48.701142+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:96417b1b8d05cccc6ce85b8ebe7333626a296196d9543474cb284d884781df77"
---

# Temperature in Battery Development

### High-temperature effects


If heating the battery reduces resistance, shouldn’t hotter temperatures improve battery performance? Not so fast. There are also negative side effects to high temperatures. For example, the liquid electrolyte is highly reactive with the materials that make up the cathode and anode. These reactions increase at higher temperatures and consume lithium, reducing the overall available energy in the battery. They also fill up the surfaces of the electrodes with junk[\[5\]](https://dev-quantumscape.pantheonsite.io/?p=10486&elementor-preview=10486&ver=1635193439#footnotes) , which increases the resistance of the battery – so instead of skateboarding down a smooth, freshly paved street, it’s more like going down a gravel road. As temperature rises, the number of these reactions increases dramatically, producing even more junk and shortening the life of the battery.


This junk buildup is not the only problem that occurs at high temperatures. Both the liquid electrolyte and polymer separator in a legacy lithium-ion battery are extremely flammable. If the temperature passes a certain point, the battery will go into thermal runaway, which can lead to self-ignition and explosion. Thermal runaway is one of the primary safety risks of modern EVs and the reason they must have more complex, bulkier and more expensive thermal management systems.


### Striking a balance


Because of these challenges, legacy lithium-ion batteries must strike a balance. If the battery is too cold, resistance is high and energy is low, and the electrolyte could even freeze and stop the battery completely. And if fast-charged in the cold, the battery can fail due to lithium dendrites. And yet, if the battery runs too hot, the electrodes fill up with junk and the battery permanently loses its capacity[\[6\]](https://dev-quantumscape.pantheonsite.io/?p=10486&elementor-preview=10486&ver=1635193439#footnotes) . As such, there is really only a narrow temperature window where a legacy lithium-ion battery can operate effectively. This constrains their real-world usefulness.


However, many of these constraints spring from problems caused by the liquid electrolyte and the graphite anode. If battery scientists could eliminate the liquid and graphite, the issues of chemical junk at high temperatures and lithium plating at low temperatures could be minimized or even completely resolved. The ideal would be an anode made of pure lithium (i.e., a lithium-metal anode) coupled with a stable solid electrolyte. This is where solid-state batteries come in.


## Alternative Solid-State Approaches


### The Resistance Problem


The main problem with switching to a solid electrolyte is that solids offer a lot more resistance than liquids, just as it’s easier to swim through water than ice. A common workaround for this problem is heating the battery cell because many solid electrolytes have a relatively acceptable level of resistance at high temperatures. It’s common to see alternative solid-state technologies tested at 60 °C (140 °F) or more. At such high temperatures, lithium atoms slide very easily through the solid electrolyte, even at high rates of power such as 1C[\[7\]](https://dev-quantumscape.pantheonsite.io/?p=10486&elementor-preview=10486&ver=1635193439#footnotes) .


However, as the temperature is decreased to more realistic levels for automotive applications (25-30 °C or 77-86 °F), the resistance tends to increase dramatically, even at very low rates of power (C/10[\[8\]](https://dev-quantumscape.pantheonsite.io/?p=10486&elementor-preview=10486&ver=1635193439#footnotes) ). As power demands increase, these alternative solid electrolytes don’t allow the lithium ions to pass smoothly, and available energy falls off steeply. At power rates of 1C, it’s not uncommon to see less than 20% of the energy accessible at room temperature, rendering the battery essentially useless for EVs. A battery like this is like a skateboard stuck on a gravel road, unable to deliver more than a tiny amount of power.


The key point to remember is that resistance in a solid-state battery can be reduced, and hence performance increased, by making the battery very hot. For instance, some electric buses use solid-state batteries operating at 80 °C (176 °F). But this won’t work for passenger vehicles because it requires large, heavy, and expensive thermal management systems to keep the battery at high temperature and bring resistance down to operational levels.[\[9\]](https://dev-quantumscape.pantheonsite.io/?p=10486&elementor-preview=10486&ver=1635193439#footnotes)


And even if the resistance of the solid-state separator is acceptable, if it can’t prevent dendrite formation, it will still have to be operated at high temperatures at which the lithium softens and becomes less likely to grow dendrites. In the case of unstable solid electrolytes like[the sulfides](https://www.quantumscape.com/blog/the-problem-with-sulfides/) , this will accelerate the decomposition of the electrolyte at the interfaces with the electrodes and shorten the battery’s lifespan, just like in a conventional liquid-electrolyte battery.


### The QuantumScape Cell


The first difference between other solid-state batteries and the QuantumScape cell is our ceramic solid-electrolyte separator. We have[published data](https://www.quantumscape.com/blog/a-discussion-of-quantumscapes-battery-technology-performance-results/) showing our separator can prevent dendrites from forming under practical operating conditions and temperatures. The separator can also be made very thin, which means resistance is very low, not only at room temperature but also much colder. Additionally, the ceramic doesn’t react with lithium in the way liquids or sulfides do. This means the anode doesn’t fill up with junk and efficiency remains very high, unlike many alternative technologies.


The second advantage of our technology is the catholyte – an organic liquid and polymer combination that helps lithium ions move smoothly from the cathode into the solid electrolyte separator. However, since our ceramic solid-electrolyte separator chemically isolates the cathode from the anode, there is minimal risk that the catholyte will react with the lithium metal on the anode, unlike other lithium-metal approaches that use a liquid electrolyte instead of a solid.


In addition, unlike liquid electrolytes in legacy lithium-ion batteries that must be stable in both the high-voltage cathode and the low-voltage anode, the QuantumScape cell is designed to prevent the catholyte from coming into contact with the anode entirely. So instead of needing to be optimized for stability under a range of voltages, the catholyte can be optimized for conductivity at lower temperatures, helping minimize resistance in the cold. This is why QuantumScape’s cells perform well in low-temperature tests.
