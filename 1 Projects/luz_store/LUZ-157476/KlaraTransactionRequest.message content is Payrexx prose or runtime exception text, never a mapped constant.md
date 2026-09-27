---
ai_hash: 3a8473a794645615
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-24
entities:
- KlaraTransactionRequest
- message field
- Payrexx
- luz_online_payment
- ConsumerServiceClientErrorException
- TransactionTask
- PayrexxResponse
- WebApplicationException
- ProcessingException
- NPE
- ChargeTransactionService
- IllegalStateException
- TransactionRestCallerV2
- ConsumerApiErrorHandled
- ConsumerApiErrorHandledInterceptor
- LUZ-157476
- PayrexxCommunicatorTest
- PayrexxResponse.java
- TransactionTask.java
- getTransactionsWithinTimeRange
- charge operation
- refund operation
- status
- ERROR status
- TECHNICAL_ERROR status
- decline reason
- failureCategory
- luz_store
- Payrexx decline codes
- Payrexx-authored prose
- JVM/framework exception text
- repo-defined literal
- dynamic exception text
- 'An error occurred: '
- request.getId()
- e.getMessage()
- response.getMessage()
- getCause()
- normal charge/refund result path
- success
- enumerable set of code-keyed constants
- structured failureCategory
source: session 2026-07-24, code investigation
status: seedling
tags:
- luz-online-payment
- payrexx
- gotcha
- LUZ-157476
title: KlaraTransactionRequest.message content is Payrexx prose or runtime exception
  text, never a mapped constant
type: lesson
---

# KlaraTransactionRequest.message content is Payrexx prose or runtime exception text, never a mapped constant

The `message` field on `KlaraTransactionRequest` (luz_online_payment) is never assigned a string literal on the normal charge/refund result path — every `setMessage(...)` passes dynamic exception text. Its content is one of exactly three kinds:

1. **Payrexx-authored prose** (common decline case, status=ERROR): from `PayrexxResponse.message` via `ConsumerServiceClientErrorException(response.getMessage())` -> `TransactionTask` catch (`e.getMessage()`, since that exception carries no cause). Pattern: `"An error occurred: <reason>."` e.g. "An error occurred: Your card has expired." The `"An error occurred: "` prefix is added by Payrexx, not Klara. **Verified (grep):** the string appears in luz_online_payment only in a doc comment (`PayrexxResponse.java:23`) and a test mock (`PayrexxCommunicatorTest.java:30`) — no `src/main` code emits or concatenates it, proving it is external/pass-through.
2. **JVM/framework exception text** (status=TECHNICAL_ERROR): WebApplicationException / ProcessingException timeout / NPE / the `ChargeTransactionService.exceptionally` executor failure. Runtime-generated, e.g. "HTTP 500 Internal Server Error".
3. **One repo-defined literal** (status=TECHNICAL_ERROR): `IllegalStateException("Can not charge without Transaction Id")` / `"Can not refund without Transaction Id"` thrown inside the try at `TransactionTask.java:30/58` when `request.getId()` is null. The ONLY message string literally defined in luz_online_payment.

On success the converter never sets message -> null.

Subtlety: the charge/refund methods in `TransactionRestCallerV2` are NOT annotated `@ConsumerApiErrorHandled` (only `getTransactionsWithinTimeRange` is), so `ConsumerApiErrorHandledInterceptor` does not wrap them — which is why Payrexx prose reaches `TransactionTask` clean via `e.getMessage()` rather than through a wrapped `getCause()`.

**Consequence for the taxonomy:** the decline reason is free-form English authored by Payrexx, not an enumerable set of code-keyed constants -> cannot be reliably switched on. Reinforces introducing a structured failureCategory at this boundary.

Repo: luz_online_payment. Ticket: LUZ-157476.

## Related

- [[Payrexx card declines reach luz_store as ERROR with prose, not DECLINED]]
- [[luz_online_payment silently drops Payrexx decline codes]]

%% ai-graph-start %%

**Related notes:**
- [[Payrexx card declines reach luz_store as ERROR with prose, not DECLINED]]
- [[LUZ-157476 decline taxonomy maps codes at luz_online_payment boundary]]
- [[Payrexx v1.0 charge API returns only status+message on failure — no ISO 8583 code]]
- [[KlaraPay V2 Java classes still call Payrexx API v1.0 on the consumer flow]]
- [[LUZ-157476 decline-code flow luz-online-payment forwards, luz_store maps]]

