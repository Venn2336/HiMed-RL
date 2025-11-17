# HiMed-RL: A Hierarchical Reward Learning Framework for Clinically-Aware Medical Report Generation

[![Paper](https://img.shields.io/badge/paper-AAAI_2026-blue)](https://aaai.org/example/extended-version)
[![Code](https://img.shields.io/badge/code-Preparing_for_Release!-yellow)](https://aaai.org/example/code)
[![Datasets](https://img.shields.io/badge/datasets-Download-orange)](https://aaai.org/example/datasets)

**HiMed-RL** is a hierarchical reward learning framework designed for automatic Medical Report Generation (MRG). This project aims to address the critical issue of "clinical hallucinations" — factual flaws or medical errors in generated reports — which currently make models untrustworthy for real-world clinical deployment. We introduce a three-level reward mechanism to explicitly prioritize clinical quality and diagnostic consistency.

Our model, **HiMed-3B**, achieves state-of-the-art (SOTA) performance on both in-domain and out-of-domain benchmarks.

---

## 👋 Introduction
Automatic Medical Report Generation (MRG) holds great promise for alleviating the documentation burden on doctors. However, while current methods can produce fluent sentences, they often fail to meet clinical standards of factual accuracy and logical consistency.

To bridge this gap, we proposed **HiMed-RL**. It moves beyond simple N-gram text matching by deconstructing reward learning into three synergistic levels:

1.  **Token-level:** Ensures linguistic fluency and readability.
2.  **Concept-level:** Enforces factual grounding by aligning key medical terms with expert knowledge.
3.  **Semantic-level:** Assesses high-level diagnostic consistency and integrity using a specialized LLM Verifier.

## 🚀 Core Features

* **Hierarchical Reward Design:** Uniquely combines reward signals from three granularities (token, concept, semantic) to holistically optimize report quality.
* **LLM Verifier:** Introduces a powerful LLM as a "semantic referee" to evaluate the report's clinical utility based on **Accuracy**, **Relevance**, and **Completeness** , grounded in STARD 2015 guidelines.
* **Human-inspired Dynamic Reward Adjustment:** Employs a novel training strategy where the model first learns to master basic facts (high weight on low-level rewards) before progressing to complex diagnostic reasoning (high weight on semantic reward). This mirrors the cognitive workflow of human clinicians.
* **SOTA Performance:** HiMed-3B (a 3B parameter model) achieves state-of-the-art performance, outperforming significantly larger models and demonstrating the efficiency of our strategy.

## 🔧 Framework Overview

The HiMed-RL pipeline (Figure 2) integrates a Policy Model, a Reward Model, and our dynamic adjustment strategy.

1.  **Policy Model:** Our **HiMed-3B** model (initialized from Qwen2.5-VL-Instruct-3B) receives a medical image and generates candidate reports (rollouts).
2.  **Reward Model:**
    * **$\mathbb{R}_{\mathfrak{token}}$:** Calculates fluency based on a BLEU-inspired metric.
    * **$\mathbb{R}_{concept}$:** Measures factual coverage using ROUGE-L, METEOR, and a bonus for key medical entities.
    * **$\mathbb{R}_{semantic}$:** The LLM Verifier provides scores for clinical accuracy, relevance, and completeness.
3.  **Dynamic Reward Adjustment:** The total reward $\mathbb{R}_{total}(t)$ is a dynamically weighted sum of the low-level $\mathbb{R}_{low-level}$ and semantic $\mathbb{R}_{semantic}$ rewards. The weights $\alpha_{1}(t), \alpha_{2}(t)$ shift focus from fluency to semantic integrity as training progresses.
4.  **Optimization:** We use the Group Reward Policy Optimization (GRPO) algorithm to optimize the policy model based on the composite reward.

## 📊 Performance Highlights

HiMed-3B excels across all three evaluation levels, particularly in semantic accuracy.

### In-Domain Performance (MIMIC-CXR, CheXpert, IU-Xray)

Our model achieves the highest scores on the semantic-level **RATE** metric across all three datasets, indicating its superior ability to preserve essential medical findings and impressions.

*(See Table 3 for full results)*

### Out-of-Domain Generalization (Padchest-GR)

On the Padchest-GR dataset, HiMed-RL demonstrates impressive generalization capabilities, significantly outperforming strong baselines. This confirms the model learns intrinsic MRG patterns rather than relying on rote memorization.

*(See Table 4 for generalization results)*

## ⚙️ Implementation Details

* **Model:** HiMed-3B
* **Backbone:** Qwen2.5-VL-Instruct-3B
* **LLM Verifier:** `Qwen3-a3b` (30B)
* **RL Framework:** verl
* **RL Algorithm:** GRPO
* **Datasets:** MIMIC-CXR, Chexpert, IU-Xray, Padchest-GR (OOD test)

## 🚀 Getting Started

> **Status:** ⚠️ **Code preparing for release!**
>
> We are currently preparing the official code, training scripts, and model checkpoints for public release. Please "watch" this repository for updates. We will release it soon!
