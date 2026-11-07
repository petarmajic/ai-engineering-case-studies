# Conversational AI Support Platform

A shopper can track an order, ask about delivery or start a support request. I worked on the existing production assistant from January through October 2026. I started with the tools the assistant called and later took responsibility for the knowledge base API.

[Overview](../README.md)

## Following a conversation

<picture>
  <source media="(max-width: 600px)" srcset="../assets/support-runtime-mobile.svg">
  <img src="../assets/support-runtime.svg" alt="The chat widget sends a message to FastAPI. The LangGraph runtime uses order and CRM tools, Redis state and FAQ retrieval.">
</picture>

**Figure 1.** FastAPI receives the message. LangGraph resumes the conversation and the model chooses a tool according to what the shopper asks.

**Tool integration.** An order can arrive in several shipments. I mapped the status of each delivery so the assistant could explain which parcels were still on their way. I also used Redis to keep order context and attachments available across messages.

**Evaluation.** I reviewed OpenTelemetry traces in Arize Phoenix to see how the assistant reached an answer. For example, if a tool returns two shipments and the reply mentions only one, the trace shows where the detail was lost.

## Building the knowledge base API

I built the FastAPI routes and review interface that let an operator prepare a new knowledge version, inspect its contents and choose when to publish it. A teammate separated the admin and chat services and added login. My responsibility was the release process behind that interface.

<picture>
  <source media="(max-width: 600px)" srcset="../assets/knowledge-release-mobile.svg">
  <img src="../assets/knowledge-release.svg" alt="The admin API writes and activates versions. Chat reads only the active version through the FAQ tool.">
</picture>

**Figure 2.** Operators can review a draft or restore a previous release. Conversations search the active version.

I migrated the earlier FAISS baseline into **PostgreSQL with pgvector**. Separating a draft from the active version meant the team could update the knowledge base while customers were still using the assistant. The FAQ tool searches the version the operator has approved, including when an earlier release is restored.
