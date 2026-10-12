# JavaScript and Python SDKs

Read this when implementing application code or running a script with the [Classive SDKs](https://github.com/classive-hq/sdk). For an AI client already connected through MCP, keep using MCP when it fits the task; installing a language SDK is not required.

Use the SDK for the project's language. Both packages use the REST API with a `ck_live_…` credential, not an MCP key. Keep `CLASSIVE_API_KEY` in the server environment. This skill installs instructions, not SDK packages or credentials.

## Install the intended version

The SDKs share a repository but are separate installable packages. JavaScript/TypeScript is published on npm as `@classive/sdk`. Python is published on PyPI as `classive-sdk`. Check the [SDK README](https://github.com/classive-hq/sdk#install) and [releases](https://github.com/classive-hq/sdk/releases) before copying an installation command. Pin the selected release in the application's dependency file.

JavaScript/TypeScript, Node.js 20+:

```sh
npm install @classive/sdk@0.1.1
```

Python 3.11+:

```sh
python -m pip install classive-sdk==0.1.1
```

Install in the project's existing dependency environment. Do not replace its package manager, recreate its environment, publish a package or configure a new key just to use the SDK.

## Quote and start once

Preserve the user's brief and settings. These snippets preview a quote; accept it only when the existing user/application authorization covers the requested work and charge. Persist the quote, exact input and a stable idempotency key before submitting.

```js
import { Classive } from '@classive/sdk';
const client = new Classive(); // Reads server-side CLASSIVE_API_KEY.
const input = {
  type: 'lesson', prompt: 'Teach equivalent fractions using a number line.',
  audience: { band: 'primary', grade: 'Year 5' }, outputs: ['notes', 'quiz'],
};
const quote = await client.jobs.quote(input);
// Check/present quote.credits and quote scope before accepting.
```

When authorized, submit the same input with `quoteId` using `client.jobs.create({...input, quoteId: quote.id}, {idempotencyKey})`. Retain the returned job id and wait with `client.jobs.wait(job.id)` or integrate webhooks. Do not turn a failed or interrupted request into a new purchase.

```python
from classive import Classive
from classive.types import CreateLesson
input: CreateLesson = {
    "type": "lesson", "prompt": "Teach equivalent fractions using a number line.",
    "audience": {"band": "primary", "grade": "Year 5"}, "outputs": ["notes", "quiz"],
}
with Classive() as client:
    quote = client.jobs.quote(input)
    # Check/present quote["credits"] and its scope before accepting.
```

When authorized, use `client.jobs.create({**input, "quoteId": quote["id"]}, options={"idempotency_key": idempotency_key})` and `client.jobs.wait(job["id"])`. For async applications, use `AsyncClassive`, `async with` and `await`. Python JSON bodies retain the API's camelCase field names; SDK methods/options use snake_case. Do not rename `quoteId`, `lengthMinutes`, `lessonIds` or `starting_after` by assumption.

## Choose methods for the workflow

| Workflow | JavaScript | Python |
| --- | --- | --- |
| Upload, extract and find sources | `materials.upload`, `wait`, `list`, `iterate` | `materials.upload`, `wait`, `list`, `iterate` |
| Quotes and deliverable jobs | `jobs.quote`, `create`, `get`, `wait`, `usage` | `jobs.quote`, `create`, `get`, `wait`, `usage` |
| Recommend and plan curricula | `curricula.recommend`, `quotePlanning`, `create`, `wait`, `importPlan` | `curricula.recommend`, `quote_planning`, `create`, `wait`, `import_plan` |
| Edit, approve and generate selected/full courses | `curricula.edit`, `approve`, `quoteLessons`, `generate` | `curricula.edit`, `approve`, `quote_lessons`, `generate` |
| Retry, revise or cancel | `jobs.quoteRetry`, `retry`, `quoteRevision`, `revise`, `cancel` | `jobs.quote_retry`, `retry`, `quote_revision`, `revise`, `cancel` |
| Deliver files and course packages | `files.json`, `downloadTo`, `jobs.package`, `downloadPackageTo`, `packageFileTo` | `files.json`, `download_to`, `jobs.package`, `download_package_to`, `package_file_to` |
| Voices and prices | `voices.list`, `preview`, `prices.get` | `voices.list`, `preview`, `prices.get` |
| App callbacks and controls | `webhooks`, `billing`, `organisation` | `webhooks`, `billing`, `organisation` |

Load the relevant local workflow reference for scope, source coverage, curriculum approval and learner/instructor separation. The [SDK workflow examples](https://github.com/classive-hq/sdk/blob/main/docs/workflows.md) give exact calls; the [method reference](https://github.com/classive-hq/sdk/blob/main/docs/reference.md) maps every method to its live REST documentation. Fetch only the needed language/workflow sections.

## Handle outcomes correctly

- The SDK retries only eligible reads. POST/PATCH/DELETE requests are never repeated automatically. After an ambiguous submission, reconcile the saved quote/job and explicitly reuse the original idempotency key and body only where supported.
- `JobFailedError` / `JobCancelledError` contain the full job; preserve charges and retained course outputs. SDK timeouts and async cancellation stop the local caller, not the server job. Use `jobs.cancel` for an explicit cancellation request.
- `files.downloadTo(fileId, path)` / `download_to` stream files by id and refuse redirects. Use returned structured file refs (`notes.json`, `notes.pdf`, etc.), not a presumed flat `notes.id`. A `FileRef.url` does not authorize forwarding a bearer key to an arbitrary host.
- Use the exported webhook verifier on the raw body, then deduplicate event ids. Do not parse/re-serialize JSON before verification.
- The SDK is typed against a versioned OpenAPI snapshot. When live schema and installed types differ, update the pinned SDK deliberately or use the documented REST operation. Never silently drop unknown settings to satisfy old types.
- Validate integration code and packaged imports locally first. Distinguish HTTP fixture tests from native API results and delivered educational quality. SDK availability does not certify pipeline reliability.
