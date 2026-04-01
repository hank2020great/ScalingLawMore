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

**New Cross-Backend Validation (Section 4.5.1) under NVML energy measurement**
- Added AWQ (fused GEMM kernels) vs bitsandbytes NF4 vs FP16 comparison on GSM8K (Mistral-7B, A100-80GB)
- AWQ achieves COR = −0.03 (faster than FP16) and 41% lower energy, fully eliminating the efficiency trap
- Yet accuracy remains 2.4pp below FP16 (40.71% vs 43.14%), confirming Theorem 4.5: accuracy loss is inherent to 4-bit precision, not kernel implementation
- Includes NVML real-time energy monitoring, validating the Quantization Trap under alternative energy measurement

**Sensitivity Analysis (Remark 3.1, expanded)**
- Added Pareto dominance argument: FP16 dominates on all three pillars simultaneously, so SI ranking is invariant to weight choice for any w_i > 0
- Added discussion of deployment-specific weight tuning

**Limitations Paragraph (Section 5)**
- Acknowledged scope limited to weight-only quantization
- Noted W4A4/W8A8 as distinct regime warranting separate analysis
- Noted ongoing 14B-scale validation

**Rigorous Proofs (Appendix)**
- All propositions and theorems (Prop. 3.2, Lemma 4.1, Prop. 4.2, Theorems 4.3–4.5) have been carefully reviewed and rigorously proved to ensure correctness

**Writing Quality**
- Fixed typos throughout
- Replaced overwrought language with neutral technical terminology
- Fixed template header placeholder
- Enlarged all figures for print readability

## Upcoming Additions (Discussion Period)

The following experiments are underway and will be shared during the discussion period:

- **14B-scale validation**: Qwen2.5-14B on A100 with dual TDP+NVML energy measurement (extends model range to 23×)
- **Per-hop accuracy stratification**: GSM8K accuracy broken down by reasoning depth (2/4/6/8-step problems) to directly validate Sequential Deductive Fragility

