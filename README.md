[README.md](https://github.com/user-attachments/files/26367617/README.md)
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
- Fixed typos: 
- Replaced overwrought language
- Enlarged all figures for print readability

## Upcoming Additions (Discussion Period)

The following experiments are underway and will be added during the discussion period:

- **14B-scale validation**: Qwen2.5-14B on A100 with dual TDP+NVML energy measurement (extends model range to 23×)
- **Per-hop accuracy stratification**: GSM8K accuracy broken down by reasoning depth (2/4/6/8-step problems) to directly validate Sequential Deductive Fragility
- **Cross-backend comparison**: GPTQ (Marlin fused kernels) and AWQ to quantify how optimized kernels affect COR and B*
