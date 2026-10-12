# Integrate Classive into an application

Read this for API integration code, a content-generation feature or an application driven by an AI client. For JavaScript/TypeScript or Python, load [SDKs](sdks.md) for installation, typed calls and async/file helpers. Fetch [OpenAPI](https://classive.app/openapi.json) plus only the relevant endpoint guides; do not copy the whole schema into this skill.

## Keep the purchase asynchronous

Use a server-side flow: validate the learning brief → obtain complete quote → apply the application's explicitly authorised scope/credit policy → accept once with an idempotency key → persist job ID → receive status updates → publish delivered learner files. A quote does not reserve credits or authorise itself. Check its expiry before accepting.

Use MCP when an AI client is orchestrating the workflow; use a language SDK or direct REST when implementing application endpoints, uploads or downloads. All reach the same organisation/job controls. Any step can use its documented SDK/MCP/REST method when the operation exists and the appropriate credential is configured; credentials and schemas are not interchangeable.

Store the accepted input, quote, maximum customer credits, idempotency key, job/curriculum/child IDs and delivery manifest. These let interrupted clients reconcile a purchase without creating another one. Do not use a long synchronous browser request as the entire job lifecycle.

## Status and events

Use bounded REST polling or the documented webhook flow. Read [webhooks](https://classive.app/docs/webhooks.md) before implementing signature verification, event handling or retry assumptions. Process duplicate notifications idempotently; reconcile with the authoritative job state. A callback cannot authorize extra paid work.

Model queued/processing/cancelling/terminal states, partial course delivery and recoverable client disconnection separately. Show actual stage plus clearly estimated progress; do not equate an ETA with an SLA. Preserve customer error codes and field details in actionable UI.

## Scope and credentials

- Keep REST/MCP keys server-side or in the AI client's secret configuration. Use minimum needed scopes; setup uses [authentication](https://classive.app/docs/authentication.md).
- Persist the organisation-owned material IDs/versions and approved curriculum version. Do not identify a source solely by filename.
- Enforce the caller's permitted outputs, source permissions and spending/concurrency policy before accepting work. Read current [billing](https://classive.app/docs/billing.md) and [curriculum/control endpoints](https://classive.app/docs/api/curricula.md).
- Use an existing authorised test environment/account. Do not create credentials, grant credits, initiate top-ups or alter organisation limits unless the user requested those actions.

## Deliver a learner experience

Use [delivery and packaging](delivery.md) to publish only learner-safe resources. Provide stable ordered lesson IDs, video/captions, notes and assessments from returned manifests. Learner enrolment, identity, progress, saved quiz attempts, tutoring and adaptive assessment remain application responsibilities unless the live contract explicitly exposes them.

## Verify the integration

Test request validation, quote expiry/scope, idempotent interrupted starts, owner isolation, terminal error handling, duplicate events, partial courses, cancellation settlement and learner/instructor separation as relevant to the change. Separate mocked transport tests from a real delivered-resource test. A live paid test needs an explicitly authorised scope and total spend ceiling; do not purchase a full matrix just to validate setup.

Useful live guides: [REST quickstart](https://classive.app/docs/quickstart.md), [create job](https://classive.app/docs/api/create-job.md), [job lifecycle](https://classive.app/docs/jobs.md), [complete-course tutorial](https://classive.app/docs/tutorials/full-course.md).
