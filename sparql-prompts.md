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

📎 [Results](https://w.wiki/EF8Q)

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

📎 [Results](https://w.wiki/EEnK)

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

📎 [Results](https://w.wiki/EEpB)

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

📎 [Results](https://w.wiki/EF8b)

---

## 💬 LLM Prompts

### 🔹 Zero-shot Prompt: Lewis Hamilton 2025

Prompt: “What Formula 1 team is Lewis Hamilton racing for in the 2025 season?”

🎯 Purpose:
This was a direct factual query without any examples or step-by-step reasoning.
It was used because Hamilton is a high-profile driver and the answer is widely known in the media.

🤖 LLM Responses:
Gemini: "Lewis Hamilton is racing for Ferrari in the 2025 Formula 1 season. He made a high-profile move from Mercedes to Ferrari, and his contract extends at least until the end of 2026."
✅ Correct, brief, fact-based.

GPT-4: "In the 2025 Formula 1 season, Lewis Hamilton is racing for Scuderia Ferrari. He joined the team after leaving Mercedes at the end of the 2024 season, marking his first time driving for a non-Mercedes-powered team. Hamilton replaced Carlos Sainz and now partners with Charles Leclerc at Ferrari."
✅ Also correct, but more detailed and contextual.

Conclusion: This zero-shot prompt worked well because the fact was simple and well-known.

---

### 🔹 Few-shot Prompt: Kimi Antonelli 2025

Prompt:
Task: Identify the current Formula 1 team of the driver.
Driver: Lewis Hamilton → Scuderia Ferrari
Driver: Max Verstappen → Red Bull Racing
Driver: Kimi Antonelli → ?

🎯 Purpose:
This technique was used because Antonelli is a new driver and may not be strongly represented in training data.
Giving clear examples of similar drivers helped the model infer the correct answer by analogy.

🤖 LLM Responses:
Gemini: "Kimi Antonelli → Mercedes"
✅ Correct.

GPT-4: "Kimi Antonelli → Mercedes"
✅ Correct.

🔍 Comparison:
Both Gemini and GPT-4 answered the same and correctly.

Conclusion: Few-shot prompting clearly improved accuracy for a less famous figure.

---

### 🔹 Chain-of-Thought Prompt: Max Verstappen 2024 Champion

Prompt: “Who won the Formula one World Championship in 2024? Let’s think step by step.”

🎯 Purpose:
This prompt was designed to encourage the model to "think aloud" by reasoning step-by-step.
It’s useful when the fact is recent, complex, or could involve multiple stages/events.

🤖 LLM Responses:
Gemini: "Max Verstappen won the Formula 1 World Championship in 2024. He secured his fourth consecutive title with Red Bull Racing at the Las Vegas Grand Prix."
✅ Concise and correct.

GPT-4: Provided a detailed breakdown: how the season unfolded, Verstappen’s performance, and the final clinch in Las Vegas.
✅ Correct with full reasoning.

Conclusion: CoT was ideal for recent, multi-step knowledge.

---

### 🔹 Few-shot Prompt: Charles Leclerc’s Partner

Prompt:
Task: Provide relationship information for a Formula 1 driver.
Driver: Lewis Hamilton
Partner: Nicole Scherzinger
Type: Unmarried partner
Background: Singer
Since: 2007
Driver: Max Verstappen
Partner: Kelly Piquet
Type: Unmarried partner
Background: Model
Since: 2021
Driver: Charles Leclerc
Partner: ...

🎯 Purpose:
Used to guide the model toward a structured, human relationship description with expected fields.
Especially important for non-explicit, softer data (e.g., relationships).

🤖 LLM Responses:
Gemini: "Partner: Alexandra Saint Mleux
Type: Unmarried partner
Background: Student/Social Media Personality (often described as an art student)
Since: 2023"
✅ Clear, accurate, formatted.

GPT-4: "Partner: Alexandra Saint Mleux
Type: Unmarried partner
Background: Art history student / Influencer
Since: 2023"
✅ Same content.

🔍 Comparison:
Both GPT-4 and Gemini gave information about the partner of Charles Leclerc with given structure. 

Conclusion: Few-shot structure helped both LLMs output the correct partner, background, and year.

---

## ✅ Conclusion

This section showcases how **SPARQL** and **LLMs** can work together to explore and enhance a Knowledge Graph.  
SPARQL provided precision and control, while LLMs offered flexibility and coverage.  
By combining both, we were able to identify missing facts and generate correct, validated RDF triples.  
Every technique—from `ZERO-SHOT` to `CHAIN-OF-THOUGHT`—played a unique role in improving the semantic web.
