---
schema_version: "1.0.0"
document_id: "dea3ad5e31ea744ae1c796cd215c5456891c4a07ef24cacbb07a3ab32d8c1081"
company_key: "yc-embrace"
company: "Embrace"
source_id: "yc-embrace-news-import-2dd1f24c69b5"
canonical_url: "https://embrace.io/blog/enriching-our-user-insights-tools-with-more-data-filters-and-context/"
published_at: "2025-03-03T23:11:40+00:00"
first_seen_at: "2026-07-21T17:58:27.596992+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:e04885458e599e6cb5be9c9bd790ca4732a6406b3b585c2ee179c9f59d87c21b"
---

# Enriching our user insights tools with more data, filters, and context

Check out what's new with Embrace's User Timeline and User Sessions pages. We've introduced some upgrades that enrich the data and filtering capabilities of these two features, providing even better context and customization capabilities for engineers who want deep insight into their end users' mobile experiences.


Embrace’s User Sessions and User Timeline features are at the core of our product, with both providing insight into real user experiences.


The Sessions page enables developers to isolate individual user sessions based on a multitude of parameters they might be interested in, such as the presence of crashes or the tagging of specific user IDs.


The User Timeline allows developers to then retrace the entire user experience of the session, visualizing a trail of both technical and behavioral events with a high level of detail. Using the Timeline, you can troubleshoot any issue with the full context of what events came before and after it.


Now, we’ve made these user impact analysis tools even more powerful by releasing a slew of data enrichment features. Read on to learn more.


## User Sessions feature enrichment


**Session properties filter** : You can now apply a filter for Session Property to search for individual user sessions in the Sessions page. Session properties are created as key/value pairs, with the filter supporting search for sessions that contain a name session property or in a particular key within a session property.


**Saved complex filters:** Embrace supports building your own complex filters to slice and dice your mobile data. Complex filters can contain different combinations of parameters. For example, you can build a custom complex filter with the following conditions: \[has crash = true\] AND \[has low memory warning = true\] AND \[app version = 2.1.2\]. These complex filters can be saved for use in later analysis and, now, our Sessions page supports using them to isolate individual user sessions that match the complex filter conditions.


## User Timeline feature enrichment


**Searchable breadcrumbs** : Breadcrumbs are small text strings that contain useful context about an event during a session, similar to logs. They are displayed in order on the User Timeline. Now, you can type in any text from the breadcrumb into the User Timeline search bar to quickly find the breadcrumb across all the events of the session.


**Exposed breadcrumb attributes:** Breadcrumbs can have attributes, which are set up as key/value pairs, given to them. Attributes provide greater context to enrich your understanding of what event or condition a breadcrumb is marking. Now, those attributes are visible directly in the User Timeline view, in addition to the standard breadcrumb message and timestamp.


**Visible app build number:** The OS build ID number is used to determine the symbol file needed to desymbolicate stack traces, and when this file is missing, stack traces remain obfuscated in the Embrace platform. In the Embrace User Timeline, stack traces of crashes have always been visible. Now, if there is a desymbolication issue, you can hover over the build ID number directly in the Timeline, to quickly spot which file you need to add to support desymbolication.


**Visible hosted SDK versions:** React Native, Unity, and Flutter developers will benefit from added information on the User Timeline. They can now see which hosted version of the Embrace SDK an end user has installed on their app, directly in the timeline. This information was already visible for native app SDKs.


## More to come


These updates represent just a few of the recent feature capabilities that Embrace has been working on to enrich our platform with greater user context. Stay tuned as we continue to add more functionality to our Sessions and User Timeline pages.


If you’d like to learn more about what these features do, head over to our


[docs site](https://embrace.io/docs/features/user-session-insights/) . If you’re new to Embrace and are interested in getting started for yourself, you can do so for free


[here](https://dash.embrace.io/signup) .


Deliver incredible mobile experiences with Embrace.


Get started today with 1 million free user sessions.


[Get started free](https://dash.embrace.io/signup/)


Author


[Virna Sekuj](https://embrace.io/author/virna-sekuj/) Virna Sekuj is a product marketer at Embrace. She has nearly ten years of experience in product management, marketing, and research analysis. Prior to working at Embrace, Virna worked at Bose, Onside Sponsorship, and GWI. In her time with Embrace, she’s used her insight and analysis expertise to lead two research studies polling engineers that have produced two reports — The State of Mobile Experience and The Mobile Developers Pain Points report.


Related Content


User Session Insights


5 April 2024


• 3 min read


### [Updates to Embrace’s User Sessions and Timeline](https://embrace.io/blog/user-timeline-updates/)


We've made some exciting new updates to Embrace's User Timeline and User Sessions pages. Read on to learn more about this, and how they can help engineers troubleshoot issues via rich context into the user experience.


App Performance


4 April 2024


• 7 min read


### [What is a User Timeline?](https://embrace.io/blog/what-is-user-timeline/)


Learn about the Embrace User Timeline: what it is, how it benefits your team, and how to best leverage the data from it to improve your app.


User Session Insights


1 September 2022


• 6 min read


### [How stitched sessions in mobile apps provide full user experience visibility](https://embrace.io/blog/how-stitched-sessions-mobile-full-user-experience-visibility/)


Unleash the power of stitched sessions in mobile apps, as we delve into how this innovative approach provides comprehensive visibility into the complete user experience journey, allowing developers to analyze user interactions, uncover patterns, identify bottlenecks, and optimize app performance for a seamless and delightful user experience.
