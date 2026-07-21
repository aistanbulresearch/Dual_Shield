# AIstanbul Platform — Chordoma Pipeline Synthesis Report
**BIOMEDICAL RESEARCH REPORT: Chordoma scRNA-seq Analysis and Therapeutic Targeting Strategy**

**Data Source:** Chordoma scRNA-seq Dataset
**Scope:** 114,989 cells (58,945 tumor + 56,044 PBMC), 6 patients

> **Note on compound names.** This summary describes the proposed intervention at the level
> of stages, molecular targets and mechanism only; the specific candidate compounds are not
> named here. They are recorded in a restricted companion repository
> (`aistanbulresearch/Dual_Shield_Data`, private) under `data/`, together with their ADMET
> profiles and the modelled dosing and sequencing. Everything below is
> exploratory *in silico* output and is not a treatment protocol or a clinical recommendation.

---

### 1. EXECUTIVE SUMMARY
This report synthesizes the analysis results of approximately 115 thousand cells of chordoma single-cell RNA sequencing (scRNA-seq) data obtained from 6 patients. The analysis has revealed that chordoma stem cells (CSCs) escape the immune system via the **HLA-E/NKG2A axis** rather than the classical PD-L1 pathway (Outer Shield), and gain structural resistance through **Vimentin-mediated partial-EMT** (Epithelial-Mesenchymal Transition) (Inner Shield). In light of these findings, a three-stage repurposing strategy is proposed, which sequentially targets the tumor microenvironment and the immune evasion mechanisms. However, an independent "Adversarial Review" process, while confirming the strengths of the hypothesis, has drawn attention to certain logical gaps concerning the status of intratumoral NK cells and protease activity, and has given the project a "Conditional Approval".

---

### 2. BIOLOGICAL FINDINGS

**A. Outer Shield (Immune Evasion Mechanism): CONFIRMED**
In the escape of chordoma cells from the immune system, an alternative pathway has been identified, contrary to the classical checkpoints:
*   **Absence of PD-L1:** Expression of the classical immune evasion marker CD274 (PD-L1) is almost entirely absent in CSCs (mean=0.015, 2.8% positivity).
*   **High HLA-E and B2M:** HLA-E (mean=1.765, 80.0% positivity) and B2M (mean=4.019, 97.5% positivity) are highly expressed in CSCs. This proves a stable MHC-I/HLA-E surface presentation.
*   **NK Cell Interaction:** Cross-tissue LIANA analysis has detected a strong interaction between HLA-E on the tumor and the KLRC1_KLRD1 (NKG2A/CD94) inhibitory receptor complex on healthy PBMC NK cells. The very low level of NK-activating ligands (MICA/MICB) (8.4% and 5.4%) facilitates immune evasion.
*   **Inhibition of Shedding (Cleavage from the Surface):** The low expression of ADAM10 (32.1%) and ADAM17 (24.9%) supports the claim that HLA-E remains stable on the cell membrane.

**B. Inner Shield (Structural Resistance and Partial EMT): CONFIRMED**
*   **Vimentin (VIM) Dominance:** VIM expression is extremely high in CSCs (mean=2.473, 86.7% positivity) and remains high even at the terminal stage in pseudotime analysis.
*   **Loss of E-Cadherin (CDH1):** The epithelial marker CDH1 is almost completely suppressed (0.8% positivity). This confirms the partial-EMT state.
*   **Arrested Embryogenesis:** Despite the scRNA-seq dropout effect, pseudotime analysis shows that TBXT (Brachyury), the main driver of chordoma, persists along the differentiation trajectory.

---

### 3. THERAPEUTIC STRATEGY (Repurposing)

In order to break chordoma's double-layered shield, a sequential combination therapy has been designed. Each stage is defined by the resistance layer or upstream regulator it addresses:

1.  **Stage 1 — TGF-beta inhibition:** *Preparatory Phase.* By inhibiting the master regulator TGF-beta, it loosens the fibrotic stroma and fundamentally weakens HLA-E expression and the EMT/Vimentin cycle.
2.  **Stage 2 — Inner Shield destabilization (Vimentin / ALDH1 axis):** *Breaking of the Inner Shield.* By tearing apart the Vimentin network and inhibiting ALDH1, it drives chordoma stem cells in the partial-EMT state into mechanical stress (anoikis) and apoptosis.
3.  **Stage 3 — NKG2A blockade:** *The Finishing Blow.* It blocks the stable HLA-E shield on cells whose stroma has been weakened and which have been placed under stress. In the absence of PD-L1, the breaking of this axis, which is the main immune evasion route, enables NK cells to destroy the tumor freely.

