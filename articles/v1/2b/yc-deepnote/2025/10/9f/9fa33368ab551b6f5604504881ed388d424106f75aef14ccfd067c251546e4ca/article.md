---
schema_version: "1.0.0"
document_id: "9fa33368ab551b6f5604504881ed388d424106f75aef14ccfd067c251546e4ca"
company_key: "yc-deepnote"
company: "Deepnote"
source_id: "yc-deepnote-news-import-99d40c54e3ad"
canonical_url: "https://deepnote.com/changelog/2025-10-23"
published_at: "2025-10-23T00:00:00+00:00"
first_seen_at: "2026-07-21T16:01:08.376400+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:aef301dd6d3ff44aae011270bbedd4c705614d0d58af3d6b22e23ac429b53ca7"
---

# Bring your own AI model & GitLab export & new admin powers

## [October 23, 2025](https://deepnote.com/changelog/2025-10-23)


#


[Bring your own AI model & GitLab export & new admin powers](https://deepnote.com/changelog/2025-10-23#bring-your-own-ai-model--gitlab-export--new-admin-powers)


###


[Bring your own AI model](https://deepnote.com/changelog/2025-10-23#bring-your-own-ai-model)


**Enterprise** workspaces can now connect their own **OpenAI-compatible endpoints** to power **Deepnote Agent** - giving you full control over the AI provider behind your data analysis workflows.


This is especially valuable for organizations that need to:


- Meet compliance or regional data requirements with specific AI providers
- Leverage proprietary or fine-tuned models optimized for specific use cases
- Keep data within specific infrastructure boundaries


Setting it up is straightforward:


- Head to **Settings & Members** → **AI tab** and click **Add model**
- Provide your endpoint URL, model ID, and API key
- Set it as your workspace default, and Agent will use your custom model for all analysis tasks


[Learn more about custom AI models](https://deepnote.com/docs/ai-custom-models)


###


[Export notebooks to GitLab](https://deepnote.com/changelog/2025-10-23#export-notebooks-to-gitlab)


Great news for GitLab users - you can now **export your notebooks directly to GitLab repositories** , just like you've been able to with GitHub.


Whether you need to back up your work, maintain audit trails for compliance, or trigger automated pipelines, exporting to GitLab is now just a few clicks away:


- Open any project, click the **Version history icon** in the top right, then select **Connect Git repository** and choose **GitLab** (you'll connect your GitLab account if you haven't already)
- Pick your repository, specify the target branch and path, and choose whether to include outputs
- Export manually with the **Commit & push** button, or **set up automatic exports** whenever you create a new version


[Check out the docs](https://deepnote.com/docs/git-export) to get started with GitLab exports.


###


[New admin powers](https://deepnote.com/changelog/2025-10-23#new-admin-powers)


We've shipped a suite of **new workspace settings** that give admins more control over security and permissions - perfect for enterprise teams with strict governance requirements.


- **Restrict email invites to your domain** : Admins can now limit workspace and project invites to only email addresses matching the workspace owner's domain, preventing accidental external invites
- **Disable incoming connections workspace-wide** : Turn off the ability for project editors to enable incoming connections, giving admins full control over external access points
- **Control private project creation** : Disable the creation of new private projects in your workspace while keeping existing ones intact
- **Set default permissions for public projects** : Choose what permissions workspace members get on newly created public projects - from Full access down to View only


These controls give you the flexibility to create the right permission environment suited to your organization's specific security and collaboration needs.
