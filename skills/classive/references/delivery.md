# Delivery and packaging

Read this when a job has delivered output or when integrating downloads. A completed status is the start of inspecting the returned deliverables, not permission to invent missing files.

## Inspect the result

Compare the actual result with the accepted quote:

- Requested lesson IDs/order, titles and output kinds.
- Video's returned actual duration and its VTT/SRT captions; requested content minutes are distinct from generation runtime.
- Notes formats, figures and math presentation when the user needs them.
- Quiz count/types and learning-goal links; inspect questions/answer material if reviewing educational quality.
- Flashcards, review findings and selected/reviewed source coverage as applicable.
- Failed or cancelled course children and any prerequisite-review flags.

Use the returned file IDs/URLs/content types. Do not construct guessed storage paths or report unrequested/unreturned formats as available. Caption availability is not proof that every cue matches speech; a delivery/schema check is not factual certification.

## Choose the correct package

| Step | MCP | REST |
| --- | --- | --- |
| Inspect a delivered job | `get_job` | `GET /v1/jobs/{id}` |
| Read learner/instructor manifest | Discover `get_package` with explicit audience | `GET /v1/jobs/{id}/package?audience=learner` or `instructor` |
| Download an ordered ZIP | REST/dashboard | `GET /v1/jobs/{id}/download?audience=learner` or `instructor` |
| Download an individual file | REST/dashboard | Its returned `/v1/files/{id}` URL |

Use the learner package for learner delivery. Keep the instructor package and answer keys behind teacher/server access controls. Direct quiz JSON can contain correct answers/explanations; do not assume every individual file URL is a learner-safe payload. Preserve stable lesson/file IDs in the application's manifest rather than flattening the course into unrelated downloads.

## Authenticate downloads

File URLs are organisation-private REST endpoints. Use a REST key with suitable read access, or the signed-in dashboard. An MCP key cannot fetch them. Keep credentials out of displayed URLs, public HTML and learner-side code.

Download through a server or copy authorised files to the user's chosen storage. Inspect status/content type before writing: an error JSON is not a successful MP4/PDF. Support documented range responses when streaming video. Do not expose instructor files through a generic learner proxy.

Report what is delivered, what failed or remains absent, charges, relevant source coverage and how the user can access the files. Preserve successfully completed lessons when the rest of a course fails or is cancelled.

Live references: [Outputs](https://classive.app/docs/outputs.md), [downloads](https://classive.app/docs/files.md), [course packaging tutorial](https://classive.app/docs/tutorials/full-course.md).
