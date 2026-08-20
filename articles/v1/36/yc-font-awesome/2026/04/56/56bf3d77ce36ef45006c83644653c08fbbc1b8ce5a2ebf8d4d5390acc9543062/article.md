---
schema_version: "1.0.0"
document_id: "56bf3d77ce36ef45006c83644653c08fbbc1b8ce5a2ebf8d4d5390acc9543062"
company_key: "yc-font-awesome"
company: "Font Awesome"
source_id: "yc-font-awesome-atom-991752a735d6"
canonical_url: "https://blog.fontawesome.com/wa-3-6-graduation-day-for-experimental-components/"
published_at: "2026-04-30T22:50:08+00:00"
first_seen_at: "2026-07-25T05:28:06.603545+00:00"
fetched_at: "2026-07-28T20:51:46.128682+00:00"
content_hash: "sha256:9750bcb330805d52d3b852ee36949c7ee1ebf8421ad77c318d86d0e7da7e53c3"
---

# WA 3.6: Graduation Day for Experimental Components

# 🎓 Con_grad_ulations!


They’ve been battle-tested. They’ve been tuned-up. And, now, they’re now ready to take the stage in your projects. Here are our WA 3.6 component graduates who we’ve upgraded from *experimental* to *stable* :


- *<wa-chart>* (including all *<wa-*-chart>* components)
- *<wa-combobox>*
- *<wa-file-input>*
- *<wa-number-input>*
- *<wa-sparkline>*
- *<wa-toast>*


Components move out of *experimental* status when they’ve had enough time out in the wild that we feel confident in their APIs. With *stable* components, we won’t implement any breaking changes until we release a major version upgrade of the library as a whole.


# 👕 *size=””* Matters


We’ve added two new sizes to all components that support the *size* attribute to round out the scale: *xs* and *xl* . If you’ve ever needed a barely-there *<wa-button>* or an in-your-face *<wa-callout>* , you’re in luck


The full gamut of sizes, demonstrated on


Along with this change, we’ve renamed the existing *small* , *medium* , and *large* sizes to *s* , *m* , and *l* to better align with how we name sizes in our design tokens and utility classes. (Bonus: Less to type!) While the long-form names have been deprecated, they’ll continue to work in every minor version release of WA 3.x.


# ✨ The Usual Extra Sparkle


As usual, there’s a slew of other improvements and fixes in WA 3.6, including a sweeping refactor of our component tests with better test coverage.[Read our changelog](https://webawesome.com/docs/resources/changelog#wa_360) for all of the glitz and glam.
