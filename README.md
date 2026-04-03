# The Quantization Trap: Breaking Linear Scaling Laws in Multi-Hop Reasoning

**ICML 2026 Submission #17502 — Updated Manuscript**

This repository contains the revised version of our submission, incorporating reviewer feedback. Below is a summary of changes.

## Latest Update (Discussion Period, April 3–7)

In response to reviewer feedback during the discussion period, we have made the following additional updates:

- **Expanded Related Work (Section 2):** Added "Quantization and Reasoning Accuracy" paragraph discussing Liu et al. [COLM 2025], Li et al. [2025], and how our work addresses two gaps in the literature: (1) isolated single-dimension evaluation and (2) lack of predictive theoretical mechanisms
- **Qwen2.5-32B Experiments (Section 4.8, Table 3):** Completed on RTX PRO 6000 Blackwell for both GSM8K and MathQA. Key finding: the *same* 32B model exhibits opposite trap behavior depending on batch size — trap dissolved on GSM8K (COR = −0.46, B=16) but active on MathQA (COR = +0.73, B=8)
- **New Theorem 4.6 (Critical Model Scale N\*):** Formalizes the condition under which the efficiency trap dissolves: N\* = φ·B\_W·B / α(π−p). Three predictions validated empirically across 0.6B–32B
- **Corollary 4.7 (70B Prediction):** At frontier scale (N >> N\*), the efficiency trap vanishes but accuracy trap persists — quantization becomes "invisibly" harmful for multi-hop reasoning
- **Model range expanded to 53× (0.6B → 32B)** across five GPU architectures

**Note on formatting:** The current manuscript uses a space-lenient format for review convenience. For the camera-ready version, we will apply the official ICML two-column template and consolidate figures/sections to fit within page limits.

---

## Changes in This Revision

**Expanded Related Work (Section 2)**
- Added discussion of quantization-aware inference optimization covering Marlin, AWQ, QuIP#, FlexGen, and Atom
- Added "Quantization and Reasoning Accuracy" paragraph positioning against Liu et al. [COLM'25], Li et al. [2025], Rajput & Sharma [2024], and Husom et al. [2025]
- Clarified how our work extends prior studies by providing joint three-dimensional diagnosis and predictive theory

**New Pipeline Overview Figure (Figure 1)**
- Added end-to-end evaluation pipeline figure: Model → Quantize → Reasoning → Measure → SI Framework → Trap Detection

**Clarified Quantization Protocol (Section 4.2)**
- Explicitly specified weight-only quantization setup: bitsandbytes NF4 (W4A16) and LLM.int8() (W8A16)
- Scoped the work to memory-bound autoregressive decoding at low batch sizes

**New Cross-Backend Validation (Section 4.5.1, Table 1, Figure 6)**
- Added AWQ (fused GEMM kernels) vs bitsandbytes NF4 vs FP16 comparison on GSM8K (Mistral-7B, A100-80GB)
- AWQ achieves COR = −0.03 (faster than FP16) and 41% lower energy, fully eliminating the efficiency trap
- Yet accuracy remains 2.4pp below FP16 (40.71% vs 43.14%), confirming Theorem 4.5: accuracy loss is inherent to 4-bit precision, not kernel implementation
- Includes NVML real-time energy monitoring, validating the Quantization Trap under alternative energy measurement

**New Per-Hop Accuracy Stratification (Section 4.6, Table 2, Figure 7)**
- GSM8K accuracy stratified by reasoning depth (1–2, 3–4, 5–6, 7+ steps) across all three precision levels
- FP16→4-bit relative loss nearly doubles from 6.9% (1–2 steps) to 11.6% (3–4 steps)
- Provides first direct empirical evidence for Sequential Deductive Fragility: quantization noise compounds multiplicatively across reasoning hops

**New Frontier-Scale Validation: 14B–32B (Section 4.8, Table 3, Figure 8)**
- Qwen2.5-14B evaluated on A100-SXM4-40GB (MathQA) and RTX PRO 6000 Blackwell (GSM8K)
- Qwen2.5-32B evaluated on RTX PRO 6000 Blackwell (GSM8K and MathQA)
- Model range now spans 53× (0.6B → 32B) across five GPU architectures (L4, A100-40GB, A100-80GB, H100, RTX PRO 6000 Blackwell)
- All experiments include dual TDP + NVML energy measurement
- Three scale-dependent trap regimes identified:
  - **Full Trap (0.6B–7B):** accuracy degrades AND efficiency worsens
  - **Efficiency-Only Trap (14B):** accuracy preserved via stochastic regularization, but COR reaches 1.35–1.69
  - **Conditional Dissolution (32B):** trap dissolves on GSM8K (COR = −0.46, B=16) but re-emerges on MathQA (COR = +0.73, B=8) — same model, same GPU, opposite outcomes driven by batch size

**New Theorem: Critical Model Scale for Trap Dissolution (Theorem 4.6)**
- Derives N* = φ·B_W·B / α(π−p): the critical model scale above which the efficiency trap dissolves
- Three predictions validated empirically: (i) larger N → trap dissolves, (ii) larger B → trap dissolves, (iii) larger (π−p) → trap dissolves
- Corollary predicts 70B+ behavior without requiring infeasible single-GPU experiments: efficiency trap vanishes but accuracy trap persists

**Sensitivity Analysis (Remark 3.1, expanded)**
- Added Pareto dominance argument: FP16 dominates on all three pillars simultaneously, so SI ranking is invariant to weight choice for any w_i > 0
- Added discussion of deployment-specific weight tuning

**Updated Limitations (Section 5)**
- Model range: 53× (0.6B–32B), five GPU architectures
- 70B addressed via Theorem 4.6 + Corollary (single-GPU FP16 physically infeasible at ~140GB)
- Scope: weight-only quantization; W4A4/W8A8 noted as distinct regime

**Rigorous Proofs (Appendix)**
- All propositions and theorems (Prop. 3.2, Lemma 4.1, Prop. 4.2, Theorems 4.3–4.6, Corollary 4.7) carefully reviewed and rigorously proved

**Writing Quality**
- Fixed typos throughout
- Replaced overwrought language with neutral technical terminology
- Fixed template header placeholder
- Enlarged all figures for print readability
