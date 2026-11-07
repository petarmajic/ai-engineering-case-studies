# AI engineering for retail

A shopper can ask our production support assistant where an order is or whether delivery is available to a particular postcode. As a **student AI/ML engineer**, I developed order tools and built the **knowledge base API** the team uses to publish answers to delivery questions. On a second team, I connected supplier price lists to the backend that maintains the product catalog.

<picture>
  <source media="(max-width: 600px)" srcset="assets/engineering-path-mobile.svg">
  <img src="assets/engineering-path.svg" alt="January to October 2026: conversation tools, trace evaluation and knowledge releases for the support assistant. November 2026: supplier file intake and Django integration for the catalog team.">
</picture>

**Figure 1.** My responsibilities on the support and catalog teams in 2026.

I rejoined the team in January after my Erasmus semester at Polytech Tours. I started by improving the data returned by individual tools. Later I took responsibility for publishing the knowledge those tools use across conversations. I had regular one-to-ones with my mentor and with our engineering manager.

## How the technologies fit together

| Engineering area | How it worked in the project |
| --- | --- |
| Web interfaces and APIs | I adapted the **JavaScript** chat widget to call the right **FastAPI** backend in each environment. I also built the admin interface operators use to review and publish knowledge. |
| Agent tools and state | **LangGraph** resumes the conversation so the agent can choose a tool. **Redis** keeps the order context available when the shopper asks a follow-up question. |
| Retrieval-augmented generation (RAG) | **PostgreSQL** stores FAQ entries and their embeddings. The FAQ tool uses **pgvector** to find matching entries in the active version and returns their text to the model. |
| Tracing and evaluation | I used **OpenTelemetry** to export traces. In **Arize Phoenix**, I compared the tool result with the answer the shopper received. |
| Catalog integration | My **Python** component turns supplier files into product records. A teammate’s **Django** service stores the validated records in **PostgreSQL**. |

## Customer support in production

An order can arrive in two shipments. I changed how the order tool maps delivery statuses so the assistant can tell the shopper which parcel arrived and which is still on its way.

<picture>
  <source media="(max-width: 600px)" srcset="assets/support-runtime-mobile.svg">
  <img src="assets/support-runtime.svg" alt="The chat runtime calls support tools, keeps state in Redis and retrieves active knowledge. About 30,000 production sessions, with roughly five to eight messages on average per successful session.">
</picture>

**Figure 2.** The assistant handled about 30,000 production sessions. Successful sessions averaged roughly 5–8 messages.

The team also needed to update FAQ content while customers were using the assistant. I built the API so an operator can inspect a draft before switching chat to it or restore a previously reviewed version.

[Support assistant architecture, retrieval and trace review](cases/support-assistant.md)

## Supplier catalog intake

Suppliers use different column names and file formats. I built a common intake for their price lists so the catalog service receives the same kind of product record from each supplier.

<picture>
  <source media="(max-width: 600px)" srcset="assets/catalog-system-mobile.svg">
  <img src="assets/catalog-system.svg" alt="Supplier files enter a Django backend backed by PostgreSQL. Dashed connections mark the planned review and AI workflow.">
</picture>

**Figure 3.** The solid connections show the intake I connected to the team’s backend. Dashed connections show the planned operator review and AI suggestions.

[Catalog architecture and my work with the backend team](cases/catalog-intake.md)

## What I want to work on next

I want to compare retrieval changes on the same set of customer questions and trace how a change affects the final answers. I’d also like to keep building APIs that other teams can use to review and release those changes.
