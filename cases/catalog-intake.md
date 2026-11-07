# Catalog Intake Workbench

Suppliers send price lists in different formats. Our team designed a workbench where an operator could review proposed product changes before sending them to an external catalog. I built the Python intake for known XLSX and CSV formats and connected it to the existing Django service that stores accepted supplier data.

[Overview](../README.md)

## How the pieces fit

<picture>
  <source media="(max-width: 600px)" srcset="../assets/catalog-system-mobile.svg">
  <img src="../assets/catalog-system.svg" alt="File intake sends validated products to Django and PostgreSQL. The planned workflow adds React review and a separate FastAPI service for AI suggestions.">
</picture>

**Figure 1.** Solid connections show the implemented initial load. Dashed connections show the planned review and AI workflow.

**Django** owns the supplier records and the last accepted catalog. In the wider design, **React** gives operators a place to review changes, **Celery** runs background jobs and a separate **FastAPI** service proposes categories and attributes. Accepted changes would then go to an external import service. The part I implemented runs through a development command on a feature branch.

## My part of the handoff

A teammate built the data model and the service for loading an initial catalog. I wrote the intake code that finds the headers, maps supplier columns to typed product records and reports errors against the original rows. I connected the two components so the service receives a complete set of valid products before it writes to PostgreSQL.

That last condition matters because an external full import can mark an omitted product inactive. I fixed a workbook case where the advertised range ended before the final product row. I added regression tests for that case and for a file with one invalid row. Both check that an incomplete import preserves the previously accepted state.

I also added Linux subprocess limits for parsing time, memory and output size. Targeted parser and normalization tests passed in the development review. The Linux subprocess tests were skipped on Windows.
