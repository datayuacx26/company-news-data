---
schema_version: "1.0.0"
document_id: "707a975741e32ff0ef39d137d790459e1c6174493ee478a5ddada5b398f23a07"
company_key: "rent-the-runway-inc-class-a-common-stock"
company: "Rent the Runway Inc."
source_id: "rent-the-runway-inc-class-a-common-stock-rss-571d2b3d11b7"
canonical_url: "https://dresscode.renttherunway.com/blog/9/10/YEAR"
published_at: "2018-09-10T19:59:08+00:00"
first_seen_at: "2026-07-20T23:19:27.389009+00:00"
fetched_at: "2026-07-28T22:26:51.183990+00:00"
content_hash: "sha256:58581b56cc3c9716765bd77239f35f0af53fc571a9523402c081dfe9c527a52a"
---

# Rent the Runway Cycle Counting Redesign

At Rent the Runway, our business has been quickly growing ever since the first day we launched. For us, that means more and more physical items that we need to buy, clean, repair, store, and ship. We are having to scale not only our servers, but our actual physical space and processes as well! One of the most important jobs is to make sure that we know where our stuff is, and we employ several different ways to ensure everything is in the right place. Our technology is not just digital, but it also has far more important physical implications. A little over a year ago, we started the project of upgrading our cycle counting system to handle the hundreds of thousands of units of clothing we need to track. We had an old cycle counting tool that was showing its age and needed to be redesigned.


“Cycle counting,” also called cycle scanning, is the process of ensuring that everything is in the right place, by comparing what is *in* inventory with what is *expected* to be in inventory. Our inventory is generally garments stored on hangers, but not exclusively: we also have for-sale extras like bras, accessories, etc. The process of counting all items is done by a special team in our warehouse and tends to take place while other teams are also putting away items returned or picking for new orders.


Any system depends first on the organization of the units. Initially, Rent the Runway organized its inventory alphabetically by designer. Anyone who has organized their books alphabetically by author will know what this means: When you add a new unit, everything after it needs to be pushed out to the right. No big deal when it starts with W or a Z but a bigger deal if it starts with C. And it’s not such a huge burden when you have 100 books, but it becomes a problem when you have a giant warehouse full of half a million garments.


When our warehouse managers saw this was becoming an issue, they moved to organizing by style, and new stock got added wherever there was room. However, then you need to keep track of where your inventory is located in the warehouse. When we started the design process we had 250k units, and knew that the number would quickly grow as we now have many, many more than that. And all of it needs to be tracked.


## **Design considerations**


Our tracking is twofold. First, we need to know what general station a unit is in: with the customer; in cleaning; in quality control; or on the rack, ready to be shipped to a customer. Second, when a unit is on the rack, we need to know where it is in the racks, which we find out based on a mapping in the database containing styles and their locations. A style-location mapping tool was one of the first things I worked on at RtR. The warehouse team needed an interface to manage those mappings. Our automated picking and putaway system depends on them.


Another consideration when redesigning the tool was mobility. Many parts of our warehouse management tool were built with a desktop browser in mind, not mobile units. Laptops were as mobile as they would get. So when the team did a cycle count using the old tool, it would take two people, a mobile laptop station, and a barcode scanner attached to the laptop. One person would scan the units on the rail, and the other one would watch the results on the screen.


Also, for the upper rails, the person counting would have to go up and down a ladder because half the inventory is in the upper rail in the aisle!


Another really big issue was the timeouts that routinely occurred with the old tool. It worked by scanning all the units in a style and then submitting all of them at once to the server, which would then calculate missing units, misracked, etc. But there was a timeout on the page, and once you started counting you had a finite window within which you had to scan it all or it timed out - and all your scans to that point would be lost. One of the first decisions we made was to make each scan atomic, so its content would be saved instantly. That way, workers could scan along at a good pace but not have to worry about losing their work if they exceeded a timeout window.


We were constrained by the fact that the smallest collection of units with strongly delineated start and end is the "rail."A rail is just a fancy term for the long pipe that we hang our garments off of for storage. It contains sections. It looks sort of like this:


Unfortunately, the boundary between one section and the next is not strongly delineated, so units can and do easily move to the next section if a style gets really crowded. For example, units that are supposed to be in section “001” may slide over to “002.” For this reason, taking stock of what you've got in your present count at the end of a single section could lead to thinking you are missing units when they aren't really missing. You really need to assess at the end of a discrete space. Therefore, we decided to associate a cycle count with the entire rail and not to take stock until we scan all the sections in the rail. We were also assured that styles would not run from one rail to the next.


(Thanks to Rachael McKnight for assistance with this illustration.)


Finally, we designed with an eye toward minimizing the number of steps a count would take. The teams are counting hundreds of units per rail - per cycle count - so, for example, clicking "OK" could mean hundreds of excess actions. We tried to eliminate confirmations and pop ups wherever possible in the process. Of six message responses possible when scanning a unit, we were able to design all but two with only a courtesy message (e.g., “scanned barcode 12345678 to section 001”) that disappears when the next scan is read. The two that require intervention are meant to (a) draw attention to what we call "anomalies," mainly misracked items, and (b) ensure the scanner removes the unit from the rack and places it aside for reracking later. And even in this situation, if this is a unit that has "slid over" to the next section, we offer an option to add a style-location mapping for the current location, so any subsequent units that would have come up misracked as well will then just be read as in a correct location.


Anomalies include misracked, missing, and duplicate barcodes. True duplicates are extremely rare, but duplicate scans may occur if the scanner loses their place and scans the same unit twice. This is the other screen where we require a reaction, to let us know if it was a true duplicate (or just finding where they left off). Items may be misracked because the unit has "slid over" to the next section, or really goes somewhere else in the warehouse.


## **Process**


A user will start a new cycle count by scanning the barcode for any section in a rail:


**The starting screen**


The system will look for a cycle count already in progress. If none is found, it will initiate a new cycle count and ask the user to start scanning barcodes of garments.


For each garment barcode scanned, the system will ask itself if the garment is misracked or the barcode is a duplicate, and it will respond accordingly.


**A misracked unit**


**A duplicate scan**


If the barcode is neither misracked nor a duplicate, the application will query the database to see if the unit is expected on rack, and if so, if it is a unit in an active state (not set aside for clearance or sale). If both these are true, the app will display a courtesy message saying the unit is expected at the location. The user can then scan the next unit’s barcode.


At the end of a section the scanner will click a button to end the current section and is prompted to scan the next section's identifier. They continue counting units until they reach the end of the rail. Then they click a button to close out the rail.


At this point the application will calculate missing items based on what inventory we expect to be in the sections scanned. Then the app will display a summary of misracked and missing units. As part of completing a cycle count the team must put away misracked units in their proper locations, and attempt to locate missing units in the rail just scanned.


Once they have found all the missing units they can and put away all the misracked units, the cycle count for the rail is marked complete.


**A report for a completed cycle count**


## **Conclusion**


At Rent the Runway, the number of items we are managing is exponentially increasing, and we’re already well on the way to building an even more scalable way to ensure the right items are in the right place at the right time, to make sure we really can be our customers’ “Closet in the Cloud.”
