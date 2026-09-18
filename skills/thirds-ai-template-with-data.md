---
name: thirds-ai-template-with-data
description: Render a saved thirds.ai template with new data as a PDF or image.
api: thirds.ai REST API
operations: [listTemplates, getTemplate, createPdf, createImage, getPdf, getImage]
---
# Render a saved template with data

1. **Find the template.** `GET /v1/templates` (cursor pagination) or `GET /v1/templates/{template_id}` to confirm the latest published `version`.
2. **Render.** For a PDF: `POST /v1/pdf` with `{"template_id":"tpl_...","version":1,"data":{...}}`. For an image: `POST /v1/image` with `{"template_id":"tpl_...","version":1,"data":{...},"image":{"format":"png","width":1200,"height":630}}`. Send `Idempotency-Key`.
3. **Poll and download.** Poll `GET /v1/pdf/{id}` or `GET /v1/image/{id}` until `succeeded`, then fetch the signed `download.url`.

**Rules:** `data` must match the template's fields or you get `template_data_invalid` / `template_missing_data`. Image width 320-7680, height 200-4320.
