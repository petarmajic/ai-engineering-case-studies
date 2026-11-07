# AI engineering for retail

A shopper can ask our production support assistant about an order or a delivery. As a **student AI/ML engineer**, I built order tools and the **knowledge base API** our team uses to publish support content. On a second project, I connected supplier price lists to a teammate’s catalog backend.

<picture>
  <source media="(max-width: 600px)" srcset="assets/engineering-path-mobile.svg">
  <img src="assets/engineering-path.svg" alt="Engineering timeline. January to October 2026: web chat, agent tools, traces, FAQ preparation, vector search and a knowledge base API. November 2026: Python file intake, validation and a Django catalog integration. The planned catalog workflow is labelled separately.">
</picture>

**Figure 1.** My responsibilities and the technologies I used on each project. The final line shows the catalog team’s planned workflow.

I rejoined the team in January after my Erasmus semester at Polytech Tours. My first tasks concerned the data returned by individual tools. Later I was responsible for how the team prepared, reviewed and released new knowledge. I had regular one-to-ones with my mentor and with our engineering manager.

## Customer support in production

The assistant handled **about 30,000 production sessions**. Successful sessions averaged roughly **5–8 messages**. I evaluated replies in **Arize Phoenix** by following OpenTelemetry traces from the tool result to the final answer. If an order had two shipments, for example, I could check whether the reply accounted for both.

I scraped support pages, split long documents into chunks and used the source text to build question-and-answer entries. I stored those entries and their embeddings in **PostgreSQL**. The FastAPI routes I built let an operator activate a reviewed version, and the FAQ tool uses **pgvector** to search that version during chat.

[Support assistant architecture, knowledge releases and trace review](cases/support-assistant.md)

## Supplier catalog intake

Suppliers use different formats for their price lists. I built the **Python** intake that maps those files to a common product record and connected it to the team’s **Django** service. The backend can reject an incomplete import before it replaces the accepted catalog.

[Catalog architecture and my work with the backend team](cases/catalog-intake.md)

## What I want to work on next

I want to compare retrieval changes on the same set of customer questions and trace how a change affects the answers. I’d also like to keep building APIs that other teams use to review and release those changes.
