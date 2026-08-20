---
schema_version: "1.0.0"
document_id: "1fabb26a017b17b8f376bdfbf7fb3e459781454c8f59718993ce9e01d3180085"
company_key: "hagerty-inc-class-a-common-stock"
company: "Hagerty Inc."
source_id: "hagerty-inc-class-a-common-stock-rss-699fc274d6c4"
canonical_url: "https://www.hagerty.com/media/opinion/the-hack-mechanic/a-100-year-old-trick-passing-inspection-with-incomplete-obdii-monitors/"
published_at: "2026-08-10T13:00:00+00:00"
first_seen_at: "2026-08-10T14:00:07.339719+00:00"
fetched_at: "2026-08-20T00:49:29.601613+00:00"
content_hash: "sha256:d676030ae348875d51fc919b6815658122c895a59cbc93d8af74903cb9323e57"
---

# A 100-Year-Old Trick: Passing Inspection with Incomplete OBDII Monitors

After my mammoth piece last week proclaiming that the ‘69 Lotus Elan +2 is now a fully functional, air-conditioned car, it spat in my face, laughed “Who are you calling fully functional, you daft wanker?” and shredded another toothed Gilmer-drive belt after only 40 miles. As they say, Lots Of Trouble, Usually Serious. Dude, you’re killing me.


So today let’s talk about something that’s actually solvable.


As I’ve mentioned before, my next-door neighbor Jeanette is a remarkably spry woman for her years. Last month, she celebrated her 100th birthday at a party. And she’s still driving her 2012 Honda Accord. Not far, mind you—just to the grocery store and to get her hair done. The car has such low mileage that I’d want dibs on her selling it to me when she decides to hang up her license, but she says it’s going to go to her grand-niece.


It’s a literal only-driven-by-a-little-old-lady 2012 Accord.


Rob Siegel


