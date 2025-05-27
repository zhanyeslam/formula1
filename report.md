---
layout: default
title: Final Project Report
---

# 🧠 Final Report: Formula 1 Drivers in Wikidata

## 🗺️ Semantic Knowledge Graph

Below is the mindmap representing the semantic structure of our enriched Formula 1 dataset:

![F1 Knowledge Graph](/assets/images/f1-mindmap.jpg)

---

## 📍 Project Summary

Throughout this project, we explored the use of **SPARQL queries**, **Wikidata**, and **Large Language Models** (LLMs) to improve the quality and completeness of structured information about Formula 1 drivers.

We began by identifying missing or incomplete data using SPARQL — such as missing team affiliations, championship records, or relationships. To address these gaps, we employed LLMs like ChatGPT and Gemini to retrieve and generate candidate triples. Although the models often made small factual errors (e.g. incorrect Q-IDs), we manually reviewed and corrected the outputs using verified Wikidata references.

We then formatted each enriched fact as an RDF triple consistent with Wikidata ontology, and visualized the full set of connections through the mindmap above.

---

## ✅ Outcome

The project demonstrates how **automated tools and human curation can work together** to support knowledge enrichment on open platforms like Wikidata.  
We have published our full analysis, case studies, and RDF graphs on this website.

<p><a href="/formula1/">← Back to main page</a></p>
