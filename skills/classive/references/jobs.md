# Jobs, cancellation and recovery

Read this after any generation start or when following, cancelling, retrying or revising existing work. Use [the lifecycle](https://classive.app/docs/jobs.md), [job detail](https://classive.app/docs/api/get-job.md), [revision/retry guide](https://classive.app/docs/api/revise-job.md) and [billing](https://classive.app/docs/billing.md) for current semantics.

## Follow asynchronous work

Save the job ID and return it promptly. Poll `get_job` or `GET /v1/jobs/{id}` with bounded intervals/backoff appropriate to a long-running job; avoid a tight loop. Honour returned rate limits and stop polling when a terminal state is reached or the user stops the task. An application can use its documented webhooks instead of holding a synchronous request open.

Read the state, stage and timestamps together. A percentage or ETA is an estimate, not proof of completion. A plateau or missing percentage does not prove the worker is dead. Explain overdue work honestly; do not fabricate progress or a finish time from elapsed time.

| State | Agent action |
| --- | --- |
| `queued` / `processing` | Retain ID and follow status; do not resubmit generation |
| `cancelling` | Follow the same job until stopped; an in-flight operation may finish |
| `completed` | Inspect actual outputs and use [delivery](delivery.md) |
| `failed` | Preserve failure code/message, charges and delivered child files; investigate before new paid work |
| `cancelled` | Inspect settlement/completed children; additional work needs an appropriate new quote |

For a course, inspect individual child states rather than assuming all lessons share the parent's result.

## Interrupted start

A network timeout or lost response leaves acceptance uncertain. Reconcile using the original quote/idempotency identity and existing job listing as appropriate. For idempotent start routes, retry with exactly the same key and accepted payload. Do not invent a new key, accept a second quote or replay a whole course because the first response was lost. A conflict requires inspecting the original request, not changing the key to evade it.

## Controls and new attempts

| Operation | MCP | REST |
| --- | --- | --- |
| Cancel active work | Discover `cancel_job` with `jobId` | `POST /v1/jobs/{id}/cancel` |
| Inspect recorded usage/charges | `get_job_usage` | `GET /v1/jobs/{id}/usage` |
| Quote a failed/cancelled individual retry | Use REST/dashboard unless discovery supplies an action | `POST /v1/jobs/{id}/retry/quote` |
| Accept that retry quote | Use REST/dashboard | `POST /v1/jobs/{id}/retry` |
| Quote a completed job's requested revision | Use REST/dashboard | `POST /v1/jobs/{id}/revisions/quote` |
| Accept the same revision scope | Use REST/dashboard | `POST /v1/jobs/{id}/revisions` |

Cancellation is an external mutation: carry out the user's requested control, not a decision inferred merely from slow progress. It stops queued work immediately or running work at safe boundaries according to the service. Inspect the resulting ledger; do not promise every already-running provider operation vanished.

Current unfinished individual jobs release unused reservations on cancellation/failure; completed course children keep their files and charges. Check live billing for changes before promising a refund or fee. Do not add an invented cancellation penalty.

A retry/revision is a new job and potential purchase. Quote the exact operation, preserve the source job identity and outputs, and ensure explicit authorization covers its maximum charge. Preserve completed siblings and old files. Do not automatically retry repeated quality failures or narrow learning goals/remove checks to obtain success.

For 400/422, correct the specific documented field or conflicting intent with the user as needed. For 402, report insufficient credits; do not top up. For 409, reconcile stale versions/quote/idempotency. For 429/503, distinguish pre-acceptance failure from uncertain start before using bounded retry. Retain the actual service error rather than classifying every failure as an invalid prompt.
