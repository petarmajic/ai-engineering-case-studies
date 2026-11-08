# AI engineering for retail

As a **student AI/ML engineer**, I worked on a customer support chatbot that handled **nearly 30,000 sessions in production**. I built order tracking tools and the knowledge base API our team uses to publish support content. I monitored conversations in Arize Phoenix and evaluated replies by comparing what the chatbot told shoppers with the shipment data our tools returned.

I now work on a supplier catalog that takes Excel price lists as input. A single file can have around **20,000 rows and 50 columns**. I write the Python code that maps each supplier’s columns to the product records our Django backend expects.

<picture>
  <source media="(max-width: 600px)" srcset="assets/engineering-path-mobile.svg">
  <img src="assets/engineering-path.svg" alt="Support assistant: conversation tools, knowledge releases and trace evaluation. Supplier catalog: file validation and Django integration. The dashed branch marks the planned team workflow.">
</picture>

**Figure 1.** My work on the support assistant and supplier catalog. The dashed branch shows the catalog team’s planned review and AI workflow.

I rejoined the team in January after my Erasmus semester at Polytech Tours. My first tasks concerned the data returned by individual tools. Later I was responsible for how the team prepared, reviewed and released new knowledge. I had regular one-to-ones with my mentor and with our engineering manager.

## Customer support in production

Successful support sessions averaged roughly **5–8 messages**. I evaluated replies in **Arize Phoenix** by following OpenTelemetry traces from the tool result to the final answer. If an order had two shipments, for example, I could check whether the reply accounted for both.

I scraped support pages, split long documents into chunks and used the source text to build question-and-answer entries. I stored those entries and their embeddings in **PostgreSQL**. The FastAPI routes I built let an operator activate a reviewed version, and the FAQ tool uses **pgvector** to search that version during chat.

[Support assistant architecture, knowledge releases and trace review](cases/support-assistant.md)

## Supplier catalog intake

Suppliers use different formats for their price lists. I built the **Python** intake that maps those files to a common product record and connected it to the team’s **Django** service. The backend can reject an incomplete import before it replaces the accepted catalog.

[Catalog architecture and my work with the backend team](cases/catalog-intake.md)

## What I learned

Working on a production assistant taught me to follow an AI feature end to end. For the knowledge base, that meant turning source content into searchable entries, releasing a reviewed version and tracing how the agent used it in a customer’s reply. I’m looking forward to contributing to the team’s next releases and learning new technologies along the way.
