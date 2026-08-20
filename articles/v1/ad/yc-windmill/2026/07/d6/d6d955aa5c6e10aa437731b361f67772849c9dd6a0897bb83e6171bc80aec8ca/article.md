---
schema_version: "1.0.0"
document_id: "d6d955aa5c6e10aa437731b361f67772849c9dd6a0897bb83e6171bc80aec8ca"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/ansible-repo-ansible-cfg"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-25T01:07:57.288074+00:00"
fetched_at: "2026-07-28T20:47:42.945239+00:00"
content_hash: "sha256:4bd7d306afbf16766cd2f31c0d4efcaaa1d3bb9375b299ae797188721fd0f163"
---

# Use a repo-provided ansible.cfg when delegating to a git repository

### [Use a repo-provided ansible.cfg when delegating to a git repository](https://www.windmill.dev/changelog/ansible-repo-ansible-cfg)


Script editor


Ansible


[v1.744.0](https://github.com/windmill-labs/windmill/releases/tag/v1.744.0)


[Docs](https://www.windmill.dev/docs/getting_started/scripts_quickstart/ansible#using-the-repos-ansiblecfg)


Ansible scripts that delegate to a git repository can set the new ansible_cfg field, a repo-relative path to an ansible.cfg that becomes the effective configuration via ANSIBLE_CONFIG, so repo settings like roles paths, inventory plugins and callbacks apply. Windmill only re-applies runtime-bound settings (temp directories, vault password) and prepends its own roles and collections install paths. Requires workers running with DISABLE_NSJAIL=true; without the field, Windmill's generated config takes precedence as before.


#### New features


- New optional ansible_cfg field in the delegate_to_git_repo block, taking a path relative to the cloned repo root
- ANSIBLE_CONFIG points at the repo config so its roles paths, inventory plugins, callbacks, host_key_checking and ssh settings apply
- Windmill layers back only runtime-bound settings: temp/home directories in the job dir and vault password/identities from the script metadata
- Windmill-managed galaxy install locations are prepended to the roles_path and collections_path declared in the repo config
- The field supports {{ argname }} placeholders like playbook, commit and inventories_location
- The job fails with a clear error if the file is missing from the cloned repo; without the field, behavior is unchanged
