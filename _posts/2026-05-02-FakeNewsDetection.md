---
title: Fake News Detection
description: Fine-tuned NVIDIA Nemotron 30B LLM for six-class political statement veracity classification, achieving 71.6% accuracy on the LIAR2 dataset.
author: hamdanalhajeri
date: 2026-05-02 12:00:00 +0000
categories: [Projects, Machine Learning]
tags: [python, nlp, llm, lora, fine-tuning, classification]
pin: false
---

# Fake News Detection

A machine learning system that classifies political statements into six truthfulness categories using the LIAR2 dataset of fact-checked claims from PolitiFact. The model combines statement content with speaker credibility metadata to produce nuanced veracity judgements.

## Overview

Instead of treating each statement in isolation, the system enriches every input with historical speaker data — how often a politician has been rated true, mostly-true, half-true, barely-true, false, or pants-fire in the past. This credibility signal is passed alongside the statement text to a fine-tuned NVIDIA Nemotron 30B model via LoRA adapters.

## Classification Labels

| Label | Meaning |
|-------|---------|
| true | Accurate and verifiable |
| mostly-true | Accurate with minor caveats |
| half-true | Partially correct |
| barely-true | Misleading with a grain of truth |
| false | Factually incorrect |
| pants-fire | Egregiously false |

## Key Features

- **LLM Fine-Tuning** — LoRA (Low-Rank Adaptation) applied to Nemotron 30B via Tinker API (no local GPU required)
- **Speaker Credibility Features** — per-speaker historical rating distributions as explicit model inputs
- **Text Preprocessing Pipeline** — URL removal, stopword filtering, and special character stripping
- **Separated Pipeline** — preprocessing runs once; training reruns with different hyperparameters

## Results

- **Test Accuracy:** 71.59%
- **Macro F1 Score:** 0.717
- **Training:** 3 epochs

## Technical Stack

- **Language:** Python
- **Model:** NVIDIA Nemotron 30B
- **Fine-tuning:** LoRA via Tinker API
- **Libraries:** pandas, scikit-learn, NLTK, matplotlib, torch, transformers
- **Dataset:** LIAR2 (PolitiFact fact-checked statements)

## Getting Started

```bash
git clone https://github.com/HamdanAlhajeri/FakeNewsDetection.git
cd FakeNewsDetection
pip install -r requirements.txt
# Run preprocessing then training notebooks
```
