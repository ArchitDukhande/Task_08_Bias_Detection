# Task 08 – Bias Detection in LLM Data Narratives
### Syracuse University — Applied Data Science  
### Author: Archit Dukhande

---

## 1. Introduction

This report analyzes how a Large Language Model (LLM) changes its explanations and player selections based on how a question is framed, even when the underlying basketball dataset remains identical.  
The experiment consists of ten hypotheses (H1–H10), each with two prompts (A and B) that differ only in framing, tone, or emphasis.

All LLM outputs were required to follow a strict JSON schema to avoid variation in format.

A subset of prompts (H3 and H4) were compared with objective ground truth using Python to check whether the LLM’s selections matched the correct numerical values.

---

## 2. Dataset

The dataset contains five anonymous players and six numerical statistics:

- **Points**
- **FG%**
- **3P%**
- **Rebounds**
- **Assists**
- **Turnovers**

Key true facts:
- Highest points → **Player A**
- Highest FG% → **Player B**
- Highest rebounds → **Player B**
- Highest turnovers → **Player B**
- Best 3P% → **Player C**
- Most assists → **Player E**

This dataset stays constant across all 20 prompts.

---

## 3. Hypotheses

Each hypothesis tests one type of bias:

- **H1** – Positive vs negative framing  
- **H2** – Confirmation priming vs neutral prompt  
- **H3** – Turnovers vs rebounds as the priority metric  
- **H4** – Efficiency focus vs volume focus  
- **H5** – “What went wrong” vs “What can improve”  
- **H6** – Leading question vs neutral wording  
- **H7** – Risk emphasis vs upside emphasis  
- **H8** – Remove a player vs mentor a player  
- **H9** – Team decline vs team improvement scenario  
- **H10** – Defense priority vs offense priority  

Prompts only differ by framing — not by new information.

---

## 4. JSON Output Format

All LLM answers used this exact structure:

```json
{
  "pick": "Player X",
  "why": ["reason1", "reason2"],
  "focus": ["offense", "defense", "shooting", "rebounding", "turnovers"],
  "risk_of_error": "low"
}
```

This ensured consistency and allowed objective comparison.

---

## 5. LLM Output Summary

Across the 20 prompts, clear shifts appeared:

- Negative/frustration prompts → **Player A** (due to low 3P% and high turnovers)
- Positive/growth prompts → **Player C** or **Player E**
- Efficiency prompts → **Player C**
- Volume prompts → **Player A**
- Risk prompts → **Player A**
- Upside prompts → **Player C**
- Removal prompts → **Player D**
- Mentorship prompts → **Player E**

These patterns show that framing, not data, drives narrative shifts.

---

## 6. Ground Truth Verification (Python)

Certain hypotheses can be evaluated numerically.

### True best performers:
- **Highest turnovers → Player B**
- **Highest rebounds → Player B**
- **Highest FG% → Player B**
- **Highest total points → Player A**

### LLM Alignment:

| Hypothesis | LLM Pick | Truth | Alignment |
|------------|----------|--------|-----------|
| H3_A (turnovers) | Player B | Player B | ✔️ |
| H3_B (rebounds) | Player B | Player B | ✔️ |
| H4_A (efficiency FG%) | Player C | Player B | ❌ |
| H4_B (points) | Player A | Player A | ✔️ |

### Interpretation of mismatch
- The only misaligned case was **H4_A**, where the LLM picked Player C because it interpreted “efficiency” broadly (3P%, turnover control), instead of strictly using FG%.  
- This is not hallucination — it is a framing-driven interpretation shift.

---

## 7. Findings

### Key observations:

- **Framing strongly influences results** even when the dataset is unchanged.
- **Negative** wording pushes the model toward identifying weaknesses (especially Player A).
- **Positive** or growth-oriented prompts shift focus to efficient or assist-heavy players (C and E).
- **Scenario-based prompts** produce dramatically different outputs with no change in data.
- **Metric-based prompts** are usually correct unless the prompt wording encourages interpretive reasoning instead of strict numerical comparison.
- The experiment shows **bias sensitivity**, not hallucination.

---

## 8. Reproducibility

The project is fully reproducible using:

- The dataset file  
- Hypotheses file  
- Helper instruction file  
- All prompts  
- All JSON model outputs  
- Python verification notebook  
- R Markdown file for final analysis  

Running either the Python notebook or the RMD reproduces the full analysis exactly.

---

## 9. Conclusion

This experiment demonstrates that LLMs can produce significantly different analyses based solely on how questions are framed. When asked for numerical comparisons, the LLM aligns with ground truth in most cases. However, qualitative framing leads to different interpretations even though the dataset remains constant.

The results highlight:
- The importance of **carefully engineered prompts**
- The need for **ground-truth validation** when evaluating model claims
- The significant role that **linguistic framing** plays in shaping LLM output

This project confirms that prompt design directly influences reasoning pathways, and even simple wording changes can lead to different conclusions.

---

