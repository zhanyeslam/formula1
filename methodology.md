# 🧪 Methodology

Our project combines **Knowledge Graph querying** with **Large Language Models (LLMs)** to detect and fill **missing facts** about selected Formula 1 drivers in **Wikidata**.

---

## 🎯 Objective

To enrich existing semantic data by:
- Identifying **gaps** in the RDF descriptions of drivers.
- Prompting LLMs to retrieve and reformat **missing knowledge**.
- Manually verifying and correcting outputs.
- Publishing all methods and findings in a clear, reproducible way.

---

## 👥 Subjects of Analysis

We focused on 4 prominent Formula 1 drivers:

- **Lewis Hamilton** (Q9673)
- **Charles Leclerc** (Q17541912)
- **Kimi Antonelli** (Q112073790)
- **Max Verstappen** (Q2239218)

---

## 🔍 Step 1: SPARQL Queries to Explore the KG

We crafted SPARQL queries using multiple keywords:

- `VALUES`, `ORDER BY`, `FILTER`, `OPTIONAL`, `UNION`, `DISTINCT`, `REGEX`, `LIMIT`, `SERVICE`
- These queries helped **extract existing data** and **highlight what’s missing**

Example:
```sparql
SELECT ?driver ?driverLabel ?partner ?partnerLabel
WHERE {
  VALUES ?driver {
    wd:Q9673 wd:Q17541912 wd:Q112073790 wd:Q2239218
  }
  OPTIONAL { ?driver wdt:P451 ?partner . }
  SERVICE wikibase:label { bd:serviceParam wikibase:language "en". }
}
ORDER BY ?driverLabel
```

This revealed that Charles Leclerc's relationship was missing, while Verstappen and Hamilton had partners listed.

---

## 🤖 Step 2: Prompting LLMs to Fill Gaps

We used **three prompting techniques** across different cases:

| Prompting Style | Example Used |
|------------------|--------------|
| **Zero-shot** | For identifying drivers' teams |
| **Few-shot** | For relationship info (Leclerc) |
| **Chain-of-Thought** | For championship winner analysis (Verstappen) |

Each LLM (ChatGPT, Gemini) received prompts in different formats and produced varied outputs — some correct, some flawed.

---

## 🧱 Step 3: RDF Generation and Manual Correction

LLMs were asked to return **RDF triples using Wikidata ontology**. Their outputs were then:

1. **Evaluated**
2. **Compared** between models
3. **Corrected manually** (invalid Q-IDs, missing qualifiers)

Final RDF was written in **Turtle format** using proper prefixes, properties, and qualifiers.

---

## ✅ Summary of Tools Used

| Tool | Purpose |
|------|---------|
| **Wikidata** | Main knowledge graph |
| **SPARQL** | Querying and gap discovery |
| **ChatGPT / Gemini** | Knowledge generation |
| **Manual editing** | RDF validation and correction |
| **Markdown + GitHub Pages** | Publication and reporting

---

## 📎 Outcome

We successfully enriched Wikidata-relevant facts for all 4 drivers, meeting all project requirements.  
Each case study followed a clear path: **SPARQL → Gap → LLMs → RDF → Validation** — providing both **automation** and **precision** in knowledge enrichment.
