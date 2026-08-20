---
schema_version: "1.0.0"
document_id: "79634b05448a0ce52d1fe143099e5a6089fd6220d249bf2a493dc38dc4038166"
company_key: "yc-redbrick-ai"
company: "RedBrick AI"
source_id: "yc-redbrick-ai-news-import-533597001763"
canonical_url: "https://app.redbrickai.com/docs/changelogs/redbrick-sdk-changelog"
published_at: null
first_seen_at: "2026-07-25T20:44:06.789868+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:850e53b866a2fa482b8a63756899d976e9c8a168bc918b703e3916248f833a5b"
---

# RedBrick AI Python SDK changelog

​


Jul 20, 2026


**Changed:**


- Allow exporting members productivity -[https://sdk.redbrickai.com/sdk.html#redbrick.common.member.Team.get_members_productivity](https://sdk.redbrickai.com/sdk.html#redbrick.common.member.Team.get_members_productivity) &[https://sdk.redbrickai.com/sdk.html#redbrick.common.member.Workforce.get_members_productivity](https://sdk.redbrickai.com/sdk.html#redbrick.common.member.Workforce.get_members_productivity)
- Allow conversion of 2D DICOM images to NIfTI
- Ability to add labels along with the new series added through[https://sdk.redbrickai.com/sdk.html#redbrick.common.upload.Upload.update_task_items](https://sdk.redbrickai.com/sdk.html#redbrick.common.upload.Upload.update_task_items)
- Improve API error logging.


​


May 15, 2026


**Changed:**


- Allow uploading empty masks.


​


May 11, 2026


**Changed:**


- Add` concurrency` to` project.labeling.assign_tasks()` .


​


Apr 1, 2026


**Changed:**


- Add support for .html reports.


​


Mar 11, 2026


**Changed:**


- Added` overlappingGroups` for tasks having overlapping labels but exported as single multi-class mask -[https://sdk.redbrickai.com/formats/annotations.html#redbrick.types.task.CommonLabelProps.overlappingGroups](https://sdk.redbrickai.com/formats/annotations.html#redbrick.types.task.CommonLabelProps.overlappingGroups)


​


Feb 25, 2026


**Changed:**


- Session-based SDK authentication: Run` redbrick login` to authenticate via browser and receive short-lived session credentials, replacing long-lived API keys. (redbrick.get_project(org_id, project_id) -[https://sdk.redbrickai.com/sdk.html#redbrick.get_project](https://sdk.redbrickai.com/sdk.html#redbrick.get_project)
- Dataset Releases: Named, point-in-time snapshots of tasks for consistent and reproducible exports. Visible across the Data Page and task analytics. Run` redbrick releases` to manage Dataset releases. (project.releases) -[https://sdk.redbrickai.com/sdk.html#redbrick.common.releases.Releases](https://sdk.redbrickai.com/sdk.html#redbrick.common.releases.Releases)


​


Feb 24, 2026


**Changed:**


- Added support for video file uploads.


​


Dec 19, 2025


**Changed:**


- Removed` with_labels` argument in` Export.get_task_events()` . The method now exports` labelsId` by default for each task submission event -[https://sdk.redbrickai.com/sdk.html#redbrick.common.export.Export.get_task_events](https://sdk.redbrickai.com/sdk.html#redbrick.common.export.Export.get_task_events)
- Added` Export.export_task_labels()` to export a specific label version for a task -[https://sdk.redbrickai.com/sdk.html#redbrick.common.export.Export.export_task_labels](https://sdk.redbrickai.com/sdk.html#redbrick.common.export.Export.export_task_labels)


​


Nov 26, 2025


**Changed:**


- Improve error reporting during task uploads.
- Improve resolution of temp config path.
- Fix download of task items with duplicate items.


​


Nov 25, 2025


**Changed:**


- Improve task upload performance.


​


Nov 14, 2025


**Changed:**


- Keep spaces in project and task file names on exports.
- Fix indexing issue in export of consensus task labels.


​


Nov 10, 2025


**Changed:**


- Add imageHeaders in task series -[https://sdk.redbrickai.com/formats/index.html#redbrick.types.task.ImageHeaders](https://sdk.redbrickai.com/formats/index.html#redbrick.types.task.ImageHeaders)
- Drop support for python 3.9
- Revert changes made in SDK v2.28.14


​


Nov 5, 2025


**Changed:**


- Export` absolutePoint1` (top-left) and` absolutePoint2` (bottom-right) 2D points in bounding box labels.


​


Oct 15, 2025


**Changed:**


- Add` mask` property in segmentMap for non-NIfTI segmentation exports.


​


Oct 2, 2025


**Changed:**


- Add` TaskEventTypes.TASK_MOVED` to denote events where a task has been manually moved to a different stage.


​


Oct 1, 2025


**Changed:**


- Add` project.upload.archive_tasks()` for bulk archival of tasks.


​


Sep 10, 2025


**Changed:**


- Move changelog to[https://docs.redbrickai.com/changelogs/redbrick-sdk-changelog](https://docs.redbrickai.com/changelogs/redbrick-sdk-changelog)


​


Sep 9, 2025


**Changed:**


- Add` project.created_at` and` project.updated_at` properties.


​


Aug 21, 2025


**Changed:**


- Internal changes.


​


Aug 8, 2025


**Changed:**


- Add` org.delete_taxonomies()` for bulk delete of taxonomies.


​


Aug 8, 2025


**Changed:**


- Update` org.taxonomies()` to return complete list of taxonomies.
- Add` org.delete_projects()` for bulk delete of archived projects.


​


Aug 6, 2025


**Changed:**


- API keys will be exported as` API Key - {keyName}`


​


Aug 1, 2025


- Improve concurrency of file downloads


​


Jul 29, 2025


**Changed:**


- All exported label objects contain a` labelId` property, which can be directly used to create entity-level comments via` project.upload.create_comment()`


**Requires: redbrick@v1.5.3+**


​


Jul 3, 2025


Requires: **RedBrick v1.5.0+**


- Ability to add pins to global/entity comments:


```text
"comment"  : {
"text"  :   <  comment   text  >,
"pin"  ?:   {
// coordinates are in image-space
"pointX"  :   <  column  >,
"pointY"  :   <  row  >,
"pointZ"  :   <  slice   index  >,
"frameIndex"  ?:   <  frame   index   for   videos  >,
"volumeIndex"  ?:   <  series   index  >,
}
}


```


​


Jun 16, 2025


- Internal API changes


​


Jun 13, 2025


**📢 We’re Now on DeepWiki!**


redbrick-sdk is now live on[DeepWiki](https://deepwiki.com/redbrick-ai/redbrick-sdk) — with built-in, high-quality AI chat support. *Ask questions, get code examples, and explore docs with DeepWiki’s smart, conversational assistant — all powered by our latest documentation.*


​


Jun 6, 2025


- Enable webhook option


​


Jun 4, 2025


**Changed:**


- Updated nibabel to force numpy v2
- Fixed update webhook API


​


May 30, 2025


**Changed:**


- org.get_dataset() now returns RBDataset instead of Dict


​


May 30, 2025


**Changed:**


- Export.list_tasks() to return tasks currently in the given stage if no` search` filter is specified


​


May 28, 2025


**Added**


- DICOM Seg support in upload and export modules (` --dicom-seg` in CLI)


**Fixed**


- Stage name validation in send_tasks_to_stage


​


May 23, 2025


- Update docs


​


May 23, 2025


**Added**


- Create task comments -[https://sdk.redbrickai.com/sdk.html#redbrick.common.upload.Upload.create_comment](https://sdk.redbrickai.com/sdk.html#redbrick.common.upload.Upload.create_comment)
- Add review_comment while reviewing tasks -[https://sdk.redbrickai.com/sdk.html#redbrick.common.labeling.Labeling.put_tasks](https://sdk.redbrickai.com/sdk.html#redbrick.common.labeling.Labeling.put_tasks)


​


May 15, 2025


- Update ModelStage.Config


​


May 7, 2025


- Silently bypass empty mask error on upload


​


May 7, 2025


**Changed:**


- Added[org.archive_project()](https://sdk.redbrickai.com/sdk.html#redbrick.RBOrganization.archive_project) /[org.unarchive_project()](https://sdk.redbrickai.com/sdk.html#redbrick.RBOrganization.unarchive_project) to manage soft deletion.
- [org.delete_project()](https://sdk.redbrickai.com/sdk.html#redbrick.RBOrganization.delete_project) now performs a full hard delete operation.


​


May 1, 2025


**Changed:**


- Fix license check with tools like liccheck


**New Contributors**


- @marcus-wirtz-snkeos made their first contribution in[https://github.com/redbrick-ai/redbrick-sdk/pull/214](https://github.com/redbrick-ai/redbrick-sdk/pull/214)


​


Apr 29, 2025


- AltaDB Datasets


​


Apr 16, 2025


- Performance upgrade for semantic and binary mask exports


​


Mar 28, 2025


**Requires RedBrick: v1.4.0+**


**Added**


- [OrgMember.is_active: bool](https://sdk.redbrickai.com/sdk.html#redbrick.OrgMember)
- [Organization.Team.enable_members(member_ids: List\[str\]) -> None](https://sdk.redbrickai.com/sdk.html#redbrick.common.member.Team.enable_members)
- [Organization.Team.disable_members(member_ids: List\[str\]) -> None](https://sdk.redbrickai.com/sdk.html#redbrick.common.member.Team.disable_members) **Updated**
- [Organization.Team.list_members(active: bool = True) -> List\[OrgMember\]](https://sdk.redbrickai.com/sdk.html#redbrick.common.member.Team.list_members)
- [Workspace.update_datapoint_attributes(dp_id: str, attributes: Dict) -> None](https://sdk.redbrickai.com/sdk.html#redbrick.RBWorkspace.update_datapoint_attributes)
- [StorageMethod.AltaDB](https://sdk.redbrickai.com/sdk.html#redbrick.StorageMethod.AltaDB) is now a RedBrick integrated storage method **Removed**
- Organization.Team.remove_members(member_ids: List\[str\]) -> None


​


Mar 14, 2025


- Use default series names in file paths


​


Mar 14, 2025


- Add storageId to task list and export


​


Mar 4, 2025


- Fix MHD segmentation direction


​


Feb 27, 2025


- Add labels export deserialization


​


Feb 27, 2025


- org.team.remove_members


​


Feb 26, 2025


- Support for multiple assignees


​


Feb 26, 2025


- Fix org.team.list_invites()


​


Feb 26, 2025


​


Feb 18, 2025


**Requires RedBrick: v1.3.0+** **[Organization](https://sdk.redbrickai.com/sdk.html#redbrick.RBOrganization)**


- **[team](https://sdk.redbrickai.com/sdk.html#redbrick.common.member.Team)**
- [get_member(member_id: str) -> OrgMember](https://sdk.redbrickai.com/sdk.html#redbrick.common.member.Team.get_member)
- [list_members() -> List\[OrgMember\]](https://sdk.redbrickai.com/sdk.html#redbrick.common.member.Team.list_members)
- [remove_member(member_id: str) -> None](https://sdk.redbrickai.com/sdk.html#redbrick.common.member.Team.remove_member)
- [list_invites() -> List\[OrgInvite\]](https://sdk.redbrickai.com/sdk.html#redbrick.common.member.Team.list_invites)
- [invite_user(invitation: OrgInvite) -> OrgInvite](https://sdk.redbrickai.com/sdk.html#redbrick.common.member.Team.invite_user)
- [revoke_invitation(invitation: OrgInvite) -> None](https://sdk.redbrickai.com/sdk.html#redbrick.common.member.Team.revoke_invitation)
- **[storage](https://sdk.redbrickai.com/sdk.html#redbrick.common.storage.Storage)**
- [get_storage(storage_id: str) -> StorageProvider](https://sdk.redbrickai.com/sdk.html#redbrick.common.storage.Storage.get_storage)
- [list_storages() -> List\[StorageProvider\]](https://sdk.redbrickai.com/sdk.html#redbrick.common.storage.Storage.list_storages)
- [create_storage(storage: StorageProvider) -> StorageProvider](https://sdk.redbrickai.com/sdk.html#redbrick.common.storage.Storage.create_storage)
- [update_storage(storage_id: str, details: StorageProvider.Details) -> StorageProvider](https://sdk.redbrickai.com/sdk.html#redbrick.common.storage.Storage.update_storage)
- [delete_storage(storage_id: str) -> bool](https://sdk.redbrickai.com/sdk.html#redbrick.common.storage.Storage.delete_storage)
- [verify_storage(storage_id: str, path: str) -> bool](https://sdk.redbrickai.com/sdk.html#redbrick.common.storage.Storage.verify_storage) **[Workspace](https://sdk.redbrickai.com/sdk.html#redbrick.RBWorkspace)**
- [create_datapoints(storage_id: str, points: List\[InputTask\], concurrency: int = 50) -> List\[Dict\]](https://sdk.redbrickai.com/sdk.html#redbrick.RBWorkspace.create_datapoints)
- [delete_datapoints(dp_ids: List\[str\], concurrency: int = 50) -> bool](https://sdk.redbrickai.com/sdk.html#redbrick.RBWorkspace.delete_datapoints)
- [update_datapoints_metadata(storage_id: str, points: List\[Dict\]) -> None](https://sdk.redbrickai.com/sdk.html#redbrick.RBWorkspace.update_datapoints_metadata)
- [add_datapoints_to_projects(project_ids: List\[str\], dp_ids: List\[str\], is_ground_truth: bool = False) -> None](https://sdk.redbrickai.com/sdk.html#redbrick.RBWorkspace.add_datapoints_to_projects) **[Project](https://sdk.redbrickai.com/sdk.html#redbrick.RBProject)**
- **[workforce](https://sdk.redbrickai.com/sdk.html#redbrick.common.member.Workforce)**
- [get_member(member_id: str) -> ProjectMember](https://sdk.redbrickai.com/sdk.html#redbrick.common.member.Workforce.get_member)
- [list_members() -> List\[ProjectMember\]](https://sdk.redbrickai.com/sdk.html#redbrick.common.member.Workforce.list_members)
- [add_members(members: List\[ProjectMember\]) -> List\[ProjectMember\]](https://sdk.redbrickai.com/sdk.html#redbrick.common.member.Workforce.add_members)
- [update_members(members: List\[ProjectMember\]) -> List\[ProjectMember\]](https://sdk.redbrickai.com/sdk.html#redbrick.common.member.Workforce.update_members)
- [remove_members(member_ids: List\[str\]) -> None](https://sdk.redbrickai.com/sdk.html#redbrick.common.member.Workforce.remove_members)


​


Jan 24, 2025


- Unified control for client session


​


Jan 17, 2025


- Update consensus stage names


​


Jan 10, 2025


- Add status in task export


​


Jan 10, 2025


- Compress MHD segmentations


​


Dec 23, 2024


**Added**


- [project.upload.send_tasks_to_stage](https://sdk.redbrickai.com/sdk.html#redbrick.upload.Upload.send_tasks_to_stage)


​


Dec 13, 2024


**Compatible with redbrick@v1.2.0+**


- **Dropped Python 3.8 support** -[https://endoflife.date/python](https://endoflife.date/python)
- Datapoint classification:[https://sdk.redbrickai.com/formats/index.html#redbrick.types.task.OutputTask.datapointClassification](https://sdk.redbrickai.com/formats/index.html#redbrick.types.task.OutputTask.datapointClassification)
- MHD segmentations:
- Upload:[https://sdk.redbrickai.com/sdk.html#redbrick.upload.Upload.create_datapoints](https://sdk.redbrickai.com/sdk.html#redbrick.upload.Upload.create_datapoints) &[https://sdk.redbrickai.com/sdk.html#redbrick.upload.Upload.update_tasks_labels](https://sdk.redbrickai.com/sdk.html#redbrick.upload.Upload.update_tasks_labels)
- Label:[https://sdk.redbrickai.com/sdk.html#redbrick.labeling.Labeling.put_tasks](https://sdk.redbrickai.com/sdk.html#redbrick.labeling.Labeling.put_tasks)
- Export:[https://sdk.redbrickai.com/sdk.html#redbrick.export.Export.export_tasks](https://sdk.redbrickai.com/sdk.html#redbrick.export.Export.export_tasks)
- Also added` prune_segmentations` support in Label & Upload labels methods, which does a two-way pruning of uncommon instances in segmentMap and segmentations


**Changed:**


- Upload attributes along with the datapoints
- Improve processing time of process_nifti_upload
- Fix bug in process_nifti_upload
- fix input types
- (v2.19.8) Fixes #197
- Make pydicom dep more flexible
- Download AltaDB series while exporting a task
- export seriesFrameIndex only when it has a non null value
- Improve speed of binary mask exports


​


Nov 16, 2024


Fixes:


- [Incorrect segmentation masks uploaded when multiple series and overlaps are present #197](https://github.com/redbrick-ai/redbrick-sdk/issues/197) proposed by @anaoum


​


Nov 13, 2024


- Fix workspace datapoint update query


​


Nov 9, 2024


- Fix bug in segmentations upload


​


Nov 9, 2024


**Changed:**


- Improve processing time of process_nifti_upload


**New Contributors**


- 🎉 @anaoum made their first contribution in[https://github.com/redbrick-ai/redbrick-sdk/pull/192](https://github.com/redbrick-ai/redbrick-sdk/pull/192)


​


Oct 30, 2024


- Fixes segmentation uploads to azure containers.
- Fixes length measurement labels exports.
- Adds more model sub types to CT Segmentator.


​


Oct 14, 2024


**Changed:**


- Add datapoint classification to export
- fix(redbrick/utils/rb_dicom_utils.py): Fix datapoint exports using SDK


​


Oct 9, 2024


- fix docker build dependencies


​


Oct 9, 2024


- Python 3.13


​


Oct 9, 2024


- Add python 3.13 support


​


Sep 28, 2024


- allow mixed formats task updates


​


Sep 28, 2024


- Patch an edge case in export of half-split data


​


Sep 27, 2024


- remove “test” from updated series name


​


Sep 27, 2024


- invalidate series items during update


​


Sep 26, 2024


**Changed:**


- remove gzipping on upload
- Fields in label computed on label editor to be optional


​


Sep 24, 2024


**Changed:**


- SDK changes for read only labels
- Update metadata along for tasks


​


Sep 12, 2024


- Fix consensus index issue


​


Sep 6, 2024


**Changed:**


- add segmentMap\[instanceId\].overlappingGroups in export ([docs](https://sdk.redbrickai.com/formats/annotations.html#redbrick.types.task.CommonLabelProps.overlappingGroups) )


​


Sep 3, 2024


**Changed:**


- Set Consensus Settings during or after project creation


​


Aug 29, 2024


- [https://sdk.redbrickai.com/formats/index.html#redbrick.types.task.Series.transforms](https://sdk.redbrickai.com/formats/index.html#redbrick.types.task.Series.transforms)
- [https://sdk.redbrickai.com/formats/annotations.html#redbrick.types.task.CommonLabelProps.group](https://sdk.redbrickai.com/formats/annotations.html#redbrick.types.task.CommonLabelProps.group)


(requires RedBrick@v1.1.2)


​


Aug 9, 2024


**Added**


- [redbrick.get_org_from_profile(\[profile\])](https://sdk.redbrickai.com/sdk.html#redbrick.get_org_from_profile)
- [redbrick.get_project_from_profile(\[project_id\], \[profile\])](https://sdk.redbrickai.com/sdk.html#redbrick.get_project_from_profile)
- [org.delete_project(project_id)](https://sdk.redbrickai.com/sdk.html#redbrick.organization.RBOrganization.delete_project)
- [org.delete_taxonomy(\[name\], \[tax_id\])](https://sdk.redbrickai.com/sdk.html#redbrick.organization.RBOrganization.delete_taxonomy)


​


Jul 4, 2024


**Changed:**


- project.settings.task_duplication
- SDK Support for Local and External HeatMaps


**New Contributors**


- @AvanishCodes made their first contribution in[https://github.com/redbrick-ai/redbrick-sdk/pull/178](https://github.com/redbrick-ai/redbrick-sdk/pull/178)


​


Jun 13, 2024


**Changed:**


- SDK Support for Local and External HeatMaps


**New Contributors**


​


May 27, 2024


- [handle overlapping segmentations in rt-struct exports](https://github.com/redbrick-ai/redbrick-sdk/commit/674ffe20682e29a2caa16f04f6df56a55b44dd19)
- [use asyncio.run instead of asyncio.get_event_loop](https://github.com/redbrick-ai/redbrick-sdk/commit/be955f67d18b171330a11dd66ad3a7695b0f5745)


​


May 23, 2024


Support for custom application[logging level](https://docs.python.org/3/library/logging.html#logging-levels) using environment variable` REDBRICK_SDK_LOG_LEVEL` or through` redbrick.config.log_level` attribute.


For example, in order to suppress all INFO and WARNING messages and only log for ERROR:


- ` REDBRICK_SDK_LOG_LEVEL=40 redbrick clone ...`
-


```text


```


$ python


> > > import redbrick redbrick.config.log_level = 40 project = redbrick.get_project(…)


​


May 21, 2024


Update deps


​


Apr 24, 2024


- Fix consensus segmentation export with empty base


​


Apr 11, 2024


- Boost CT Segmentator


​


Apr 9, 2024


- Minor patch


​


Apr 9, 2024


**Changed:**


- Now available as a Docker image -[https://hub.docker.com/r/redbrickai/redbrick-sdk](https://hub.docker.com/r/redbrickai/redbrick-sdk)
- Project creation improvements
- Project Webhooks
- Update task labels at any point in pipeline
- Workspaces support
- PNG binary mask upload


​


Mar 26, 2024


- Export only final labels


​


Mar 26, 2024


- Improve task export performance


​


Feb 22, 2024


- Trigger readthedocs build


​


Feb 22, 2024


- Docs improvements


​


Feb 21, 2024


- More docs improvements


​


Feb 20, 2024


- Update docs links


​


Feb 20, 2024


- Fix docs build


​


Feb 20, 2024


- [update docstrings and readthedocs for formats](https://github.com/redbrick-ai/redbrick-sdk/commit/f2bedc8918ddd24398a8d0f042bf3123d116375c) by @shivam124081 🎉


​


Feb 15, 2024


- Add taxonomy types


​


Feb 8, 2024


- Fix label export index


​


Feb 8, 2024


- Global redbrick.config


​


Feb 5, 2024


- Add types for upload and export


​


Jan 24, 2024


- Export improvements


​


Jan 19, 2024


- Improved support for RT-Struct


​


Jan 18, 2024


- context.config.verify_ssl for upload/download files


​


Jan 18, 2024


- Add context.config


​


Jan 18, 2024


- Check global segmentations during upload


​


Jan 12, 2024


- TASK_CORRECTED event


​


Jan 10, 2024


- Advanced project creation
- Improved labeling metrics
- Import DICOM RT-Struct segmentations
- Use` rt_struct=True` in` project.upload.create_datapoints()` OR` --rt-struct` in` redbrick upload`
- The keys of` segmentMap` will be ROI names instead of instance ids ->` {"Segment_1": "category"}`


​


Dec 20, 2023


- Update Dependencies


​


Dec 19, 2023


**Changed:**


- Chima/maj 2537 unit tests of sdkcli


​


Dec 6, 2023


- updated list_tasks` assignee` format


```text
[{
"taskId"  :   str  ,
"name"  :   str  ,
"createdAt"  :   str  ,
"updatedAt"  :   str  ,
"currentStageName"  :   str  ,
"createdBy"  ?:   {  "userId"  :   str  ,   "email"  :   str  },
"priority"  ?:   float  ([  0  ,   1  ]),
"metaData"  ?:   dict  ,
"series"  ?:   [{  "name"  ?:   str  ,   "metaData"  ?:   dict  }],
"assignees"  ?:   [{
"user"  :   str  ,
"status"  :   str  ,
"assignedAt"  :   datetime  ,
"lastSavedAt"  ?:   datetime  ,
"completedAt"  ?:   datetime  ,
"timeSpentMs"  ?:   float  ,
}]
}]


```


- uploaded dicom files are stored in raw format


​


Dec 5, 2023


[https://docs.redbrickai.com/python-sdk/format-reference#taxonomy-object](https://docs.redbrickai.com/python-sdk/format-reference#taxonomy-object)


​


Dec 5, 2023


- Add .dicom support


​


Nov 29, 2023


**Changed:**


- Feat/add tests
- Addresses #173


**New Contributors**


- @cAtaman made their first contribution in[https://github.com/redbrick-ai/redbrick-sdk/pull/170](https://github.com/redbrick-ai/redbrick-sdk/pull/170)


​


Nov 15, 2023


- Docs updates


​


Nov 3, 2023


- Bug fix (project clone)


​


Nov 3, 2023


- Logging improvements


​


Oct 26, 2023


- Add top-level series to consensus tasks


​


Oct 20, 2023


**CLI (redbrick export)**


- ` --without-masks` : Exports only tasks JSON without downloading any segmentation masks. Note: This is not recommended for tasks with overlapping labels.
- ` --semantic` : Whether to export all segmentations as semantic_mask. This will create one segmentation file per class. If this is set to True and a task has multiple instances per class, then attributes belonging to each instance will not be exported.
- ` --binary-mask` : Whether to export all segmentations as binary masks. This will create one segmentation file per instance.
- ` --single-mask` : Whether to export all segmentations in a single file. Binary mask will be considered if both binary_mask and single_mask are set.


**SDK (project.export.export_tasks)**


- ` without_masks: bool = False` : Same as` --without-masks`
- ` without_json: bool = False` : Doesn’t create the tasks JSON file.
- ` semantic_mask: bool = False` : Same as` --semantic`
- ` binary_mask: Optional\[bool\] = None` : True is same as` --binary-mask` and False is same as` --single-mask`


​


Oct 17, 2023


- Fix non-binary semantic class merging


​


Oct 16, 2023


- SDK:[https://redbrick-sdk.readthedocs.io/en/stable/sdk.html#redbrick.export.Export.export_tasks](https://redbrick-sdk.readthedocs.io/en/stable/sdk.html#redbrick.export.Export.export_tasks)


```text
semantic_mask (bool = False) – Whether to export all segmentations as semantic_mask. This will create one instance per class. If this is set to True and a task has multiple instances per class, then attributes belonging to each instance will not be exported.


binary_mask (Optional[bool] = None) – Whether to export all segmentations as binary masks. This will create one segmentation file per instance. If this is set to None and a task has overlapping labels, then binary_mask option will be True for that particular task.


If both semantic_mask and binary_mask options are True, then one binary mask will be generated per class.


```


- CLI:` redbrick export`


​


Oct 9, 2023


- Fix typo


​


Oct 9, 2023


Added` RBProject.settings` member to manage project-level settings ([https://redbrick-sdk.readthedocs.io/en/stable/sdk.html#redbrick.settings.Settings](https://redbrick-sdk.readthedocs.io/en/stable/sdk.html#redbrick.settings.Settings) )


​


Oct 4, 2023


- Export cuboid labels


​


Sep 28, 2023


**Fixes**


- Export groundtruth (c1431b9b519322c7aa0c1ff84429c987fb477c0a)
- Export consensus items (ea631b108c01370e2a78cb642d8cae954c7d19fc)
- Create project taxonomy (ae113dd77f2991e24af0e53179f31ceff9477185)


​


Sep 20, 2023


- Fix taxonomy format docs link


​


Sep 18, 2023


- Fix taxonomy format docs link


​


Sep 13, 2023


- Fix export series mapping


​


Aug 22, 2023


- Add sec. user ids to consensus export


​


Aug 18, 2023


- Fix ‘redbrick report’


​


Aug 18, 2023


**Fixes:**


- Internal server error on Export.get_active_time when task_id provided (#166)


​


Aug 15, 2023


**Fixes:**


- Unable to programatically determine the outcome of a create/update taxonomy operation (#165)


​


Aug 11, 2023


- Type distribution


​


Aug 11, 2023


**Added**[PEP-561](https://peps.python.org/pep-0561/) type distribution (#164) **Removed**


- Implicit version checking (#163)


For explicit version check:


- SDK:` redbrick.version()`
- CLI:` redbrick -v`


​


Aug 8, 2023


- Fix segmentation export re-map issue


​


Aug 2, 2023


- Import NRRD images


​


Jul 28, 2023


**Added:**


- Support to export segmentations in DICOM RT-Struct format using` --rt-struct` in redbrick export /` rt_struct: bool` in Export.export_tasks
- Export.get_active_time


```text
def get_active_time(
self,
*,
stage_name: str,
task_id: Optional[str] = None,
concurrency: int = 100,
) -> Iterator[Dict]:
"""Get active time spent on tasks for labeling/reviewing.
Parameters
-----------
stage_name: str
Stage for which to return the time info.


task_id: Optional[str] = None
If set, will return info for the given task in the given stage.


concurrency: int = 100
Request batch size.


Returns
-----------
Iterator[Dict]
>>> [{
"orgId": string,
"projectId": string,
"stageName": string,
"taskId": string,
"completedBy": string,
"timeSpent": number,  # In milliseconds
"completedAt": datetime,
"cycle": number  # Task cycle
}]
"""


```


- Export.list_tasks added param


```text
completed_at: Optional[Tuple[Optional[float], Optional[float]]] = None
If present, will return tasks that were completed in the given time range.
The tuple contains the `from` and `to` timestamps respectively.


```


**Changed:**


- Renamed RBOrganization.create_taxonomy_new -> RBOrganization.create_taxonomy
- Export.export_tasks now returns Iterator\[Dict\]
- Export.list_tasks now returns Iterator\[Dict\]
- Export.get_task_events now returns Iterator\[Dict\]
- Unified all` user` entities across exports to email of user.


**Removed:**


- Export.search_tasks
- Export.redbrick_nifti
- Labeling.get_tasks
- Labeling.get_task_queue
- Labeling.assign_tasks (removed current_user param)


**Changed:**


- Support DICOM RT-Struct segmentations export
- Export enhancements
- fix super truth export


​


Jul 25, 2023


**Changed:**


- fix super truth export


​


Jul 18, 2023


**Added:**


- Export.get_active_time


```text
def get_active_time(
self,
*,
stage_name: str,
task_id: Optional[str] = None,
concurrency: int = 100,
) -> Iterator[Dict]:
"""Get active time spent on tasks for labeling/reviewing.
Parameters
-----------
stage_name: str
Stage for which to return the time info.


task_id: Optional[str] = None
If set, will return info for the given task in the given stage.


concurrency: int = 100
Request batch size.


Returns
-----------
Iterator[Dict]
>>> [{
"orgId": string,
"projectId": string,
"stageName": string,
"taskId": string,
"completedBy": string,
"timeSpent": number,  # In milliseconds
"completedAt": datetime,
"cycle": number  # Task cycle
}]
"""


```


- Export.list_tasks added param


```text
completed_at: Optional[Tuple[Optional[float], Optional[float]]] = None
If present, will return tasks that were completed in the given time range.
The tuple contains the `from` and `to` timestamps respectively.


```


**Changed:**


- Renamed RBOrganization.create_taxonomy_new -> RBOrganization.create_taxonomy
- Export.export_tasks now returns Iterator\[Dict\]
- Export.list_tasks now returns Iterator\[Dict\]
- Export.get_task_events now returns Iterator\[Dict\]
- Unified all` user` entities across exports to email of user.


**Removed:**


- Export.search_tasks
- Export.redbrick_nifti
- Labeling.get_tasks
- Labeling.get_task_queue
- Labeling.assign_tasks (removed current_user param)


​


Jul 14, 2023


**Changed:**


- Support DICOM RT-Struct segmentations export


​


Jul 12, 2023


**Changed:**


- Fix image export for consensus tasks


​


Jul 5, 2023


**Changed:**


- (v2.12.8) - Add superTruth in consensus export


​


Jul 3, 2023


**Changed:**


- (v2.12.7) - Add extra info in Export.list_tasks


```text
[{
"taskId": str,
"name": str,
"createdAt": str,
"currentStageName": str,
"createdBy"?: {"userId": str, "email": str},
"priority"?: float([0, 1]),
"metaData"?: dict,
"series"?: [{"name"?: str, "metaData"?: dict}],
"assignees"?: [{"userId": str, "email": str}]
}]


```


​


Jun 26, 2023


**Changed:**


- Upload.update_tasks_priority (v2.12.6)
- release v2.12.6


​


Jun 8, 2023


**Changed:**


- Fix readthedocs (v2.12.5)


​


Jun 8, 2023


**Changed:**


- Fix readthedocs (v2.12.4)


​


Jun 8, 2023


**Changed:**


- Fix GH Actions (v2.12.3)


​


Jun 8, 2023


**Changed:**


- Fix and update docs (v2.12.2)


​


Jun 8, 2023


**Changed:**


- Update docs - Export.list_tasks


​


May 31, 2023


**Changed:**


- Nested taxonomy and hints
- Labeling.update_tasks_priority + Series level meta data
- Misc improvements


​


May 24, 2023


**Changed:**


- Labeling.update_tasks_priority + Series level meta data


​


May 18, 2023


**Changed:**


- Nested taxonomy and hints


​


May 2, 2023


**Changed:**


- Extensionless dicom files upload
- Update docs for enums
- Restrict project creation to use Taxonomy V2


​


Apr 18, 2023


**Changed:**


- v2.11.1 - Fix project creation


​


Apr 11, 2023


**Changed:**


- Workspace, tasks filter and major improvements
- alpha release
- list_tasks fix
- list_tasks fix
- release v2.11.0


​


Apr 6, 2023


**Changed:**


- list_tasks fix


​


Apr 6, 2023


**Changed:**


- list_tasks fix


​


Apr 5, 2023


**Changed:**


- Workspace, tasks filter and major improvements
- alpha release


​


Mar 20, 2023


**Changed:**


- improve handling file upload/download concurrency


​


Mar 13, 2023


**Changed:**


- json handle dcm upload without extension


​


Mar 2, 2023


**Changed:**


- Optimize non-segmentation task export


​


Feb 23, 2023


**Changed:**


- v2.10.1 update packages


​


Feb 21, 2023


**Added**


- Allow updating task items
- Support put labeling tasks with existing labels
- Add from_timestamp in task report export
- Export task report for all labeling cycles **Fixed**
- Memory management in nifti import/export processing
- Fast export for single task
- Export frameindex for DCM videos **Deprecated**
- Discontinue to_timestamp in task export


​


Feb 13, 2023


**Changed:**


- v2.9.1 - Instance Classification


​


Jan 6, 2023


**Changed:**


- Support Python 3.10 and 3.11


​


Jan 5, 2023


**Changed:**


- Labeling interface changes


---


**Added**


1. ` Labeling.assign_tasks`


- Assign multiple tasks at once in the stage that they are currently in **Changed:**


1. ` Labeling.put_tasks`


- All arguments except` stage_name` and` tasks` are keyword-only.
- ` finalize` boolean indicates whether to submit or save the task as draft in the label stage
- ` review_result` boolean indicates whether to accept or reject all tasks in the review stage


1. ` Labeling.get_task_queue`


- Get tasks in users’ queue based on their email **Removed**


1. ` Labeling.assign_task`


- In favour of` Labeling.assign_tasks`


​


Dec 29, 2022


**Changed:**


- Update readthedocs theme, and docstrings.


​


Dec 21, 2022


**Changed:**


- add ellipse and measurement stats to export


​


Dec 13, 2022


**Changed:**


- CLI export images ‘—dicom-to-nifti’
- change task events report userId to email


​


Nov 26, 2022


**Changed:**


- Remove rasterio dependency


​


Nov 18, 2022


**Changed:**


- Generate task events report + Deprecate legacy export and import
- Add` concurrency` in upload
- better handle segmentation files with volume index without a label instance


---


- redbrick_nifti :: return-type labels
- update attribute format docs
- report command to fetch task events report
- deprecate legacy projects data import/export
- Add option for concurrency (Default: 50) in data upload
- Improve error feedback
- Compress all request and response data
- single series segmentation labels


​


Nov 2, 2022


**Changed:**


- RB-1175 : Deprecate active learning
- Task Pre-Assignment
- fix nifti mask color for taxonomy v2


​


Oct 20, 2022


**Changed:**


- project members list
- sdk support for new tax labels


​


Oct 5, 2022


**Changed:**


- NIfTI png export, New Taxonomy


​


Oct 3, 2022


- Properly bump version


​


Oct 3, 2022


**Changed:**


- Improvements to task import/export
- fix segmentation input type


​


Sep 13, 2022


**Changed:**


- Fix export series index issue + Disable creating legacy project types


​


Aug 22, 2022


**Changed:**


- Drop Python3.6 support
- redesign task format
- Consensus Tasks


​


Aug 8, 2022


**Changed:**


- Drop Python3.6 support
- redesign task format


​


Jun 28, 2022


**Changed:**


- simplify segmentation upload
- deleteTasks and new task object format


​


Jun 21, 2022


**Changed:**


- simplify segmentation upload


​


Jun 10, 2022


**Changed:**


- typo fix and support local paths when labelsPath is a list


​


May 25, 2022


**Changed:**


- fix no labels export issue


​


May 24, 2022


**Changed:**


- nifti - import/export overlapping labels


​


Apr 26, 2022


**Changed:**


- fix readthedocs import issue and build


​


Apr 26, 2022


​


Apr 25, 2022


**Changed:**


- support label from external storage on upload


​


Apr 1, 2022


**Changed:**


- set label storage


​


Mar 31, 2022


**Changed:**


- active learning + mask taxonomy colour + other improvements
- upgrade pillow==9.0.1 for python > 3.6
- cli export by stage


​


Mar 18, 2022


**Changed:**


- fix data paginator iteration


​


Mar 17, 2022


**Changed:**


- add ima to supported dicom extensions
- org.create_taxonomy + .nii, .nii.gz DICOM image upload


​


Mar 5, 2022


**Changed:**


- task labeling time information per org


​


Feb 26, 2022


**Changed:**


- add labeling methods to docs


​


Feb 26, 2022


**Changed:**


- resurrect notebook event loop handler


​


Feb 23, 2022


**Changed:**


- add methods to docs


​


Feb 23, 2022


**Changed:**


- add task search and taxonomies methods


​


Feb 16, 2022


**Changed:**


- sdk dicom upload with labels
- support custom storage id in upload + lint fixes
- Bump pillow from 8.3.2 to 9.0.0
- move task to start api
- pillow to 8.4.0 for python 3.6 support
- minor changes
- fix groundtruth tasks export
- export input labels


​


Feb 4, 2022


**Changed:**


- skip task in export for all errors
- slicer: first draft
- remove slicer
- fix mask labels data type


​


Jan 27, 2022


**Changed:**


- skip task in export for all errors
- slicer: first draft


​


Jan 7, 2022


**Changed:**


- add cli export test
- segmentation export
- redbrick_nifti export
- workflow tests
- prep v1
- error handling and improvements
- CLI NIfTI export
- better file path and naming sync
- CLI upload
- polishing upload


​


Dec 15, 2021


**Changed:**


- add exists_okay check to project creation
- automatically attach nest_asyncio when there is a running event loop
- make url an optional arg in main methods
- get task id for uploads
- CLI - I
- active learning info get only unassigned tasks
- cli export caching
- fix:: check dpId existence in cached dimensions
- minor improvement for cache validity
- Hotfix/segmentation export


​


Dec 10, 2021


Bug fix for segmentation export.


​


Dec 3, 2021


Speed up to direct upload with async io.


​


Dec 3, 2021


Added support for image and video direct upload.


​


Dec 1, 2021


- fix client error management


​


Dec 1, 2021


**Changed:**


- use latestTaskData and labelsData for export


**New Contributors**


- @pritamrungta made their first contribution in[https://github.com/redbrick-ai/redbrick-sdk/pull/50](https://github.com/redbrick-ai/redbrick-sdk/pull/50)


​


Nov 25, 2021


- New method for checking processing state of training.


​


Nov 19, 2021


- bug fix: upload of segmentation masks with single class fixed.


​


Nov 19, 2021


- Connection request limiting and session re-use


​


Nov 18, 2021


- Allow get_learning_info call after start_processing


​


Nov 16, 2021


Use environment markers in setup.py.


​


Nov 16, 2021


- Version bump for setup.py modifications.


​


Nov 16, 2021


- Rasterio has been removed as a dependency for windows machines. To upload/export png masks, windows users will have to manually install rasterio.
- Create datapoint with masks interface has been updated.


​


Nov 15, 2021


- datapoint.json file now uses task_id instead of dp_id.


​


Nov 12, 2021


0.6.5 - add is_ground_truth flag to upload


​


Nov 11, 2021


- bug fix: correctly handle 0 label tasks in segmentation export.


​


Nov 11, 2021


- Variable naming changes.


​


Nov 11, 2021


- Bug fix: If RedBrick AI polygon segmentation holes/regions have only 2 vertices, ignore for PNG mask export.
- Feature: Fill holes on segmentation mask PNG export.


​


Nov 5, 2021


**Changed:**


- Active learning upgrade
- Active learning v2


​


Nov 2, 2021


Include datapoint map in png export


​


Nov 2, 2021


Updating matplotlib package.


​


Nov 2, 2021


Support for PNG export format for segmentation projects.


​


Oct 21, 2021


- minor bug fix for coco bbox area calculation


​


Oct 14, 2021


- Handle incorrect api key error


​


Oct 8, 2021


​


Oct 8, 2021


​


Oct 1, 2021


Active learning create project


​


Sep 29, 2021


Assign task to labeler


​


Sep 27, 2021


Single task export


​


Sep 15, 2021


ocr value added to query


​


Aug 11, 2021


​


Aug 5, 2021


​


Aug 5, 2021


​


Jul 22, 2021


​


Jul 22, 2021


​


Jul 21, 2021


​


Jul 21, 2021


​


Jul 21, 2021


​


Jul 21, 2021


​


Jul 17, 2021


Add coco format


​


Jul 15, 2021


​


Jul 15, 2021


​


Jul 14, 2021


Small bug fixes to export


​


Jul 12, 2021


​


Jul 12, 2021


​


Jul 9, 2021


​


Jul 9, 2021


​


Feb 3, 2021
