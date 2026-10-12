# Curricula and courses

Read this for a recommended learning sequence, an editable course plan or generation of all/selected lessons. A plan purchase and lesson generation are separate scopes. Written-only courses are supported by selecting written outputs.

## Workflow and transports

| Step | MCP action to discover | REST route |
| --- | --- | --- |
| Get starting-point recommendations | `recommend_curricula` | `POST /v1/curricula/recommendations` |
| Import the user's existing plan | Use REST/dashboard | `POST /v1/curricula/import` |
| Quote AI planning and missing preparation | `quote_curriculum` | `POST /v1/curricula/quote` |
| Accept planning quote | `start_curriculum` | `POST /v1/curricula` |
| Read current/specific version | `get_curriculum` | `GET /v1/curricula/{id}` with optional version |
| Save an edited draft | `revise_curriculum` | `POST /v1/curricula/{id}/versions` |
| Approve a specific version | `approve_curriculum` | `POST /v1/curricula/{id}/approve` |
| Quote all or selected lessons | `quote_course` | `POST /v1/curricula/{id}/quote` |
| Accept generation quote | `start_course` | `POST /v1/curricula/{id}/generate` |
| Follow course and child lessons | `get_job` | `GET /v1/jobs/{id}` |

Discover schemas and read [Curriculum API](https://classive.app/docs/api/curricula.md) and the [full-course tutorial](https://classive.app/docs/tutorials/full-course.md). Recommendations are Classive starting points, not accredited curricula or a paid planning start.

## Plan first

Preserve audience, goals, prerequisites, selected source versions, requested lesson count/duration and research choices. An official standard can have a name/edition/source URL; attach its source document when exact content matters. A name or URL does not establish verified alignment.

For AI planning, show the complete planning quote and confirm authorization for that scope and maximum credits. Missing source summaries can be included in the quote; do not assume they are free. Start with its quote ID and stable idempotency key, then poll the curriculum ID until a draft or failure is returned. A draft plan is not a generated course.

For an existing customer plan, use the documented import shape and validate stable lesson IDs, ordered prerequisites and source mappings. Import does not imply approval or permission to buy generation.

## Review and version

Review objectives, sequence, prerequisite references, estimated video and learning time, source-section coverage and gaps. Check the final chapters as well as the opening when the source is a book. Present material omissions rather than inventing coverage.

Use the actual current version as `expectedVersion` when editing. A conflict means reread and reconcile; do not overwrite another version. Approve only the specific reviewed version. MCP approval additionally requires its explicit `approve: true` argument. Approval alone does not start generation.

## Choose and generate lessons

Quote the approved version with explicit outputs and either selected stable `lessonIds` or the documented whole-course selection. Retain the user's subset: unselected prerequisites are displayed, not automatically purchased. Explain any prerequisite gap before starting.

Confirm the generation quote fits the approved scope/budget, then accept that exact quote with a stable idempotency key. MCP `start_course` uses the curriculum ID, quote ID and explicit acceptance; REST uses the documented generation body. Save the parent job ID and returned child identities.

Follow each lesson separately. Preserve successful siblings when another lesson fails, is cancelled or needs revision. Retrying/revising one child creates new work; do not rebuild the entire course. Changes to prerequisites can flag older dependent lessons for review; flags do not automatically authorize regeneration.

Finish with [delivery and packaging](delivery.md): ordered stable lesson IDs, learner files and separate instructor answer keys. Course generation does not supply enrolment, learner identity, progress tracking or adaptive tutoring.
