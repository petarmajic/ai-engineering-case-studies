# Conversational AI Support Platform

A shopper can ask about an order, follow up about one of its deliveries or send a photo for a support request. I worked on the existing assistant from January through October 2026. My work covered the order tools, conversation state, trace evaluation and the API for publishing new knowledge.

[Overview](../README.md)

## What happens during a chat

<picture>
  <source media="(max-width: 600px)" srcset="../assets/support-runtime-mobile.svg">
  <img src="../assets/support-runtime.svg" alt="The chat widget sends a message to FastAPI. The LangGraph runtime uses order and CRM tools, Redis state and FAQ retrieval.">
</picture>

**Figure 1.** FastAPI accepts the message. LangGraph resumes the conversation and the model chooses a tool according to what the shopper asks.

**Orders and attachments.** An order can arrive in several shipments. I mapped the order and warehouse statuses so the tool returned a status for each delivery. I also added Redis storage for attachments. A shopper can send a photo before a ticket exists. Once the ticket is created, the application uses the stored ticket ID to attach the photo to the right request.

**Production use.** The assistant handled about **30,000 sessions**. Successful sessions averaged roughly **5–8 messages**.

**Evaluation.** I used OpenTelemetry to export traces in batches and reviewed them in Arize Phoenix. For example, if an order tool returns two shipments and the reply mentions only one, the trace lets me see where the detail was lost. I also inspected retries and fallback calls. A repeated lookup and a repeated ticket request have different consequences, so I added a separate call limit for ticket creation.

## The knowledge base API

I built the **FastAPI** interface that operators use to start a knowledge build, check its status, list and export versions, activate a draft and restore an earlier release. I also built the review interface on top of it. A teammate separated the admin service from the chat service and added session login.

<picture>
  <source media="(max-width: 600px)" srcset="../assets/knowledge-release-mobile.svg">
  <img src="../assets/knowledge-release.svg" alt="The admin API writes and activates versions. Chat reads only the active version through the FAQ tool.">
</picture>

**Figure 2.** Building and activating are separate operations. An operator can inspect a candidate before changing what customers receive.

| Operator action | What happens in the backend |
| --- | --- |
| Start a build | The service collects source text and tables. For long documents, the model selects source blocks and Python assembles the answer from the original wording. |
| Review a version | Checks compare table headers and row counts with the source. A successful build produces a draft the operator can inspect or export. |
| Activate or restore | A PostgreSQL transaction switches the active version. I fixed reactivation of archived versions so an operator could return to previously reviewed content. |

I migrated the earlier **FAISS** baseline to versioned records in **PostgreSQL with pgvector** and changed the FAQ tool to search the active version. For a delivery question, vector search finds a relevant table and Python checks the postcode against its rows. A postcode missing from a surcharge list tells us nothing about delivery availability. If the tool cannot identify the postcode or interpret the table reliably, it returns the broader table context.
