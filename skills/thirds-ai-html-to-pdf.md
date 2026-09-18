---
name: thirds-ai-html-to-pdf
description: Render raw HTML to a PDF with thirds.ai and download the finished file.
api: thirds.ai REST API
operations: [createPdf, getPdf, getDownload]
---
# HTML to PDF

1. **Create the render.** `POST /v1/pdf` with `{"html": "<html>...</html>"}` and headers
   `Authorization: Bearer thirds_sk_v1_...` and a unique `Idempotency-Key`. One finished PDF costs 1 credit; a failed render costs nothing.
2. **Poll.** The response is `202` with a job id. Poll `GET /v1/pdf/{id}` under an application deadline. Stop on `failed` or `cancelled`; continue while `queued` or `running`.
3. **Download.** On `succeeded`, resolve `download.url` against `https://thirds.ai` and fetch it **without** the API key. The signed link lasts 15 minutes; the file stays in the account for 30 days.

**Rules:** always send `Idempotency-Key` so a retry returns the same job. On `429` read `Retry-After` (seconds). Errors use `{error:{code,request_id}}`; log `x-request-id`.
