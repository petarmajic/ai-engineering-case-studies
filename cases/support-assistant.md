# Conversational AI Support Platform

A shopper can track an order, follow up about a delivery or start a support request. From January through October 2026, I worked on the tools behind those conversations and built the API our team used to publish new knowledge.

[Overview](../README.md)

## Following a conversation

<picture>
  <source media="(max-width: 600px)" srcset="../assets/support-runtime-mobile.svg">
  <img src="../assets/support-runtime.svg" alt="The chat widget sends a message to FastAPI. The LangGraph runtime uses order and CRM tools, Redis state and FAQ retrieval.">
</picture>

**Figure 1.** The web widget sends the message to FastAPI. LangGraph resumes the conversation and the model can ask a tool for the data it needs.

**Order context.** I mapped shipment statuses separately so an answer about one parcel would not be mistaken for the status of the whole order. I used Redis to keep order context and attachment references available across messages.

**Trace review.** I used OpenTelemetry to export request traces to Arize Phoenix. Each trace let me follow the model and tool calls behind a single reply. For example, if the order tool returns two shipments but the final reply mentions only one, I can inspect how the model used the tool result.

## Building the knowledge base API

I built the FastAPI routes for preparing, inspecting and activating knowledge versions. I also built the review interface that calls those routes. A teammate separated the admin and chat services and added login.

<picture>
  <source media="(max-width: 600px)" srcset="../assets/knowledge-release-mobile.svg">
  <img src="../assets/knowledge-release.svg" alt="The admin API writes and activates versions. Chat reads only the active version through the FAQ tool.">
</picture>

**Figure 2.** An operator can review a draft while the assistant searches the active version. Restoring an earlier release changes which entries the next query can retrieve.

## From source text to a reply

**Scrape and chunk.** I collected FAQ pages and longer support documents from the web. For long documents, the build proposes questions and selects the source chunks that answer them. The application checks those chunk IDs and assembles the answer from the original text. That keeps a generated question tied to wording the team can review.

**Embed and publish.** I migrated the earlier FAISS baseline to **PostgreSQL with pgvector**. Each FAQ entry belongs to a knowledge version and stores its question, answer, source and embedding. The build embeds the entry’s search text. The knowledge base API controls when the draft becomes searchable by the chat agent.

**Retrieve and answer.** The FAQ tool embeds the shopper’s question, finds nearby entries in the active version and gives their text to the model. For supported postcode tables, I added a row lookup after retrieval. A surcharge table can be relevant to a delivery question, but a postcode missing from that table tells us nothing about whether delivery is available.

[See one FAQ entry move from page to chat](../README.md#how-a-web-page-becomes-a-chat-answer)
