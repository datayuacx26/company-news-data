---
schema_version: "1.0.0"
document_id: "d33465e770b7744acb1d714c78240e4cf7f50dba43c0331f664fa52f9f9e75d4"
company_key: "yc-acunote"
company: "Acunote"
source_id: "yc-acunote-news-import-fc65b5e5f495"
canonical_url: "https://www.acunote.com/blog/2014/11/bitbucket-integration.html"
published_at: "2014-11-24T00:00:00+00:00"
first_seen_at: "2026-07-24T14:19:11.091402+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:551fffd43251a0af3a1771123cb4769925f649d4fb42a254fd994746c84b8fc8"
---

# Bitbucket Integration

Acunote now integrates with Bitbucket in addition to GitHub, Subversion, and Perforce. With Bitbucket integration you can see commits pushed to Bitbucket repositories (both Git and Mercurial) in the Timeline, perform code review on commits and create code inspection tasks.


## How to enable Bitbucket-Acunote integration


In Acunote:


- go to **Edit Organization > Repositories** and copy your **Bitbucket POST Hook URL** to the clipboard


In Bitbucket:


- visit the repository **Settings** page
- select **Hooks** from the menu
- select **POST** from the list of available hooks and click **Add hook**
- paste the url you copied from Acunote into **URL** field
- click on "Save"


Once hook is configured, new commits will start appearing automatically in the Acunote Timeline.


Note that you need to do this configuration separately for each of Bitbucket repositories you want to integrate with Acunote.


## Additional configuration for private Bitbucket repositories


When you click on a commit in the timeline Acunote will fetch and show full details and diff for the commit. If your repository is public, no further configuration is required to do this. For private Bitbucket repositores Acunote also needs **Bitbucket username and password** .


To set this up in Acunote:


- wait for first commit from Bitbucket to appear in Acunote Timeline
- click on the commit to get to commit deatils
- follow the **provide valid Bitbucket username and password** link to the repository details page
- ( **alternately** you can go there from **Edit Organization > Repositories** page)
- enter your Bitbucket username and password
- refresh the commit details, you should now see the diff
