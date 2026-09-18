---
name: thirds-ai-csv-batch
description: Render one thirds.ai template across many data rows and download a ZIP of the results.
api: thirds.ai REST API
operations: [createBatch, getBatch, retryBatch, getBatchArchive]
---
# CSV / row batch render

1. **Create the batch.** `POST /v1/batches` with a `template_id`, `version`, and 10-200 rows of `data`. Each finished file costs 1 credit. Send `Idempotency-Key`.
2. **Poll rows.** `GET /v1/batches/{id}` to watch each row (`queued`/`running`/`succeeded`/`failed`).
3. **Retry / cancel.** `POST /v1/batches/{id}/retry` re-runs failed, cancelled, or pending rows; `POST /v1/batches/{id}/cancel` stops pending and queued rows before they render.
4. **Download.** Fetch the ZIP of successful files via the batch archive signed link (`GET /v1/batches/archive/{token}`).

**Rules:** account concurrency is 16 live jobs (8 per key); pace submissions (5/sec, burst 21).
