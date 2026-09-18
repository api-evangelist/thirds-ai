---
name: thirds-ai-ai-template-build
description: Build an editable thirds.ai template with AI from a prompt, HTML, or image, then publish a version.
api: thirds.ai REST API
operations: [createTemplateBuild, editTemplateBuild, getTemplateBuild, getTemplateBuildDraft, publishTemplateBuild]
---
# AI template build

1. **Start the build.** `POST /v1/template-builds` with your prompt / HTML / image intent. An AI build costs 50 credits and needs trial, pack, or plan credits (free credits do not fund AI).
2. **Iterate.** `POST /v1/template-builds/{build_id}/messages` to send a change (a words/HTML message costs 25 credits; a new-picture message 50). Read the result with `GET /v1/template-builds/{build_id}` and the draft with `GET /v1/template-builds/{build_id}/draft`. Cancel a pending message with `DELETE /v1/template-builds/{build_id}/messages/{message_id}` to release reserved credits.
3. **Publish.** When the draft is right, `POST /v1/template-builds/{build_id}/publish` to save a permanent template version, then render it with the template-with-data skill.

**Rules:** send `Idempotency-Key`. AI errors surface as `failure_category` / `failure_code` with a `repair_code` for the last safe step.
