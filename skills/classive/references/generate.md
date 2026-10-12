# Generate a learning resource

Read this for a clip, an individual lesson or a written pack. Use [curricula and courses](curriculum.md) for ordered multi-lesson work and [review materials](review.md) for checking existing content.

## Shape the learning brief

Convert the request into a teachable scope: what learners should explain or do afterwards, their audience band/local grade or course level, prior knowledge, language/locale, requested depth and outputs. Preserve supplied values. Clarify only missing choices that materially change the scope or quote.

- Use a clip for a short focused explanation and a lesson for a larger single learning unit. Check current duration limits in [request settings](https://classive.app/docs/reference/request-settings.md).
- For notes/quiz/flashcards without video, explicitly select those outputs on an appropriate teaching job. There is no need to invent a separate `type: written`.
- Set outputs explicitly: omitting them can request the complete bundle. Video includes captions; do not treat captions as an independent invented output value.
- Express goals as learner capabilities. Keep the teaching prompt consistent with the selected type and structured fields; put supported format preferences in their own fields.
- Use the learner's actual local year/course label. Do not infer an equivalence between school systems. Curriculum/exam-board fields guide content but do not certify official alignment.
- List available voices and use a returned public voice ID. Preview through the catalogue/dashboard when choosing; do not substitute an internal provider voice ID.
- Web and corpus research are normally enabled. Preserve an explicit source-only request and its selected materials; do not enable outside research to make a rejected job succeed.

Live detail: [Audience](https://classive.app/docs/reference/audience.md), [outputs](https://classive.app/docs/outputs.md), [voices](https://classive.app/docs/voices.md), [inputs](https://classive.app/docs/inputs.md).

## Execute the same scope through either transport

| Step | MCP | REST |
| --- | --- | --- |
| Read voices/base rates | Discover `list_voices` / `get_prices` | `GET /v1/voices` / `GET /v1/prices` |
| Check selected material versions | `get_material` for each owned ID | `GET /v1/materials/{id}` |
| Obtain complete quote | `quote_job` with the validated teaching request | `POST /v1/jobs/quote` with that request |
| Inspect existing quote | `get_quote` | `GET /v1/quotes/{id}` |
| Start accepted scope | `start_job` with `quoteId`, `acceptQuote: true`, stable `idempotencyKey` | `POST /v1/jobs` with the quoted request and `quoteId`; stable `Idempotency-Key` header |
| Follow delivery | `get_job` | `GET /v1/jobs/{id}` |

Read the live [create-job guide](https://classive.app/docs/api/create-job.md) and discover the current quote/start schemas before preparing payloads. An estimate gives base-rate information; use a complete quote to confirm scope and maximum charge.

Check the quote against the intended outputs, duration, goals, source versions and research permissions. Explain its maximum customer charge and expiry. The quote must fall within existing explicit authorization; otherwise obtain approval before accepting it. Do not quietly enlarge an accepted quote or silently remove scope to fit a budget.

Save the quote ID and idempotency key before starting. MCP loads the immutable input from its quote; it does not accept replacement teaching input at start. REST submits the matching quoted request. Return the resulting job ID and use [jobs and recovery](jobs.md).

## Completion criteria

Use [delivery and packaging](delivery.md) to verify the returned output set, captions, actual video duration, material/source coverage and learner/instructor separation. Identify missing outputs rather than describing an incomplete or failed job as ready.
