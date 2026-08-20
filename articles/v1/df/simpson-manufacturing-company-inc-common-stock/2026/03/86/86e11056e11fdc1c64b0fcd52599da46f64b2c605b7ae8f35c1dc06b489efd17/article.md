---
schema_version: "1.0.0"
document_id: "86e11056e11fdc1c64b0fcd52599da46f64b2c605b7ae8f35c1dc06b489efd17"
company_key: "simpson-manufacturing-company-inc-common-stock"
company: "Simpson Manufacturing Company Inc."
source_id: "simpson-manufacturing-company-inc-common-stock-rss-b64b6a87e68b"
canonical_url: "https://seblog.strongtie.com/2026/03/how-i-learned-to-stop-worrying-and-love-multi-period-seismic-design/"
published_at: "2026-03-11T15:00:14+00:00"
first_seen_at: "2026-07-20T23:17:24.279995+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:b3698044aaadff08e6a6979607cc184711ef5baf32fcea0a641fe2eb3eb569da"
---

# How I Learned to Stop Worrying and Love Multi-Period Seismic Design

*One of my most vivid memories from college is my Struct I professor teaching us that civil engineering is an empirical field. He wasn’t just defining what empirical meant, he was also teaching us we need to ready our minds for change. This instruction came at a fortuitous time. This was 1997 and three years prior to that the Northridge earthquake had rocked Los Angeles and was about to change the way structural engineers look at seismic design and detailing forever. Beginning with the changes codified in the 1997 UBC, the standard of practice for seismic design today has evolved to become radically different from what was followed prior to Northridge. In this article the change I would like to help us all understand is multi-period seismic in ASCE 7-22 — where it came from, what’s “multi-” about it, and how it will affect what you do in your own designs.*


**Why Multi-Period Seismic Design?**


In 2018, the U.S. Geological Survey (USGS) updated its National Seismic Hazard Model (NSHM). Part of this update included recommendations from the Building Seismic Safety Council (BSSC) Project 17 Committee. BSSC expressed concerns that current models did not adequately address soft soils (Site Class D). They contended that current models depend too much on unjustified interpolation to the point of being unconservative. The specific interpolation they targeted was that between one-second and long-period (TL) and the spectral shapes between Site Classes B, C, D, and E. The result of this, they argued, was that moderately long-period structures had unconservative Sa values and that site amplification factors, Fa and Fv, did not adequately address soft soils. Below is a summary of their requests and the actions taken by USGS:


BSSC requests


1. Incorporate response spectra for more than two periods (0.2s, 1s).
2. Incorporate references for more site classes than one (Site Class B).
3. Eliminate site amplification coefficients (Fa, Fv).


USGS actions:


1. The USGS incorporated response spectra for 22 periods from 0s to 10s.
2. The USGS referenced eight site classes from hard rock to very soft soil.
3. Engineers will pull Sms and Sm1 directly from response spectra maps based on site class rather than those factors being the products of SS, S1, Fa, and Fv.


These changes make up what is known as Multi-Period Seismic Design.


### **What’s Different About** **Multi-Period Response Spectra (MPRS)?**


For one, the shape of the spectra map is different. Instead of the two-period graph with the plateau between T0 and TS, with an Sa capping out at SDS, the multi-period spectra peaks and falls.


Two-Period Design Response Spectrum Multi-Period Design Response Spectrum


Beyond this superficial comparison, the way the spectral response graph is created is different. Prior to ASCE 7-22, when engineers would build a response spectrum, they would pull spectral values for two periods (0.2s, 1s) from USGS maps and a long-period transition value from ASCE 7. USGS reported spectral values (Ss, S1) for Site Class B, so the engineer would need to amplify them based on the actual site class with Fa and Fv. After plotting these points on a map, the engineer would interpolate a curve between 1s and TL. With multi-period response spectra, by contrast, engineers pull Sa values directly from USGS based on site class, and those values will be graphed directly without amplification. Engineers still need to interpolate values, but because there are more USGS spectral accelerations, they can do so linearly.


ASCE 7-22 has additional criteria for using MPRS graphs in design:


1. Instead of using SDS for the Cs calculation, you need to pull Sa directly from the table based on period T.


Method 1


2. However, when design period T is less than the period where Sa is maximum, per ASCE 7-22 Section 12.8.1, you shall use the maximum.


1. Default site conditions that EORs need to consider without more detailed information are also different.


Lower bounds remain the same as those for two-period seismic design.


### **Practical Implications for Engineers**


While this seems like a lot of work for a small change, the analysis brings a big benefit that will make life easier for design engineers. Engineers will no longer be required to perform a site-specific analysis of longer-period structures on soft soil sites when they use MPRS.


Another plus is that the ASCE Hazard Tool has already integrated MPRS mapping into its system. The tool will even calculate the default site classes and them for you as well!


ASCE Hazard Tool


For design, you will see some change, but less than you think. Here is a comparison of a two-point spectral response graph vs. a multi-period spectral response graph for my city.


Default MPRS vs Default Two-Period Spectrum


What you may notice first is the peak goes above the plateau. Low-period structures may see an increase in their Sa value from previous codes, especially if that period is to the left of the peak. Design engineers of low-rise structures will want to sharpen their pencil when it comes to verifying the building period to avoid taking a larger hit than necessary. There are also moderately higher Sa values between 1s and 3s. This demonstrates what the BSSC was concerned about with respect to the interpolation between 1s and TL.


### **Final Thoughts**


In conclusion, change is never easy, especially when trying to navigate it while remaining efficient and profitable. However, with a little understanding and some help from modern tools, we can succeed and feel confident about the change. Looking back to those early years, the switch from UBC prescriptive Cs values to two-period seismic analysis carried more trepidation than this. I’m excited to see where we go from here.


[Share](https://www.linkedin.com/cws/share?url=https%3A%2F%2Fseblog.strongtie.com%2F2026%2F03%2Fhow-i-learned-to-stop-worrying-and-love-multi-period-seismic-design%2F)


[Share](https://www.facebook.com/share.php?u=https%3A%2F%2Fseblog.strongtie.com%2F2026%2F03%2Fhow-i-learned-to-stop-worrying-and-love-multi-period-seismic-design%2F)


[Tweet](https://twitter.com/intent/tweet?text=How+I+Learned+to+Stop+Worrying+and+Love+Multi-Period+Seismic+Design&url=https%3A%2F%2Fseblog.strongtie.com%2F2026%2F03%2Fhow-i-learned-to-stop-worrying-and-love-multi-period-seismic-design%2F&via=strongtie)


[Pin](https://pinterest.com/pin/create/button/?url=https%3A%2F%2Fseblog.strongtie.com%2F2026%2F03%2Fhow-i-learned-to-stop-worrying-and-love-multi-period-seismic-design%2F)


## Author:


Eric Hansen


Eric Hansen is a licensed SE in Utah and a licensed PE in California and Texas. He also holds a PMP certification with the Project Management Institute. Eric joined Simpson Strong-Tie in 2024 after a 25-year career in structural design consulting. He lives in Salt Lake City, Utah with his wife, eight kids, and a dog.[View all posts by Eric Hansen](https://seblog.strongtie.com/author/eric-hansen/)
