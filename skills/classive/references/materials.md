# Materials and document coverage

Read this when the task depends on a book, syllabus, draft, reference or assessment source. Use only material the user owns or has permission to use. Keep supplied attribution, source URL and rights information.

## Find and reuse before uploading

| Step | MCP | REST |
| --- | --- | --- |
| Search organisation materials | Discover `list_materials` with search/status/pagination | `GET /v1/materials` with documented query parameters |
| Inspect a selected version | `get_material` with `materialId` | `GET /v1/materials/{id}` |
| Upload a new source/version | Dashboard or REST; no upload action assumed | `POST /v1/materials` |
| Retry failed extraction | Dashboard or REST | `POST /v1/materials/{id}/retry` |
| Delete a source/index | Dashboard or REST | `DELETE /v1/materials/{id}` |

Search by filename/source URL and paginate the library; do not ask the user to choose from hundreds of loaded buttons. Identify the actual ready material/version, not just a filename. Reuse it where appropriate instead of creating duplicate uploads.

For a new upload, read [Materials API](https://classive.app/docs/api/materials.md) for multipart versus JSON/base64 shape, metadata and current envelope/file limits. Retain the original source; a replacement uses the documented version-link field. Supply only documented fields.

## Wait and inspect

Upload is asynchronous. Poll the returned material ID until extraction is ready or failed. A 202 is acceptance, not usable extracted content. Do not quote work against a queued/failed source.

Inspect:

- Extraction completeness, truncation/errors and retained character count.
- Indexed sections/chapters and beginning/middle/ending representation.
- Actual locators: physical pages only when provided reliably; otherwise character ranges.
- Version and applicable retention/expiry before starting later work.

An empty/missing index or incomplete extraction needs an honest explanation; do not label it a fully processed book. Completeness of retained text does not certify OCR accuracy.

## Choose the right coverage workflow

- **A focused lesson:** relevant passages from the selected owned versions may be enough. Report selected and omitted scope; a short lesson cannot promise to teach every chapter.
- **A curriculum from a book:** use [curriculum planning](curriculum.md). Inspect its complete section/chapter map, gaps and lesson coverage before approving. Missing section summaries are preparation work included in the planning quote and reused per version.
- **An exhaustive review:** use [review materials](review.md). Check explicit reviewed/unreviewed sections. Retrieval relevance is not an exhaustive pass.

Do not equate `extracted`, `selected`, `reviewed` and semantically `covered`. Use the actual response fields and their documented qualifications.

Source deletion can be refused while active work references it. It does not imply removal of completed outputs, billing records or already-used passages. Read current retention behavior before promising erasure.

Live references: [Material lifecycle](https://classive.app/docs/materials.md), [long materials](https://classive.app/docs/how-to/long-materials.md), [using materials](https://classive.app/docs/how-to/use-your-materials.md).
