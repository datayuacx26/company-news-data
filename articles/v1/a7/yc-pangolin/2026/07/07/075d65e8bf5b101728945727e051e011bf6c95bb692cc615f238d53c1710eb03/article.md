---
schema_version: "1.0.0"
document_id: "075d65e8bf5b101728945727e051e011bf6c95bb692cc615f238d53c1710eb03"
company_key: "yc-pangolin"
company: "Pangolin"
source_id: "yc-pangolin-news-import-841387734568"
canonical_url: "https://pangolin.net/news/1-20-release"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-22T08:11:44.188163+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:82210398014642a8d384f2271a1874895c5c6778aec70991146f606eb096d9dc"
---

# Pangolin 1.20: Resource Launcher & Global Command Palette

Pangolin 1.20 is here. This release focuses on how people actually find and reach their resources: a rebuilt Resource Launcher for everyday users, a command palette for administrators, and a cleaner private resource workflow across the dashboard. Let's walk through it.


## Release Highlights


### Resource Launcher


The Resource Launcher is the landing page non-administrators see when they sign in. It was always meant to be a quick way to find and open the resources you have access to, but the old version was limited. You could see what was available, but not much beyond that.


The new Resource Launcher is available to both non-administrators and administrators, and it's built to scale with how your organization actually works. Resources are grouped by site or label, searchable, and filterable. Switch between grid and list view depending on what you're looking at. Collapse site sections you don't need and focus on the ones that matter.


Saved views are the other big piece. A saved view captures your full launcher configuration: which filters are active, how resources are grouped, whether you're in grid or list mode, and anything else you've set on the page. Save it as your personal default, or save it as a named view you can switch between from the tabs at the top.


Administrators can save views for everyone in the organization, set the organization-wide default, and still maintain their own personal views on top of that. Non-administrators can save views for themselves. The result is a launcher that works as a reusable starting point, whether you're an admin curating a page for a team or an end user who wants a quick way back to the same handful of sites every morning.


### Command Palette


Administrators now get a global command palette. Press **⌘K** (or **Ctrl+K** on Windows) from anywhere in the dashboard to open it.


The palette is a single search surface for navigation and discovery. Type a page name to jump straight to Sites, Users, Roles, or anywhere else in the dashboard. Search for a resource or site by name and the palette surfaces matching public resources, private resources, and sites. Select a result and you're there.


If you manage a large deployment, this cuts down on clicking through the sidebar to find the thing you already know the name of. Need the Atlanta retail POS resource? Search for it. Need to get to the Sites page? Same palette.


### Private Resource Pages


Private resources now follow the same page-based patterns as the rest of the dashboard. Previously, creating and editing a private resource happened in a pop-up modal. That worked at small scale, but it felt out of step with how public resources, sites, and other entities are managed elsewhere.


Create and edit private resources on dedicated pages now, with the same layout and navigation you're used to from other parts of Pangolin. No screenshot for this one, but if you've been managing private resources for a while, you'll notice the difference immediately.


### General Improvements and Bug Fixes


As always, 1.20 also includes various UI improvements and bug fixes throughout the product.


### Community Edition & VNC


[Labels](https://pangolin.net/news/1-19-release) , introduced in Pangolin 1.19, have moved from Enterprise Edition to Community Edition.


VNC connections now include a username field in addition to the password. Previously, only a password was required. That improves compatibility with a wider range of VNC servers, including the built-in macOS VNC server.
