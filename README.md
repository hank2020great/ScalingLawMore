# The Quantization Trap: Breaking Linear Scaling Laws in Multi-Hop Reasoning

**ICML 2026 Submission #17502 — Updated Manuscript**

This repository contains the revised version of our submission, incorporating reviewer feedback. Below is a summary of changes.

## Changes in This Revision

**Expanded Related Work (Section 2)**
- Added discussion of quantization-aware inference optimization covering Marlin, AWQ, QuIP#, FlexGen, and Atom
- Clarified how our work extends prior energy-efficiency studies to the multi-hop reasoning regime

**New Pipeline Overview Figure (Figure 1)**
- Added end-to-end evaluation pipeline figure: Model → Quantize → Reasoning → Measure → SI Framework → Trap Detection

**Clarified Quantization Protocol (Section 4.2)**
- Explicitly specified weight-only quantization setup: bitsandbytes NF4 (W4A16) and LLM.int8() (W8A16)
- Scoped the work to memory-bound autoregressive decoding at low batch sizes

**New Cross-Backend Validation (Section 4.5.1)**
- Added AWQ (fused GEMM kernels) vs bitsandbytes NF4 vs FP16 comparison on GSM8K (Mistral-7B, A100-80GB)
- AWQ achieves COR = −0.03 (faster than FP16) and 41% lower energy, fully eliminating the efficiency trap
- Yet accuracy remains 2.4pp below FP16 (40.71% vs 43.14%), confirming Theorem 4.5: accuracy loss is inherent to 4-bit precision, not kernel implementation
- Includes NVML real-time energy monitoring, validating the Quantization Trap under alternative energy measurement

**New Per-Hop Accuracy Stratification (Section 4.6, Table 2, Figure 7)**
- GSM8K accuracy stratified by reasoning depth (1–2, 3–4, 5–6, 7+ steps) across all three precision levels
- FP16→4-bit relative loss nearly doubles from 6.9% (1–2 steps) to 11.6% (3–4 steps)
- Provides first direct empirical evidence for Sequential Deductive Fragility: quantization noise compounds multiplicatively across reasoning hops

**New 14B-Scale Validation (Section 4.7, Table 3)**
- Qwen2.5-14B evaluated on A100-SXM4-40GB (MathQA) and RTX PRO 6000 Blackwell Server Edition (GSM8K)
- Model range now spans 23× (0.6B → 14B) across five GPU architectures (L4, A100-40GB, A100-80GB, H100, RTX PRO 6000 Blackwell)
- At 14B scale, 4-bit accuracy exceeds FP16 (80.4% vs 76.9%) due to stochastic regularization, yet COR reaches 1.35–1.69 and energy increases 1.3–1.8×
- Reveals the Disjunctive Trap: quantization forces a mandatory penalty in either Trust (7B) or Efficiency/Energy (14B) — no configuration achieves "equivalent quality at lower cost"
- All experiments include dual TDP + NVML energy measurement

**Sensitivity Analysis (Remark 3.1, expanded)**
- Added Pareto dominance argument: FP16 dominates on all three pillars simultaneously, so SI ranking is invariant to weight choice for any w_i > 0
- Added discussion of deployment-specific weight tuning

**Limitations Paragraph (Section 5)**
- Acknowledged scope limited to weight-only quantization
- Noted W4A4/W8A8 as distinct regime warranting separate analysis

**Rigorous Proofs (Appendix)**
- All propositions and theorems (Prop. 3.2, Lemma 4.1, Prop. 4.2, Theorems 4.3–4.5) have been carefully reviewed and rigorously proved to ensure correctness

**Writing Quality**
- Fixed typos throughout
- Replaced overwrought language with neutral technical terminology
- Fixed template header placeholder
- Enlarged all figures for print readability
