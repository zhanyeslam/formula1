---
layout: default
title: "SPARQL & Prompts"
---

# 🧠 SPARQL Queries and LLM Prompts

This section documents the SPARQL queries and LLM prompts used to extract, verify, and enrich information about Formula 1 drivers in Wikidata. We combined structured querying with intelligent prompting to identify gaps and generate RDF triples.

---

## 🔍 SPARQL Queries

### 1️⃣ Driver Lookup by Name

**Purpose:** Searches for Formula 1 drivers whose names include "Hamilton", "Leclerc", "Antonelli", or "Verstappen" using `FILTER` and `REGEX`.

```sparql
SELECT DISTINCT ?driver ?driverLabel
WHERE {
  ?driver wdt:P106 wd:Q10841764 .
  ?driver rdfs:label ?driverLabel .
  FILTER(LANG(?driverLabel) = "en") .
  FILTER(
    REGEX(?driverLabel, "Hamilton", "i") ||
    REGEX(?driverLabel, "Verstappen", "i") ||
    REGEX(?driverLabel, "Leclerc", "i") ||
    REGEX(?driverLabel, "Antonelli", "i")
  )
}
LIMIT 10
```

📎 [Result](https://w.wiki/EF8Q)

---

### 2️⃣ Nationality or Team (UNION Query)

**Purpose:** Uses `UNION` to retrieve either nationality (P27) or team (P54) for each driver.

```sparql
SELECT DISTINCT ?driver ?driverLabel ?infoLabel
WHERE {
  VALUES ?driver {
    wd:Q9673 wd:Q17541912 wd:Q112073790 wd:Q2239218
  }
  {
    ?driver wdt:P27 ?info .
  }
  UNION
  {
    ?driver wdt:P54 ?info .
  }
  SERVICE wikibase:label { bd:serviceParam wikibase:language "en". }
}
ORDER BY ?driverLabel
```

📎 [Result](https://w.wiki/EEnK)

---

### 3️⃣ Championships Won by Drivers

**Purpose:** Checks which competitions each driver has won using property P2522.

```sparql
SELECT ?driver ?driverLabel ?competitionLabel
WHERE {
  VALUES ?driver {
    wd:Q9673 wd:Q17541912 wd:Q112073790 wd:Q2239218
  }
  ?driver wdt:P2522 ?competition .
  SERVICE wikibase:label { bd:serviceParam wikibase:language "en". }
}
ORDER BY ?driverLabel
```

📎 [Result](https://w.wiki/EEpB)

---

### 4️⃣ Unmarried Partners

**Purpose:** Uses `OPTIONAL` to display known and unknown relationships for each driver (property P451).

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

📎 [Result](https://w.wiki/EF8b)

---

## 💬 LLM Prompts

### 🔹 Zero-shot Prompt: Lewis Hamilton 2025

Prompt: “What Formula 1 team is Lewis Hamilton racing for in the 2025 season?”

LLM Responses:
- Gemini: ✅ Ferrari
- GPT-4: ✅ Ferrari, detailed

Conclusion: Worked well due to simplicity and visibility.

---

### 🔹 Few-shot Prompt: Kimi Antonelli 2025

Prompt: Driver: Hamilton → Ferrari, Verstappen → Red Bull, Antonelli → ?

LLM Responses:
- Gemini: ✅ Mercedes
- GPT-4: ✅ Mercedes

Conclusion: Few-shot format improved accuracy for a newer name.

---

### 🔹 Chain-of-Thought Prompt: Max Verstappen 2024 Champion

Prompt: “Who won the F1 Championship in 2024? Let’s think step by step.”

LLM Responses:
- Gemini: ✅ Verstappen, Las Vegas GP
- GPT-4: ✅ Verstappen with full season reasoning

Conclusion: CoT was ideal for recent, multi-step knowledge.

---

### 🔹 Few-shot Prompt: Charles Leclerc’s Partner

Prompt: Lewis → Nicole, Verstappen → Kelly, Leclerc → ?

LLM Responses:
- Gemini: ✅ Alexandra Saint Mleux, since 2023
- GPT-4: ✅ Same, with role as art student/influencer

Conclusion: Structure guided correct output.

---

## ✅ Conclusion

This section showcases how **SPARQL** and **LLMs** can work together to explore and enhance a Knowledge Graph.  
SPARQL provided precision and control, while LLMs offered flexibility and coverage.  
By combining both, we were able to identify missing facts and generate correct, validated RDF triples.  
Every technique—from `FILTER` to `CHAIN-OF-THOUGHT`—played a unique role in improving the semantic web.