But because the car isn’t driven often, the battery slowly runs down.[Three years ago I checked for a parasitic drain, didn’t find one](https://www.hagerty.com/media/opinion/the-hack-mechanic/how-to-drain-a-car-battery-starring-my-95-year-old-neighbors-daily-driver/) , and replaced it with a new Interstate AGM battery which seems to be stouter than the last one, but I still prophylactically charge it every few months just to keep ahead of it. (If the cigarette lighter was active without the key turned, I’d install a small solar charger on the back deck, but it’s not, and I really don’t want to do anything with cables that she might trip on.)


In addition to that, her lack of driving affects things inspection-wise as well. Years ago, Massachusetts did away with its tailpipe testing (a direct measurement of unburned hydrocarbons, carbon monoxide, and nitrogen oxides). Instead, on cars that are 15 years old and newer, they rely on the standard OBDII system that every car sold in the United States since 1996 has. If the Check Engine Light (CEL) is on, it’s an automatic fail.


But even if the light is out, you’re not out of the woods yet. While the new test essentially *does* say, “Well, if the OBDII system says everything’s fine, we’re not going to second-guess it,” they *do* plug their inspection computer into the OBDII port and check for fault codes. So if you’ve been crafty and done something like, oh, say, removing the light itself, “shooting the messenger” isn’t going to work.


But even if there are no codes, there’s still something that can trip you up, and that’s the OBDII “readiness monitors.” There are about a dozen of these—three that run continuously (misfire, fuel ratio, and “comprehensive component” that checks sensors and actuators), and up to ten non-continuous monitors that only run under specific conditions like cold start or highway driving.


If you have an OBDII code reader or a scan tool, you can clear the codes that are causing the CEL to be on, but doing so resets all the readiness monitors to “Not Ready” (as does disconnecting the battery). If the fault was caused by something that’s monitored continuously and you’ve cleared the code without actually having done anything to fix it, the light will likely come right back on. But even if there’s nothing left that’s wrong, you still need to drive the car enough that the monitors announce that they’re “ready” for the car to pass inspection.


And how far is that?


This is where you get into the arcane topic of “drive cycles.”[In this piece](https://www.hagerty.com/media/magazine-features/fifty-years-ago-the-government-decided-to-clean-up-car-exhaust-its-still-at-it/) , Hagerty writer Aaron Robinson dove deeply into the history of automotive emission controls (we love to hate them, but American cities were choked with smog, and it was a public health hazard). As part of the standardization of testing, Los Angeles officials came up with standard drive cycles that mimicked an average commute. These were then adopted as part of federal emissions standards, and the drive cycle concept is still baked into OBDII. You can find more details on it[here](https://www.obdautodoctor.com/tutorials/obd-readiness-monitors-explained/) , but the standard drive cycle is a combination of highway and city driving.


However, Jeanette doesn’t drive on the highway. Ergo, years back when she took the car in for inspection, it failed because some of the monitors weren’t ready. They told her to drive it more on the highway. She said, “But I don’t *like* driving on the highway,” and I started doing it for her and then getting the car inspected.


All of this went flawlessly the past two years. I’d take the car onto I-95, go a few exits north, turn around, then head for the inspection station. Only this year, I had a brain fart, forgot, and headed straight to get a sticker. When the inspection technician drove it out of the bay, I saw the big “R” on the sticker and asked what the issue was.


“Emissions,” he tersely said.


“Can you be more specific?” I asked.


“It wasn’t ready,” he said. I understood.


“Don’t just drive it on the highway,” he advised. “It may need more than just that. Drive it around for a few days, then bring it back for your (one) free retest.”


I was thinking, “This morning is my window of opportunity to get it done, I’m not going to let this stretch into days,” but I didn’t say that out loud.


Then I remembered: I don’t need to need to read up on drive cycles and drive the car X miles on the highway at speed Y, and then C miles in stop-and-go at speed D. I just need to plug in a scan tool, look at the readiness monitors, and see when they clear.


I don’t have a super whiz bang professional-level scan tool, but I do have a Foxwell NT510. For the most part, a $30-ish tool is just an OBDII code-reader—it reads the standardized OBDII information but does nothing marque-specific, while a $120-ish tool generally has a set of marque-specific capabilities (my NT510 is configured for BMW and Mini). Thinking correctly that the readiness indicators were part of standard OBDII, I grabbed the Foxwell and plugged it into the Accord, and sure enough, it listed Monitors Not Applicable (N/A), Monitors OK, and Incomplete Monitors (INC). It said there were three of the latter, though there didn’t seem to be any way to drill in and see which ones they were.


It’s the INC monitors that are important at inspection time.


Rob Siegel


I left the scan tool plugged into the OBDII port and began driving the Accord. The number of INC monitors quickly dropped to two. I got on the Interstate, and after two exits, it dropped to one. But then it stayed there. Wanting to get this done, I drove about 40 miles, stopping at rest areas, shutting the car off, restarting it, and continuing. The INC count didn’t budge. I headed south on another highway and got off on a local road to intentionally do the last 15 miles in stop-and-go. By the time I was near the inspection station, the INC count sat stubbornly at one.


Hmmmmn.


I began wondering what to tell Jeanette if I couldn’t get the car to pass. The rational answer was “Ignore it.” The rejection sticker gives you 60 days. After that, the odds that a police officer is going to give a 100-year-old woman a ticket for a rejection sticker if she tearfully tells the truth and says that she’s afraid to take the car onto the highway seem pretty small. And if you’re still driving it three months later, it’ll be 2027 and the car will be 16 years old and will have aged out of the testing.


But getting it inspected so she wouldn’t feel like a criminal would be much better.


Then I had a thought—I asked my phone, “How many incomplete monitors are allowed in Massachusetts inspection?” It said “2012 and newer model year non-diesel vehicles may have a maximum of one ‘not ready’ non-continuous monitor.” It also reported that the EVAP monitor is typically the one that takes the longest to complete.


Well then!


I lined the car up in front of the inspection bay. The tech gave me the hairy eyeball, as it seemed that I was explicitly ignoring his advice. I held up both the scan tool and my phone and said, “My phone says that I’m allowed one incomplete monitor, and my scan tool says that one is what I have, so it should pass.” He gave me a little head bob that indicated a degree of respect. When it passed, I thanked him profusely, explained that I wasn’t ignoring his advice, but that this was my window of opportunity to do this favor for my century-old neighbor.


He stared at me, then a look of understanding came over his face. “So *that’s* why the mileage is so low!” he said.


So, if you live in one of the 27 states that have motor vehicle inspections that require emissions testing, and if they include an OBDII plug test, do a search and see how many incomplete monitors they allow. Although Massachusetts doesn’t check when the car is older than 15, a Google search says that EPA guidelines generally allow for two incomplete monitors for 1996-2000 vehicles, and one for vehicles 2001 or newer.


But back away from Jeanette’s Accord. I have dibs. After her grand-niece :^)


Like I said, driven only by a little old lady to get her hair done.


Rob Siegel


***


Rob’s newly expanded book, *The Best of The Hack Mechanic: 40 years of hacks, kluges, and assorted automotive mayhem* , is available on Amazon[here](https://www.amazon.com/dp/0998950750) . His other seven books are available[here](https://www.amazon.com/Rob-Siegel/e/B00E6EXQ6Q) , or you can order personally inscribed copies from Rob’s website, www.robsiegel.com.


The post[A 100-Year-Old Trick: Passing Inspection with Incomplete OBDII Monitors](https://www.hagerty.com/media/opinion/the-hack-mechanic/a-100-year-old-trick-passing-inspection-with-incomplete-obdii-monitors/) appeared first on[Hagerty Media](https://www.hagerty.com/media) .
