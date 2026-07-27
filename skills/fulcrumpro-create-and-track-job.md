---
name: Create and track a production job
description: Create a job from a sales order in Fulcrum, advance its status, and track labor time.
api: openapi/fulcrumpro-openapi-original.json
operations: [CreateJob, GetJob, StatusUpdateJob, GetJobTracking, StartTimer]
---

# Create and track a production job (Fulcrum)

Move work onto the shop floor and monitor it.

## Auth
`Authorization: Bearer <token>` (site-scoped JWT). Base URL
`https://api.fulcrumpro.com/api/`.

## Steps
1. Create the job with `CreateJob` (POST `/api/jobs`). Reference the item to
   make (`itemToMakeId`) and, when applicable, the source `salesOrderId`; set
   `earliestScheduledStartUtc` to influence scheduling.
2. Read it with `GetJob` (GET `/api/jobs/{jobId}`).
3. Advance the job through its workflow with `StatusUpdateJob`
   (POST `/api/jobs/{jobId}/status`).
4. Start labor tracking with `StartTimer` (POST `/api/job-tracking-timers/start`).
5. Monitor live progress with `GetJobTracking` (GET `/api/jobs/{jobId}/tracking`).

## Rules
- Errors are RFC 9457 `application/problem+json`.
- Cancel rather than delete a live job: `CancelJob` (POST `/api/jobs/{jobId}/cancel`).
- No idempotency key — verify state with `GetJob` before retrying a status change.
