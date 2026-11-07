# Catalog Intake Workbench

Suppliers use different formats for their price lists. Our team designed a workbench to turn those files into product updates an operator could review. I built the Python intake for known XLSX and CSV formats and connected it to the team’s Django backend.

[Overview](../README.md)

## How the system fits together

<picture>
  <source media="(max-width: 600px)" srcset="../assets/catalog-system-mobile.svg">
  <img src="../assets/catalog-system.svg" alt="File intake sends validated products to Django and PostgreSQL. The planned workflow adds React review and a separate FastAPI service for AI suggestions.">
</picture>

**Figure 1.** The file intake and initial catalog load were implemented. The dashed connections show the planned review and AI workflow.

**Django** owns the supplier and product records in **PostgreSQL**. The team’s design gives operators a **React** interface to review changes. A separate **FastAPI** service would suggest categories and attributes, with **Celery** coordinating background work.

## My contribution

A teammate built the service that stores the accepted catalog. I translated supplier columns into the typed product records that service expects. I added validation and tests so a file with missing or invalid data cannot replace a catalog the team has already accepted.

That handoff taught me how to design a Python component around another engineer’s API. The two parts had to agree on what a valid product looks like and when it is safe to write to the database.
