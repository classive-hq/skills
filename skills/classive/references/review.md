# Review existing materials

Read this when the user wants accuracy, clarity, audience suitability or coverage checked in an existing source. Reviewing a document is distinct from creating a lesson based on it.

## Define the review

Select ready owned material versions with [materials](materials.md), and retain the intended audience and any learning goals. A review prompt is an optional focus; omit it for the standard review. Do not turn an unrelated authoring request into a review or add teaching output/voice/duration fields to a review payload.

For a whole-document request, inspect extraction completeness and all retained sections first. A book with unreadable or omitted extracted portions cannot be reported as completely reviewed. Preserve any source-only research policy; explicitly selected materials are not permission to search outside them.

## Quote, start and follow

| Step | MCP | REST |
| --- | --- | --- |
| Inspect source/version/coverage | `get_material` | `GET /v1/materials/{id}` |
| Obtain complete review quote | `quote_job` using the current `review` schema | `POST /v1/jobs/quote` |
| Accept the approved scope | `start_job` with quote acceptance/idempotency | `POST /v1/jobs` with matching review input/quote; idempotency header |
| Poll review state | `get_job` | `GET /v1/jobs/{id}` |
| Read result manifest | `get_package` when available | Job results/package endpoints |
| Download the review JSON | REST or dashboard | Returned authenticated file URL |

Read [create-job fields](https://classive.app/docs/api/create-job.md), [review result format](https://classive.app/docs/outputs.md) and [long-document review](https://classive.app/docs/how-to/long-materials.md). Review scope and source length affect the quote. Do not substitute a tiny selected excerpt when the user requested whole-document analysis.

## Present the actual findings

On completion, identify the source/version and scope reviewed. Preserve issue severity, quoted evidence, corrections and locators as returned. Distinguish a model's finding from a separately established fact when interpreting the report.

For exhaustive review, compare reviewed/unreviewed section IDs with the retained extraction map. Synthesis of checked section reports is not another complete read of the originals. Report partial/unreviewed scope explicitly; do not describe retrieval hits or a visited-section count as proof that every possible issue was detected.

No lesson files are expected from a review job. If the user then wants corrected educational resources, that is a separately scoped generation or revision workflow with its own quote. Use [jobs and recovery](jobs.md) on failure rather than purchasing a repeated review automatically.
