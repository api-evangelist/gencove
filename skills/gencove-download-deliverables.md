---
name: Download Gencove analysis deliverables
description: Find completed samples in a project and retrieve their deliverable files, including the project's merged VCF.
api: openapi/gencove-openapi-original.json
operations: [projects_list, project_samples_list, samples_retrieve, project_merge_vcfs_retrieve, file_types_list]
---

# Download Gencove analysis deliverables

Use the Gencove Back API (`https://api.gencove.com/api/v2/`) with
`Authorization: Api-Key <key>`.

## Steps

1. **Locate the project.** Call `projects_list` and select the project id.
2. **Find completed samples.** Call `project_samples_list` and filter for samples
   whose `last_status` indicates analysis completed.
3. **List deliverable files.** Call `samples_retrieve` for a sample; its `files`
   array carries each deliverable's `file_type` and download URL. Use
   `file_types_list` to understand available file types.
4. **Get the merged VCF.** For a project-level joint call set, call
   `project_merge_vcfs_retrieve` to fetch the merged VCF deliverable.

## Rules

- Download URLs are time-limited presigned links; fetch promptly.
- Respect pagination (`meta.next`) when listing samples.
- A 404 on a deliverable means analysis has not finished or the file type was not
  produced for that pipeline.
