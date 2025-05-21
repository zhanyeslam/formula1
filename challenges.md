---
layout: default
title: "Challenges & Solutions"
---

# ⚠️ Challenges and Solutions

Throughout the KE4H project, we encountered several challenges that tested both our technical skills and creative problem-solving. Below, we outline the key difficulties we faced and how we successfully addressed them.

---

## 1️⃣ Crafting Accurate SPARQL Queries

**Challenge:**  
Formulating SPARQL queries that return precise, meaningful data from Wikidata was more difficult than anticipated. Even small syntax issues or misuse of properties (like using `P54` vs. `P108`) could return empty or misleading results.

**Solution:**  
We thoroughly studied existing SPARQL patterns and refined our use of key constructs like `UNION`, `OPTIONAL`, `FILTER`, and `VALUES`. We also validated each query with manual checks and tested results in the Wikidata Query Service to ensure correctness.

---

## 2️⃣ Q-ID Errors in LLM Outputs

**Challenge:**  
When prompting LLMs like ChatGPT and Gemini to generate RDF triples using the Wikidata ontology, the models often returned incorrect or non-existent Q-IDs. For example, a driver might be linked to a fake team entity, or a property like `P1344` would be used incorrectly instead of `P2522`.

**Solution:**  
We manually verified each Q-ID and property by searching directly in Wikidata. The RDF triples were then rewritten by hand, ensuring each entity and relationship matched the official ontology. This step significantly improved the precision of our enrichment.

---

## 3️⃣ Building and Structuring the Website

**Challenge:**  
Before this project, none of us had experience with GitHub Pages or Jekyll-based site creation. Ensuring that all pages rendered properly with a consistent theme, and that navigation worked smoothly across Markdown pages, required research and experimentation.

**Solution:**  
We explored GitHub Pages documentation and tested various layouts until we found a working configuration. We created `_config.yml`, applied a consistent theme (`jekyll-theme-architect`), and added front matter (`layout`, `title`) to each page. By the end, we had a fully functioning website with clear navigation, responsive layout, and proper content rendering.

---

## ✅ Summary

| Challenge                         | Problem Description                                           | Solution Summary                                |
|----------------------------------|----------------------------------------------------------------|--------------------------------------------------|
| SPARQL Query Design              | Difficult to retrieve correct data from Wikidata              | Iterative testing and refinement of SPARQL logic |
| LLM RDF Errors                   | Incorrect Q-IDs and properties in generated triples           | Manual validation and rewriting using Wikidata   |
| Website Development              | No prior experience with Jekyll or GitHub Pages               | Learned and applied best practices for structure |

Through collaboration, trial-and-error, and self-learning, we turned these challenges into opportunities for growth and ultimately delivered a reliable, well-integrated semantic web project.
