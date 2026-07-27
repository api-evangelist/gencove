---
name: Analyze samples through a Gencove project
description: Create a project bound to a pipeline capability, upload FASTQ data, submit samples for analysis, and poll for completion.
api: openapi/gencove-openapi-original.json
operations: [pipeline_capabilities_list, projects_create, upload_credentials_create, project_samples_create, project_samples_list, samples_retrieve]
---

# Analyze samples through a Gencove project

Use the Gencove Back API (`https://api.gencove.com/api/v2/`). Authenticate on every
request with `Authorization: Api-Key <key>` (or `Authorization: Bearer <jwt>`).

## Steps

1. **Pick a pipeline capability.** Call `pipeline_capabilities_list` and choose the
   capability ID for the analysis you want to run.
2. **Create a project.** Call `projects_create` with a name and the chosen pipeline
   capability ID. Optionally set `webhook_url` so Gencove notifies you on completion.
3. **Get upload credentials.** Call `upload_credentials_create` to obtain presigned
   upload targets for your FASTQ files, then upload the files to them.
4. **Attach samples.** Call `project_samples_create` to register the uploaded
   FASTQs as samples in the project (each sample carries your `client_id`).
5. **Track status.** Poll `project_samples_list` (or `samples_retrieve` for one
   sample) and read `last_status`. Prefer the `analysis_complete_v2` webhook over
   polling when you set a `webhook_url`.

## Rules

- Pagination is limit/offset; read `results` and follow `meta.next` until null.
- Auth failures return 401; permission failures 403; unknown ids 404 (see
  `errors/gencove-error-codes.yml`).
- There is no idempotency-key header — do not blindly retry create calls.