---

### 4. VERIFICATION RESULTS (PubMed Literature Search)

When the counterpart of the proposed mechanisms in the literature was examined, the findings were seen to be **entirely "de novo" (new) for chordoma**:
*   **Supported General Concepts:** The role of HLA-E/NKG2A in general tumor immune evasion (23 articles), clinical studies of the Stage 3 NKG2A-blocking agent (11 articles) and of the Stage 1 TGF-beta inhibitor (27 articles), chordoma immunotherapy (98 articles).
*   **Those for Which No Chordoma-Specific Evidence Was Found (New Discoveries):** HLA-E/NKG2A immune evasion in chordoma, the partial EMT/Vimentin relationship in chordoma, ADAM10/17-mediated HLA shedding, the specific pseudotime behavior of TBXT in chordoma stem cells.
*   **Limited Evidence:** The Stage 2 agent's targeting of Vimentin in cancer (5 articles).

*(The literature searches were run against the specific compound names; those queries and their full responses are recorded in `verification/pubmed_verification.json` in the restricted
companion repository.)*

---

### 5. ADVERSARIAL REVIEW CRITICISMS

The project received **CONDITIONAL APPROVAL with a confidence score of "5"** from the independent review module.

**Strengths:**
*   The HLA-E, B2M and PD-L1 data are extremely consistent among themselves.
*   The low level of MICA/MICB supports the hypothesis biologically.
*   Cross-tissue LIANA analysis is innovative and methodologically valuable.

**Critical Weaknesses and Logical Contradictions:**
1.  **NK Cell Contradiction:** The analysis states that the NKG2A interaction is absent in *intratumoral* NK cells (exhaustion/receptor loss). However, the Stage 3 intervention targets precisely this receptor. If this receptor is absent in intratumoral NKs, the agent will be ineffective in those cells. This creates a serious logical contradiction between the hypothesis and the therapeutic recommendation.
2.  **ADAM10/17 Protease Target:** It has been claimed that low ADAM10/17 enables HLA-E to remain on the surface. However, in the literature ADAM10/17 is predominantly associated with MICA/MICB shedding; for HLA-E this mechanism is weakly documented.
3.  **Lack of Literature Support:** The finding of zero PubMed evidence for the HLA-E/NKG2A axis in the chordoma-specific context gives rise to the risk that the findings rest entirely on in silico computation.
4.  **Methodological Risk:** In the cross-tissue LIANA analysis, comparing tumor cells with healthy PBMCs is a method open to biological artifacts (fallacies).

---

### 6. CONCLUSION AND NEXT STEPS

This scRNA-seq analysis proposes a new PD-L1-independent, HLA-E/NKG2A-mediated immune evasion mechanism in chordoma and a Vimentin-focused structural resistance network (partial-EMT). The proposed three-stage combination has a theoretically quite strong rationale.

However, in order to resolve the critical contradictions noted in the "Adversarial Review", it is essential that the following steps be taken before the project moves to the in vitro/in vivo validation stage:

1.  **Resolving NK Cell Infiltration Dynamics:** For the Stage 3 intervention to be able to work, *new and active (NKG2A positive)* NK cells must migrate from the peripheral blood into the tumor. Whether the Stage 1 loosening of the stroma will enable this infiltration must be tested in ex vivo 3D chordoma organoid models.
2.  **Verification of ADAM10/17 Targets:** Whether ADAM10/17 cleaves HLA-E or MICA/MICB must be verified biochemically (Western Blot/ELISA) in chordoma cell lines using protease inhibitors.
3.  **Spatial Transcriptomics:** In order to eliminate the possible artifacts in the cross-tissue analysis, the physical proximity of HLA-E-expressing chordoma cells and NK cells within the tumor tissue must be shown by spatial transcriptomics or multiplex immunohistochemistry (mIHC).

**Final Decision:** The hypothesis carries high innovation value. On condition that the logical contradictions are resolved by in vitro experiments, this study could form the basis of a groundbreaking clinical investigation in chordoma immunotherapy.
