---
schema_version: "1.0.0"
document_id: "893e4d532a6fb0c848fdd127304e39377fc9c9a2aa517d2a7f1ab430a493f9d1"
company_key: "yc-alkali"
company: "Alkali"
source_id: "yc-alkali-news-import-7da3f732694e"
canonical_url: "https://www.alkali.engineering/blog/alkali-guide-to-nesting"
published_at: null
first_seen_at: "2026-07-21T05:51:46.944392+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:39ae95e0393aabf45f76c43057b95b170b0e1a620c66b43b506ca4e7dafdfb84"
---

# An Intro to Nesting

## What's the Problem?


Structural steel fabricators lose money on every inch of metal stock wasted. Even on a small job, metal waste can clear a few thousand dollars from your margin. This adds up over the course of a year. Fabs fight this through efficient nesting, the art of getting the minimum metal needed for their cuts.


## What's Nesting?


Nesting is how cuts are planned on stock pieces. Stock pieces are the lengths of steel (usually 20', 40' or 60') purchased from the metal manufacturer, supplier, or distributor. Nesting is how to cut out usable pieces from these stock pieces. The challenge:


- **Do this accurately** : there can be dozens to thousands of different beams and columns to cut. Nothing should be forgotten.
- **Do this fast** : by-hand nesting for dozens of beams is time consuming. Thousands is impossible.
- **Do this efficiently** : wasted material is wasted spend.


To do this efficiently, we introduce the idea of a **pattern** . A pattern is the collection of cuts made on a stock piece. A fab might want to cut two 15' beams and one 10' beam. Consider one pattern on a 40' stock vs 3 patterns on three pieces of 20' stock for doing this.


Pattern Waste Comparison: 40' vs 20' Stock Stock Length Pattern Drop Waste


40' x1 Stock 1: 15', 15', 10' 0' 0'


20' x3 Stock 1: 15'
Stock 2: 15'
Stock 3: 10' Stock 1: 5'
Stock 2: 5'
Stock 3: 10' 20'


We can see different patterns on different stock lead to different waste. As the amount of stocks and cuts grows, it gets harder and harder to figure out which pattern is the most efficient.


There are over a dozen different wide flange shapes in this framing plan snippet, with many different lengths. Multiply that across multiple floors in this med center—the number of cuts and lengths to track can be over a hundred, and this can lead to over a million possible patterns.


In addition to choosing the stock lengths and pattern, the following considerations are also important:


- **Kerf** , or how much metal is lost per cut.
- **End trim** , or how much static length we need on the end of the beam to hold the piece steady while cutting.


So, available stock lengths, kerf, and end trim are all factors in finding the right patterns to cut your steel. For a given job there could be just a few patterns, but often there are thousands or millions. This makes finding an efficient pattern like searching for a needle in a haystack. It's worth it, if done fast enough. Money is on the line!


---


## Practical Nesting


In practice, nesting will be done through software, since that's the only way to get through the thousands or millions of patterns efficiently. So you know you need stock lengths, cuts, kerf, and end trim, which is what every software will ask of you. We'll briefly cover how to choose these. Then we'll cover tips for effective nesting.


**Stock lengths** : these are provided by whoever you're buying from. Often, allowing your nesting software to use longer stock makes for less waste.


**Cuts** : these will come from your plans and schematics.


**Kerf** : different cutting methods will yield different kerf. The manufacturer of your cutting device (e.g., plasma vs saw) should have recommended kerf allowances. Follow your manufacturer's guidance. Free guides are also available online[here](https://esab.com/us/nam_en/esab-university/blogs/what-is-cutting-kerf-and-why-is-it-important/) ,[here](https://zsyishang.com/cutting-kerf-metal-fabrication-guide/#Laser_Cutting_-_High_Precision_Clean_Edges) , and[here](https://fractory.com/what-is-cutting-kerf/) .


**End Trim** : this is going to depend on your machine. Your machine may "grip" part of the stock and that cannot be advanced past the saw—this is lost length that cannot be used and hence should be accounted for when nesting. Again, your machine manufacturer should have guidance.


With these in mind, you can start nesting.


This is the minimum to start nesting in any software. Here is an example entry in the Alkali Platform.


But how to get the most out of nesting? Here's an easy trick: **nest across jobs!** The more material you put into the nesting, the more ways the software can figure out how to organize the cuts to minimize waste.


Some pitfalls to avoid, as well:


**Pitfall 1** : Check if that large beam is actually 70', or if it is composed of small splices! If the 70' beam is actually several smaller pieces welded or bolted together, then the nesting only needs the smaller splices.


**Pitfall 2** : Make sure the beams you order are the size you can have delivered. Are you able to rent a big enough truck to get that 60' stock on the highway?


**Pitfall 3** : Double check your beam measurements. If your cut is too small, you might run out of space on your stock length.


With this in mind, you're ready to save money with nesting. But how?


---


## Getting Started


Your options are to nest by-hand, ask a distributor to nest for you, or use your own software. We'll cover each, but we recommend using your own software.


You can try to nest by-hand, but that is challenging beyond a few sticks. To nest by hand, you'd have to sit down, take a stock length, and fit what you can. If you can't fit something, either pull up a new stick or go up a stock size. Hope that in the drop you can fit something so you don't waste material. But just getting workable patterns for every stick could take hours. Sometimes, mistakes are made—pieces are dropped or time is lost.


This was painful enough that mathematicians Gilmore and Gomory came up with an early algorithm in[1961](https://www.cs.uleth.ca/~benkoczi/OR/read/cutting-stock-LP.pdf) . These modern nesting algorithms were first implemented in an early software in 1984 by ProNest. What could take hours or days now happens in seconds.


You can have distributors nest for you, but sometimes that means you miss out. How so? Some fabs see considerable savings by using larger stock lengths, but the distributor or supplier might not sell those—the distributor is going to sell whatever works for them. Instead, you can go right to the mill and get a better deal. As a bonus, the mill is often cheaper on a per-foot basis anyways (though you'll usually have to wait an extra day for delivery). You'll only know if you can save cash if you nest yourself.


Today, nesting by software is all but mandatory—it's priced into many jobs and expected by GCs. We offer nesting through our[Alkali Platform](https://www.alkali.engineering/) . The platform will automate the takeoff for the beams and columns, and then with a click of a button load it into our nesting software and auto-nest. Even better, it will provide warnings about the above pitfalls—to make sure you get the most out of your material purchases. It's free to try, so see if it works for you.


[Try Alkali Here](https://www.alkali.engineering/)