**Relations:**
- KlaraTransactionRequest — *HAS_FIELD* — message field
- message field — *CONTAINS* — Payrexx-authored prose
- message field — *CONTAINS* — JVM/framework exception text
- message field — *CONTAINS* — repo-defined literal
- message field — *IS_NEVER* — mapped constant
- message field — *IS_NEVER_ASSIGNED_STRING_LITERAL_ON* — normal charge/refund result path
- message field — *RECEIVES* — dynamic exception text
- Payrexx-authored prose — *ORIGINATES_FROM* — PayrexxResponse.message
- Payrexx-authored prose — *IS_HANDLED_BY* — ConsumerServiceClientErrorException
- ConsumerServiceClientErrorException — *IS_CAUGHT_BY* — TransactionTask
- TransactionTask — *EXTRACTS_MESSAGE_VIA* — e.getMessage()
- Payrexx-authored prose — *HAS_PREFIX* — An error occurred: 
- Payrexx — *ADDS_PREFIX* — An error occurred: 
- luz_online_payment — *DOES_NOT_EMIT_PREFIX* — An error occurred: 
- Payrexx-authored prose — *ASSOCIATED_WITH* — ERROR status
- JVM/framework exception text — *IS_RUNTIME_GENERATED* — true
- JVM/framework exception text — *INCLUDES* — WebApplicationException
- JVM/framework exception text — *INCLUDES* — ProcessingException
- JVM/framework exception text — *INCLUDES* — NPE
- JVM/framework exception text — *ORIGINATES_FROM* — ChargeTransactionService
- JVM/framework exception text — *ASSOCIATED_WITH* — TECHNICAL_ERROR status
- repo-defined literal — *IS_AN* — IllegalStateException
- repo-defined literal — *IS_DEFINED_IN* — TransactionTask.java
- repo-defined literal — *TRIGGERED_BY* — request.getId() IS_NULL
- repo-defined literal — *ASSOCIATED_WITH* — TECHNICAL_ERROR status
- message field — *BECOMES_NULL_ON* — success
- TransactionRestCallerV2 — *PERFORMS* — charge operation
- TransactionRestCallerV2 — *PERFORMS* — refund operation
- charge operation — *LACKS_ANNOTATION* — ConsumerApiErrorHandled
- refund operation — *LACKS_ANNOTATION* — ConsumerApiErrorHandled
- getTransactionsWithinTimeRange — *HAS_ANNOTATION* — ConsumerApiErrorHandled
- ConsumerApiErrorHandledInterceptor — *DOES_NOT_INTERCEPT* — charge operation
- ConsumerApiErrorHandledInterceptor — *DOES_NOT_INTERCEPT* — refund operation
- Payrexx prose — *REACHES* — TransactionTask VIA e.getMessage()
- Payrexx prose — *DOES_NOT_USE* — getCause()
- decline reason — *IS_A* — free-form English
- decline reason — *AUTHORED_BY* — Payrexx
- decline reason — *IS_NOT_A* — enumerable set of code-keyed constants
- decline reason — *CANNOT_BE* — reliably switched on
- structured failureCategory — *IS_RECOMMENDED* — true
- luz_online_payment — *IS_A* — Repo
- LUZ-157476 — *IS_A* — Ticket
- Payrexx card declines — *REACH* — luz_store
- Payrexx card declines — *HAS* — ERROR status
- Payrexx card declines — *CONTAINS* — prose
- luz_online_payment — *DROPS* — Payrexx decline codes
- KlaraTransactionRequest — *BELONGS_TO* — luz_online_payment
- TransactionTask — *BELONGS_TO* — luz_online_payment
- ChargeTransactionService — *BELONGS_TO* — luz_online_payment
- TransactionRestCallerV2 — *BELONGS_TO* — luz_online_payment
- PayrexxResponse.java — *BELONGS_TO* — luz_online_payment
- PayrexxCommunicatorTest — *BELONGS_TO* — luz_online_payment

%% ai-graph-end %%