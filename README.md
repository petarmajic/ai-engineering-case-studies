# AI engineering for retail

I worked on a production support assistant that helps shoppers track orders and check delivery options. I started with the tools it calls during a conversation and went on to build the **knowledge base API** that lets our team change the information it uses. On a second project, I built the part of a catalog application that turns supplier files into records the backend can accept.

**Python · FastAPI · LangGraph · Redis · PostgreSQL · pgvector · OpenTelemetry · Arize Phoenix · Django**

## How my responsibilities grew

After my Erasmus semester at Polytech Tours, I rejoined the team in January 2026 as a **student AI/ML engineer**. My early assignments were small enough to follow through one tool call. Later my changes touched what every conversation could retrieve.

<picture>
  <source media="(max-width: 600px)" srcset="assets/engineering-path-mobile.svg">
  <img src="assets/engineering-path.svg" alt="My work progressed from order tools and conversation state to trace evaluation, knowledge releases and supplier data intake.">
</picture>

**Figure 1.** My responsibilities across two teams. The first three steps belong to the support assistant. The fourth shows where I joined the catalog team’s backend work.

I had regular one-to-ones with my mentor and with our engineering manager. On the support team, a colleague separated the admin application from chat and added login. I owned the knowledge base API behind that interface.

## Customer support in production

When an order arrives in two shipments, a single delivery status can give the shopper the wrong impression. I changed how the order tools represent each shipment and added conversation-scoped storage in **Redis** so a follow-up stays tied to the same order.

<picture>
  <source media="(max-width: 600px)" srcset="assets/support-runtime-mobile.svg">
  <img src="assets/support-runtime.svg" alt="The chat runtime calls support tools, keeps state in Redis and retrieves active knowledge. About 30,000 production sessions, with roughly five to eight messages on average per successful session.">
</picture>

**Figure 2.** The language model selects a tool. Application code reads the business data and checks the rules before the assistant explains the result.

I evaluated replies in **Arize Phoenix** by following **OpenTelemetry** traces from the customer’s request to the tool result and the final answer. Through the **FastAPI** routes I wrote, an operator can start a knowledge build, inspect a draft and decide when chat should use it.

[Inside the assistant: conversation state, trace evaluation and the knowledge base API](cases/support-assistant.md)

## Supplier catalog intake

Our second team was building a workbench for turning supplier price lists into catalog updates. I built the file intake and connected it to a teammate’s **Django** service. If a file contains a blocking error, the import stops before it can replace the supplier’s accepted data.

<picture>
  <source media="(max-width: 600px)" srcset="assets/catalog-system-mobile.svg">
  <img src="assets/catalog-system.svg" alt="Supplier files enter a Django backend backed by PostgreSQL. Dashed connections mark the planned review and AI workflow.">
</picture>

**Figure 3.** The team designed the operator review and AI services around Django. My part was the path from a supplier file to a validated initial catalog.

[Catalog architecture and my contribution](cases/catalog-intake.md)

I want to keep building backend services for AI products where I can trace a customer’s answer to its source and test what happens when that source changes.
