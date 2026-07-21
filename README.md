# Dual Shield

**A "Dual Shield" theory of chordoma immunobiology and developmental dynamics.**

This repository contains a single-cell RNA-seq analysis pipeline and a multi-agent LLM
reasoning layer applied to chordoma, a rare and treatment-refractory bone tumour of the
axial skeleton. The pipeline proposes — and then adversarially critiques — a two-layered
model of chordoma resistance, together with an exploratory sequential targeting strategy
derived from it.

> ⚠️ **Research prototype.** Every finding here is computational (*in silico*). Nothing in
> this repository has been validated *in vitro* or *in vivo*, and none of it constitutes
> clinical guidance. The pipeline's own adversarial review stage returned a
> **conditional approval with a confidence score of 5/10** and identified unresolved
> logical contradictions — see [Adversarial review](#adversarial-review) below.

---

## The Dual Shield hypothesis

| Layer | Mechanism | Status |
|---|---|---|
| **Outer Shield** — immune evasion | Chordoma stem cells evade NK-mediated killing through the **HLA-E / NKG2A** axis rather than the classical PD-1/PD-L1 checkpoint | Supported by the data |
| **Inner Shield** — structural resistance | **Vimentin-driven partial EMT** confers mechanical resistance and an arrested developmental state | Supported by the data |

### Outer Shield — key measurements

| Gene | Role | Mean (CSCs, n=2,078) | % positive (all tumour cells) |
|---|---|---:|---:|
| `HLA-E` | NK inhibitory ligand | 1.765 | 80.0% |
| `B2M` | MHC-I component | 4.019 | 97.5% |
| `CD274` (PD-L1) | Classical checkpoint | 0.015 | 2.8% |
| `MICA` | NK-activating ligand | 0.046 | 8.4% |
| `MICB` | NK-activating ligand | 0.034 | 5.4% |
| `ADAM10` | Sheddase | 0.238 | 32.1% |
| `ADAM17` | Sheddase | 0.152 | 24.9% |

The near-absence of PD-L1 alongside high, stably-presented HLA-E is the central
observation: the classical checkpoint that most immunotherapies target is simply not
engaged in these tumours.

### Inner Shield — key measurements

| Gene | Role | Mean (CSCs, n=2,078) | % positive (all tumour cells) |
|---|---|---:|---:|
| `VIM` | Mesenchymal / mechanical resistance | 2.473 | 86.7% (73.7% within CSCs) |
| `CDH1` (E-cadherin) | Epithelial marker | 0.007 | 0.8% |
| `TBXT` (Brachyury) | Notochordal stem identity | 0.058 | 3.7% within CSCs — see note |

High vimentin with near-total loss of E-cadherin is consistent with a **partial** EMT
state. `TBXT` is subject to scRNA-seq dropout; the pseudotime trend (rolling mean) rather
than the raw per-cell mean is the informative signal, and it shows `TBXT` persisting
across the differentiation trajectory rather than resolving to zero. `TBXT` positivity is
in any case already baked into the annotation itself (`TBXT+` defines the `Tumor_CSC`
label).

> **Note on denominators.** The two columns above have different populations: means are
> computed over chordoma stem cells only, while the percentages are over all tumour cells.
> The synthesis report in `reports/` quotes the same mixed pair. Both figures come from
> `data/pipeline_state.json` in the restricted repository, where per-gene `mean` /
> `pct_nonzero` (all cells) and
> `by_cell_type` (per population) are recorded separately — check there before quoting any
> single number.

### The three-step evidence chain for HLA-E / NKG2A

The interaction is not directly visible in a naive intratumoural analysis. The pipeline
resolves this in three steps:

1. **Intratumoural LIANA** — no HLA-E → NKG2A interaction detected. Why?
2. **NK receptor check** — tumour-infiltrating NK cells express `KLRC1` (NKG2A) and
   `KLRD1` (CD94) at **zero**, while circulating PBMC NK cells from the same patients
   express both. The receptor is lost on infiltration.
3. **Cross-tissue LIANA** — pairing tumour stem cells against *healthy circulating* NK
   cells recovers the **HLA-E → KLRC1_KLRD1** interaction.

Read together, this argues the shield is active and that contact with it is what disables
the NK compartment. **This inference is also the pipeline's most heavily criticised step** —
see the caveats below.

---

## Therapeutic reasoning layer

Given the two-layer model above, the pipeline explores whether the shields could be
addressed in sequence rather than simultaneously — reasoning that the layers are not
independent, and that the order in which they are engaged may matter more than any single
target.

The reasoning proceeds over three stages, each defined by the shared upstream regulator or
resistance layer it addresses:

| Stage | Layer addressed | Intended role |
|---|---|---|
| 1 | Shared upstream regulator of both shields | *Preparation.* Weaken the stromal and transcriptional programme that sustains both layers |
| 2 | Inner Shield (structural / partial-EMT) | *Destabilise.* Remove the mechanical resistance that protects the stem-cell compartment |
| 3 | Outer Shield (immune evasion) | *Expose.* Lift the inhibitory signal so cytotoxic immune cells can engage |

The specific agents considered, their targets, ADMET profiles, and the modelled dosing,
sequencing and timing are **not** part of this repository. They are held in a restricted
companion repository, [`Dual_Shield_Data`](https://github.com/aistanbulresearch/Dual_Shield_Data) (private), under
`data/drug_repurposing.json`, `data/admet_profiles.json` and
`data/treatment_optimization.json`.

> These are exploratory *in silico* outputs of an LLM reasoning layer, recorded so the
> pipeline is reproducible and auditable. They are not a treatment protocol, not a
> recommendation, and have no preclinical or clinical validation. See the adversarial
> review below, which identifies a direct logical conflict in the strategy.

---

## Adversarial review

An independent review agent audits the pipeline's own conclusions. It returned
**CONDITIONAL APPROVAL, confidence 5/10**, confirming internal consistency of the
expression data but raising four unresolved objections:

1. **NK contradiction.** The analysis states that tumour-infiltrating NK cells have *lost*
   NKG2A — yet the stage-3 intervention is premised on engaging exactly that receptor. If
   the receptor is absent on the NK cells actually inside the tumour, there is nothing for
   it to bind there. This is a genuine logical conflict between the proposed mechanism and
   the proposed intervention, and it is unresolved.
2. **ADAM10/17 target ambiguity.** Low sheddase expression is argued to keep HLA-E
   surface-stable, but the literature links ADAM10/17 principally to MICA/MICB shedding;
   the HLA-E link is weakly documented.
3. **No chordoma-specific literature support.** PubMed returns zero chordoma-specific
   evidence for the HLA-E/NKG2A axis, so the finding rests entirely on this computation.
4. **Methodological risk.** Comparing tumour cells against healthy PBMCs in the
   cross-tissue LIANA step is susceptible to biological artefact.

Full review text: `data/adversarial_review.json` in the restricted repository,
[`Dual_Shield_Data`](https://github.com/aistanbulresearch/Dual_Shield_Data).

**Required before any wet-lab escalation:** resolve NK infiltration dynamics in *ex vivo*
3D chordoma organoids; confirm biochemically (Western blot / ELISA) whether ADAM10/17 cleave
HLA-E or MICA/MICB; and use spatial transcriptomics or multiplex IHC to establish physical
proximity between HLA-E⁺ tumour cells and NK cells.

---

## Dataset

| Property | Value |
|---|---|
| Assay | 10x Genomics single-cell RNA-seq |
| Patients | 6 |
| Cells loaded | 114,989 (58,945 tumour + 56,044 PBMC) |
| Cells after doublet removal | 112,418 (58,945 tumour + 53,473 PBMC) |
| Genes | 36,601 |
| Integration | Harmony (batch correction across patients) |

Raw patient data is **not** included in this repository. The notebook expects 10x matrices
under a Google Drive path; see the data-loading cells for the expected layout.

---

## Pipeline architecture

```
Layer 1  Transcriptomics    QC → doublet removal → Harmony integration → clustering → annotation
Layer 2  Hypothesis tests   H1 HLA-E/NKG2A (LIANA)  ·  H2 tryptophan catabolism  ·  H5 pseudotime
Layer 3  LLM agents         Claude Opus 4.7 — hypothesis validation, drug reasoning, optimisation
Layer 4  Ground truth       PubMed  ·  ClinicalTrials.gov  ·  Open Targets
Layer 6  ADMET              Toxicity and pharmacokinetic screening
Layer 3′ Adversarial review Independent audit of Layer 3 output
Layer 7  Synthesis          Final consolidated report
```

The adversarial review stage deliberately runs on a **different model** from the primary
reasoning agent, so the audit is not a model grading its own work.

---

## Repository layout

```
config/
  compounds.json                 Candidate compound definitions (placeholder)
notebooks/
  dual_shield_pipeline.ipynb     Full pipeline code, outputs stripped
results/
  figures/                       All generated figures (PNG)
  tables/                        QC summary and ligand-receptor interaction tables (CSV)
reports/
  pipeline_synthesis_report.md   Final synthesis report
requirements.txt
```

### Restricted material

Compound-level outputs are held in a separate private repository,
**`aistanbulresearch/Dual_Shield_Data`**, because they name specific candidate compounds
and record an exploratory dosing and sequencing model.

| | This repository | `Dual_Shield_Data` (private) |
|---|---|---|
| Notebook code | ✅ | ✅ |
| Notebook recorded outputs | ❌ stripped | ✅ |
| Figures and tables | ✅ | — |
| Synthesis report | ✅ | — |
| `data/`, `verification/` | ❌ | ✅ |
| `config/compounds.json` | placeholder | real definitions |

The notebook here carries the full pipeline **code** but no recorded outputs — the figures
it generated are in `results/figures/` and the tables in `results/tables/`. Access to the
restricted repository is by request to AIstanbul Research Group.

#### Compound configuration

The notebook does not hard-code any candidate compound. It reads them from
`config/compounds.json`, which supplies each agent's name, key, stage, mechanism class,
prompt text and literature-verification query. The file committed here is a **placeholder**
naming `Compound A`/`B`/`C`; the pipeline runs end to end against it, but the agent outputs
will refer to the placeholders.

To reproduce the recorded run, replace it with the real definitions from the restricted
repository — either in place, or by pointing the notebook elsewhere:

```bash
export COMPOUND_CONFIG=/path/to/real/compounds.json
```

The schema is documented in the `_schema` block at the top of the placeholder. Supplying
your own definitions runs the pipeline against a different compound set without touching
any code.

### Figures

| File | Content |
|---|---|
| `TUMOR_doublet_analysis.png`, `PBMC_doublet_analysis.png` | Scrublet doublet score distributions |
| `02_harmony_integration.png` | Integration overview ¹ |
| `03_*_marker_dotplot.png`, `03_*_feature_plot.png` | Marker gene expression, tumour and PBMC |
| `04_annotated_umaps.png` | Annotated cell-type UMAPs |
| `05_H1_Tumor_NK_Communication.png`, `05_H1_Tumor_NK_DotPlot_Fixed.png` | H1 ligand-receptor interactions |
| `06_H2_Metabolic_*.png` | H2 tryptophan / IDO1 pathway activity |
| `07_H5_Pseudotime_UMAP*.png`, `07_H5_Gene_Trends*.png` | H5 pseudotime trajectory, with and without VIM |
| `08_ADAM_Protease_*.png` | ADAM10/17 sheddase expression |

¹ `02_harmony_integration.png` was produced by an earlier run in which the Harmony step
fell back to PCA-only after an embedding shape error. The Harmony integration actually used
by every downstream analysis comes from the subsequent manual-Harmony cell. Both cells are
retained in the notebook so each delivered artefact is reproducible; the figure is kept for
provenance and should not be read as the final integrated embedding.

---

## Running it

The notebook was developed in Google Colab with a high-memory runtime.

```bash
pip install -r requirements.txt
```

The LLM layer requires an Anthropic API key, read from `CLAUDE_API_KEY` — either as an
environment variable or as a Colab secret:

```bash
export CLAUDE_API_KEY="sk-ant-..."
```

No key is stored in this repository. The verification layer calls public APIs
(NCBI E-utilities, ClinicalTrials.gov v2, Open Targets GraphQL) and needs no credentials.

---

## Notes on this release

- The notebook has been cleaned for publication: cells that errored or were superseded by a
  later fix were removed, and the remainder reordered into pipeline sequence. What remains
  is the path that actually produced the artefacts in `results/`.
- Code comments, printed output and recorded results were translated from Turkish to
  English. Numbers, gene symbols, identifiers and API responses are unchanged.
- The notebook's recorded outputs were stripped before publication and are kept in the
  restricted repository. The code is unchanged, so the notebook still documents the method
  in full; it simply carries no stored results.
- The primary reasoning agent was migrated from Google Gemini to **Claude Opus 4.7**
  (`claude-opus-4-7`) via the Anthropic SDK, using adaptive thinking. The recorded outputs
  held in the restricted repository predate that migration and were produced by the
  original model, so a re-run would not reproduce them verbatim.
- The adversarial review cells retain their original model configuration by design.
- No compound is named anywhere in this repository. Compound definitions were moved out of
  the notebook into `config/compounds.json`, of which only a placeholder is published here.

---

## Citation

If you use this work, please cite it as:

> AIstanbul Research Group. *Dual Shield: A two-layered model of immune evasion and
> structural resistance in chordoma.* GitHub, 2026.
> https://github.com/aistanbulresearch/Dual_Shield

---

## License

No license has been chosen yet. Until one is added, all rights are reserved by
AIstanbul Research Group.
