# Task 08 – Bias Detection in LLM Data Narratives
### Syracuse University — Applied Data Science  
### Author: Archit Dukhande

---

## Overview

This project examines how Large Language Models (LLMs) change their answers based only on the way a question is framed, even though the underlying dataset stays exactly the same. I use a small five-player basketball dataset and create ten hypotheses (H1–H10), each testing a different type of potential bias such as framing, confirmation bias, leading questions, metric prioritization, or scenario-based reasoning.

Each hypothesis has two prompts:
- Prompt A – framed one way  
- Prompt B – framed in the opposite direction  

The LLM must answer using a strict JSON format so the results remain comparable.

A subset of prompts can be evaluated using ground truth (highest points, highest FG percent, highest turnovers, highest rebounds). These were verified using Python.

---

## Dataset

The dataset contains five players with the following fields: points, field goal percentage, three-point percentage, rebounds, assists, and turnovers.

Important numeric facts:
- Player A scores the most points  
- Player B leads in FG percentage  
- Player B leads in rebounds  
- Player C leads in 3-point percentage  
- Player E leads in assists  
- Player B commits the most turnovers  

All prompts use this exact dataset.

---

## Hypotheses

The ten hypotheses test different bias types including:
- Negative vs positive framing  
- Confirmation prompting  
- Turnover vs rebound prioritization  
- Efficiency vs volume  
- “What went wrong” vs “What can improve”  
- Leading question vs neutral wording  
- Risk framing vs upside framing  
- Removing a player vs mentoring a player  
- Decline season vs improvement season  
- Defense vs offense development priority  

All prompt formatting and output rules are defined in the helper instruction file.

---

## LLM Output Format

Every LLM answer must follow this JSON schema:

```json
{
  "pick": "Player X",
  "why": ["reason1", "reason2"],
  "focus": ["offense", "defense", "shooting", "rebounding", "turnovers"],
  "risk_of_error": "low"
}
```

All 20 JSON outputs (H1_A to H10_B) are stored in the results folder.

---

## Ground Truth Verification

Some hypotheses can be evaluated using pure math.  
Python was used to compute:

- Highest turnovers → Player B  
- Highest rebounds → Player B  
- Highest field goal percentage → Player B  
- Highest total points → Player A  

These checks allowed direct comparison between LLM picks and true values.

### Final alignment results:
- H3_A → aligned  
- H3_B → aligned  
- H4_A → not aligned  
- H4_B → aligned  

The mismatch on H4_A happened because the LLM blended multiple “efficiency” concepts instead of strictly following FG percent.

---

## Findings

Several clear patterns emerged:

- The LLM heavily shifts its narrative based on framing.  
- Negative prompts often highlight Player A due to low three-point efficiency and high turnovers.  
- Positive or improvement prompts highlight Player C or Player E.  
- Scenario prompts produce very different answers even with identical numbers.  
- For metric-based questions, the model mostly aligns with ground truth but can diverge when the framing encourages a more interpretive response.

---

## Reproducibility

The entire experiment is fully reproducible using:

- The dataset  
- The hypotheses file  
- The helper instructions file  
- The R Markdown analysis  
- The Python notebook for verification  
- All prompt and answer files included in the repository  

Running the RMD or the Python notebook will reproduce the analysis exactly.

---

## Conclusion

This project shows that LLMs are highly sensitive to prompt phrasing. Even when given the same dataset, the model’s answer changes depending on framing, leading cues, and scenario wording. Numeric-based questions generally align with ground truth, but narrative prompts show clear shifts in focus.

This demonstrates why prompt design matters and why numerical claims from LLMs should always be cross-checked programmatically.

---
