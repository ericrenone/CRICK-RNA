**CRICK - RNA**  
*Open-source platform for RNA science, computation, and discovery*

### Overview

CRICK - RNA is a community-driven, open-source platform that unifies canonical RNA science with reproducible computational tooling, curated datasets, and benchmarked workflows. Named in honor of Francis Crick’s formulation of the central dogma and the wobble hypothesis, the project treats RNA as the operational substrate of cellular information flow — sequence, structure, modification, interaction, regulation, and dynamics — and provides production-grade primitives for every layer of that stack.

Our mission: turn the modern RNA literature into a reproducible, computable, composable substrate that researchers can rely on without re-implementing prior art.

### Foundational Principles

RNA sits at the center of post-transcriptional information processing. Messenger RNAs encode proteins; transfer and ribosomal RNAs execute translation; small nuclear and small nucleolar RNAs orchestrate splicing and rRNA maturation; microRNAs and small interfering RNAs guide post-transcriptional silencing; long non-coding RNAs scaffold ribonucleoprotein complexes and modulate chromatin; circular RNAs act as miRNA sponges, translation templates, and protein decoys; PIWI-interacting RNAs defend genomes against transposons; and chemically modified ribonucleotides — m⁶A, m⁵C, pseudouridine, m¹A, 2′-O-methyl, Inosine — form an epitranscriptomic layer that modulates stability, structure, localization, translation, and immunogenicity.

Three organizing identities run through the codebase. First, the genetic code is a 64→20 surjection with 44 wobble dimensions of degeneracy — a natural error-correcting code that separates amino-acid identity (the projected functional content) from synonymous-codon choice (the regulatory content controlling tRNA pairing, elongation rate, mRNA folding, and immunogenicity). Second, RNA structure is dominated by Watson–Crick and non-canonical base pairing whose pairwise topology lives natively in an N×N×D pair tensor — the same data structure that drives every modern structure-prediction architecture. Third, RNA function is realized through dynamic ensembles, not single conformers, so kinetics, modifications, and binding partners are first-class objects, not afterthoughts.

### Canonical State of the Art

#### RNA Language and Foundation Models

RNA foundation models have converged on encoder transformer backbones pretrained on RNAcentral-derived non-coding RNA corpora using masked language modeling, with model scale, data quality, and structural conditioning as the principal axes of variation.

- **RNA-FM** (Chen et al., *Nature Methods* 2024) — 100M-parameter BERT-style model trained on ~23M ncRNA sequences from RNAcentral, providing embeddings that improve downstream secondary-structure, contact, and functional-element prediction.
- **RiNALMo** (Penić et al., *Nature Communications* 2025) — 650M-parameter model trained on 36M high-quality ncRNA sequences with FlashAttention and rotary positional embeddings; demonstrates strong inter-family generalization for 2D structure tasks.
- **AIDO.RNA** (CMU / GenBio AI, 2024) — 1.6B-parameter encoder, the largest RNA language model to date, trained on 42M curated ncRNA sequences over 30B unique nucleotides; state-of-the-art on 24/26 tasks across the RNA understanding benchmark, including secondary structure, mean ribosome load, mRNA stability, and cross-species splice-site prediction.
- **Uni-RNA** — billion-scale unified DNA/RNA model trained on noisy mixed corpora, demonstrating that data quality outweighs raw token count.
- **ERNIE-RNA** — structure-enhanced pretraining incorporating predicted base-pair priors into attention masks.
- **RNA-MSM** — multiple sequence alignment–based RNA language model, the RNA analogue of MSA Transformer.
- **UTR-LM** and **5′UTR-LM** — domain-specialized models for translation-efficiency and ribosome-load prediction.
- **CaLM** — codon-aware language model for coding-region representation that respects the 64→20 wobble structure.
- **RNAErnie** — type-guided fine-tuning conditioned on Rfam RNA family annotations.
- **RNAGenesis** — generalist generative foundation model coupling a 32-layer transformer encoder with hybrid n-gram tokenization and a latent diffusion decoder for sequence generation with inference-time gradient guidance and beam search.
- **GenerRNA** — autoregressive generative pretrained language model for de novo RNA sequence design, including protein-binding aptamers.

Scaling regularities mirror those observed in protein language models: zero-shot fitness correlation strengthens monotonically with parameter count and high-quality data, larger ncRNA-curated models outperform larger but noisier corpora, and unsupervised log-likelihoods correlate with experimental fitness strongly enough to enable zero-shot variant ranking.

#### Secondary Structure Prediction

Modern 2D structure prediction blends thermodynamic priors with learned pair-probability matrices. Canonical tools in the pipeline:

- **Thermodynamic baselines**: RNAfold (ViennaRNA), Mfold/UNAFold, RNAstructure, LinearFold and LinearPartition for O(n) thermodynamic sampling, CONTRAfold (discriminative SCFG), CentroidFold, EternaFold (parameters trained on Eterna crowd-design data).
- **Hybrid learned-thermodynamic**: MXfold2 (Sato et al., *Nature Communications* 2021) — max-margin training with thermodynamic regularization.
- **End-to-end deep learning**: UFold (encoder–decoder treating the sequence as a 2D image, ~F1 0.91 on ArchiveII), SPOT-RNA and SPOT-RNA2 (transfer-learned, native pseudoknot support), Sincfold, DSRNAFold (*NAR* 2025, integrative deep learning with structural context).
- **Probing-conditioned**: SHAPE-, DMS-, and PARS-conditioned variants that incorporate experimental reactivity as soft constraints.
- **Pseudoknot-aware**: UFold and SPOT-RNA remain the primary deep-learning options; IPknot and Knotty cover the optimization-based regime.

For unseen RNA families (the inter-family generalization gap), thermodynamic anchoring still meaningfully reduces overfitting; the project supports both modes via a unified configuration.

#### Tertiary Structure Prediction

RNA 3D prediction has converged toward two families: AlphaFold-2-style end-to-end with explicit pair-tensor refinement, and AlphaFold-3-style diffusion-based all-atom generation.

- **AlphaFold 3** (Abramson et al., *Nature* 2024) — Pairformer + multi-cross-diffusion module; current SOTA across RNA, protein, ligand, and complex prediction. License-restricted weights but reproducible architectures.
- **Boltz-1** (Wohlwend et al., MIT 2024) and **Boltz-2** (MIT + Recursion 2025) — open-source AlphaFold-3-class with affinity prediction in Boltz-2.
- **Chai-1** (Chai Discovery 2024) — open biomolecular complex prediction with diffusion sampling.
- **Protenix** (2025) — open AlphaFold 3 reproduction; RLM-augmented Protenix matches AF3 on CASP-RNA TM-scores with RNA language model conditioning.
- **HelixFold3** (Baidu) — open AlphaFold 3 reimplementation.
- **RoseTTAFold All-Atom** and **RoseTTAFoldNA** (Baek et al., 2024) — generalist all-atom and nucleic-acid-specialized variants.
- **RNA-specialist end-to-end**: RhoFold+ (Shen et al., *Nature Methods* 2024) integrating RNA-FM embeddings, NuFold (Kagaya et al., 2025) with flexible nucleobase-center frames, DRfold and DRfold2, trRosettaRNA, DeepFoldRNA.
- **Hybrid energy-minimization**: FARFAR2, SimRNA, IsRNA, and Vfold-based pipelines, used as refinement stages downstream of deep-learning predictors.

Empirically, AlphaFold 3 attains the strongest sequence-length robustness on CASP-RNA; specialist end-to-end models remain competitive on smaller targets and degrade gracefully on orphan RNAs lacking MSA, especially when conditioned on RNA language model embeddings. MSA depth, structural diversity in PDB nucleic-acid entries, and template availability remain the principal accuracy ceilings.

#### Inverse Folding and De Novo Design

Sequence design conditioned on 2D or 3D structure has become a first-class workflow:

- **gRNAde** (Joshi & Liò, ICLR 2025 spotlight) — SE(3)-equivariant geometric deep learning for 3D-structure-conditioned RNA inverse design, validated in blinded Eterna competitions at expert-level success rates on pseudoknotted targets and used to engineer functional RNA polymerase ribozymes.
- **RDesign** — hierarchical, data-efficient representation learning for tertiary-structure-based RNA design.
- **RNAGenesis** design head — latent-diffusion sequence generation with gradient and beam-search guidance for aptamer and sgRNA scaffold optimization (sub-nanomolar IGFBP3 aptamers, sgRNA scaffolds outperforming wild-type for AAVS1 and B2M knockout).
- **GenerRNA** — protein-binding aptamer generation by fine-tuned generative language modeling.
- **NA-MPNN** — ProteinMPNN-style sequence design for nucleic acids.
- **Structure-to-sequence aptamer design** (Iwano et al., *Nature Computational Science* 2024) — experimentally validated light-up aptamer discovery.
- **Restricted Boltzmann machines** for family-specific riboswitch design (SAM-I and others), interpretable and robust at low data scales.
- **Hybrid energy-based / covariation models** — structure-aware design that extrapolates beyond training distributions for Azoarcus ribozyme and synthetic ligand-binding RNA families.

Zero-shot design from genomic foundation models — using unsupervised log-likelihood to rank designed variants — is now a standard baseline; AIDO.RNA (1.6B) and RiNALMo (650M) lead this regime, with inverse-folding models adding orthogonal structural constraints at a fraction of the parameter count.

#### mRNA Therapeutics: UTRs, Codons, and Stability

The therapeutic mRNA stack decomposes into: 5′ cap analog → 5′ UTR → open reading frame (codon-optimized, modified-nucleoside substituted, e.g. N1-methylpseudouridine) → 3′ UTR → poly(A) tail. Each layer has dedicated predictive and generative models.

- **5′ UTR / ribosome load**: Optimus 5-Prime (the seminal MRL CNN trained on polysome-profiling MPRA libraries), UTR-LM, UTailoR (*iScience* 2025) coupling a discriminative MRL predictor with a generative sequence model, and deep-learning-designed 5′ UTRs supporting megaTAL gene editing across cell lines.
- **3′ UTR / stability**: ML-driven 3′ UTR design from high-throughput stability assays; Saluki-style models for mRNA half-life from sequence and codon usage.
- **Codon optimization**: CodonBERT, CaLM, and CodonFM (Arc Institute + NVIDIA, January 2026) for codon-usage-bias-aware optimization that respects tRNA pool composition, mRNA secondary structure stability, and translation elongation kinetics.
- **Combinatorial UTR pairs**: empirically validated combinations (e.g. designed 5′UTR with IGHG2 + mtRNR1 3′UTRs) that exceed reference mRNA-1273 expression by >130%.
- **Modified-nucleoside substitution**: pseudouridine and N1-methylpseudouridine modeling for immunogenicity reduction and translation enhancement.

#### Splicing Prediction

Pre-mRNA splicing remains one of the highest-yield targets for variant interpretation:

- **SpliceAI** (Jaganathan et al., *Cell* 2019) — 10kb-context dilated residual CNN; the de facto standard for splice-altering variant prediction.
- **Pangolin** (Zeng & Li, *Genome Biology* 2022) — tissue-specific (heart, liver, brain, testis) splice-site usage trained across human, macaque, mouse, and rat.
- **SpliceTransformer** (*Nature Communications* 2024) — transformer-based tissue-specific predictor; explains ~60% of intronic and synonymous pathogenic ClinVar variants as splicing-mediated.
- **MMSplice / MTSplice** — modular tissue-aware splicing.
- **AbSplice** — combines SpliceAI with RNA-seq evidence for aberrant-splicing prediction.
- **CADD-Splice** — meta-predictor stacking multiple splice-scoring tools.
- **MaxEntScan**, **HAL**, **Spliceator** — classical baselines retained for reproducibility.

#### Epitranscriptomics: m⁶A, m⁵C, Ψ, and Beyond

The epitranscriptome modulates stability, structure, translation, and immunogenicity. The platform integrates:

- **CHEUI** (*Nature Communications* 2024) — convolutional models predicting m⁶A and m⁵C jointly at single-molecule resolution from nanopore direct RNA sequencing signals.
- **m6ATM** (*Briefings in Bioinformatics* 2024) — WaveNet-DSMIL multiple-instance-learning architecture for site-level m⁶A and stoichiometry, robust at low (~20%) modification ratios.
- **m6Anet**, **EpiNano**, **xPore**, **Dorado / Remora** — direct RNA-seq modification calling for the broader epitranscriptome.
- **DRACH motif** and downstream METTL3/METTL14/WTAP writer, FTO/ALKBH5 eraser, and YTH-domain reader modeling, with stoichiometry and cell-state context.

#### Non-coding RNAs

The platform treats every major ncRNA class as a first-class object:

- **microRNAs**: TargetScan, miRanda, DIANA-microT, deep-learning target predictors with seed-pairing and 3′UTR-context features; miRBase integration.
- **siRNAs**: design rules for thermodynamic asymmetry, GC content, seed-region off-targeting, and chemical modification patterns.
- **antisense oligonucleotides (ASOs)**: gapmer design, MOE / cEt / LNA chemistry encoding, and off-target screening across the transcriptome.
- **long non-coding RNAs**: linc2function-style coding-potential classifiers, subcellular localization predictors, structural-domain annotation, and lncRNA–disease association graph-learning predictors.
- **circular RNAs**: back-splice junction calling, IRES and m⁶A-driven cap-independent translation, ORNA/CirPure/Clean-PIE engineered circularization, and circRNA–drug-resistance prediction.
- **piRNAs, snoRNAs, snRNAs, tRNAs**: type-specific search, modification annotation, and structure prediction.
- **riboswitches and aptamers**: SAM, TPP, FMN, guanine, theophylline, and synthetic aptamer families with secondary-structure ON/OFF state modeling.

#### Circular RNA Therapeutics

circRNAs offer extended half-life, lower innate immune activation when purified properly, and durable protein expression:

- **Engineered circularization platforms**: ORNA (group I intron permuted-PIE), CirPure and CirPrecise (AI-guided precursor secondary-structure design), Clean-PIE (split-site scoring within IRES or ORF).
- **Translation initiation**: viral and synthetic IRES selection, cap-independent translation enhancers, m⁶A-driven initiation.
- **Impurity-aware QC**: linear-RNA and dsRNA byproduct profiling, since immunogenicity claims are platform- and purification-dependent.
- **Therapeutic modalities**: protein-coding circRNAs, decoy circRNAs (miRNA / RBP sponges), circRNA vaccines (including HER2 VLP-displayed antigens with EPM/EABR motifs).

#### RNA–Protein Interactions

- **CLIP-seq-trained predictors**: iDeepS, DeepCLIP, GraphProt2 (graph neural networks over secondary structure), PrismNet (structure-aware), RBPNet, RBPsuite.
- **Zero-shot RBP prediction**: ZeRPI (contrastive learning over GNN representations for unseen RBPs), ZHMolGraph (*Communications Biology* 2025) combining GNN with unsupervised RNA and protein language models for orphan-pair prediction.
- **eCLIP / iCLIP / PAR-CLIP** integration with ENCODE RBP catalogs and the ~2,000-RBP human RNA-binding proteome.
- **Structure-based binding**: RoseTTAFoldNA, AlphaFold 3, and Boltz-2 for protein–RNA complex prediction; surface-feature attribution distinguishing RNA-binding from DNA-binding interfaces by electrostatics and hydrophobicity.

#### RNA-Targeting Small Molecules

Small-molecule RNA drug discovery is a fast-moving frontier driven by 2025–2026 industry partnerships (Skyhawk, Wayfinder, Remix, Arrakis):

- **Binding-site identification**: RNAsite, Rsite2, deep-learning pocket detectors over 3D RNA surfaces.
- **Ligand binding prediction**: RNAmigos and RNAmigos2 (graph-based), RNAsmol, SMARTBind (best-in-class on the 16-target 2025 PDB benchmark, 0.937 mean rank percentile).
- **Riboswitch targeting**: TPP, FMN, guanine, c-di-GMP, glmS, and SAM riboswitches with crystallographic ligand-binding pockets.
- **Disease-associated tertiary motifs**: targeting CUG/CCUG/CAG repeat expansions in myotonic dystrophy, Huntington’s, ALS/FTD (C9orf72); RNA degraders (RIBOTACs) coupling RNA-binding scaffolds with RNase recruiters.
- **RNA-binding-protein modulators**: small molecules disrupting RBP–RNA interfaces in cancer, neurodegeneration, and autoimmunity (Skyhawk-style splice modulators such as risdiplam as proof-of-concept).
- **Fragment-based discovery**, **DNA-encoded libraries (DEL)**, and **small-molecule microarrays (SMM)** as upstream screening modalities feeding the computational stack.

#### CRISPR-Cas13 RNA Editing

Type VI CRISPR systems target single-stranded RNA without genomic modification:

- **RfxCas13d / CasRx**: high knockdown efficiency, collateral cleavage at high expression levels.
- **PspCas13b**: improved specificity, capable of depleting circRNAs without affecting cognate linear transcripts.
- **High-fidelity variants**: hfCas13d (Yang lab) with <5% efficiency loss vs. RfxCas13d and dramatically reduced trans-cleavage; hfCas13X for AAV-compatible compact delivery.
- **Guide-design models**: TIGER (CNN-based, AUC ~0.90), DeepCas13 (hybrid CNN–RNN integrating sequence, structure, and context, AUC ~0.82 with superior interpretability), and crRNA efficiency prediction over the 127K-guide RNAtargeting.org screen.
- **Engineered guides**: circular gRNAs for biostability, transient mRNA-encoded Cas13 expression for therapeutic editing (e.g. ADAR1 knockdown in TNBC immunotherapy).
- **ADAR-recruiting editing**: REPAIR, RESCUE, and LEAPER-style A-to-I and C-to-U RNA editing without ectopic effectors.

#### Lipid Nanoparticle Delivery

The four-component LNP — ionizable lipid, helper phospholipid, cholesterol, PEG-lipid — remains the dominant clinical delivery vehicle:

- **Ionizable lipids**: DLin-MC3-DMA (Onpattro), SM-102 (Moderna), ALC-0315 (Pfizer/BioNTech), and next-generation biodegradable ionizables.
- **Organ targeting (SORT)**: cationic DOTAP → lung, anionic 18PA → spleen, ionizable DODAP → liver; cholesterol modulation for extra-hepatic distribution.
- **Ligand-decorated LNPs**: mannose-PEG for liver sinusoidal endothelial cells (LSECs) and Kupffer cells, GalNAc for hepatocyte ASGPR, antibody-conjugated for T-cell targeting.
- **AI-guided design**: D-MPNN over ionizable-lipid SMILES; protein-corona prediction with SHAP-attributed feature importance; image-based ML on LNP-treated cell phenotypes for efficacy prediction.
- **Modality-specific formulation**: siRNA (Onpattro, Givlaari, Leqvio), mRNA vaccines and replacement therapies, saRNA and circRNA carriers with adjusted N/P ratios and PEG densities.

#### Single-Cell Foundation Models and Virtual Cells

The transcriptomic state of a cell is the integrated output of the RNA stack:

- **scGPT** (Cui et al., *Nature Methods* 2024) — generative pretrained transformer over 33M cells; cell-type annotation, multi-batch integration, perturbation prediction, gene network inference.
- **Geneformer** (Theodoris et al., *Nature* 2023) — transfer learning for network biology with rank-value encoding.
- **scFoundation** (*Nature Methods* 2024) — large-scale single-cell transcriptomics foundation model.
- **CellFM** (*Nature Communications* 2025) — 100M-cell pretrained model.
- **scBERT**, **Universal Cell Embeddings (UCE)**, **GeneCompass** (knowledge-informed cross-species), **RegFormer** (gene-regulatory hierarchies).
- **State** (Arc Institute 2025) and **Stack** (Arc Institute 2026) — virtual-cell foundation models for perturbation response prediction at the 100M-cell scale.
- **Tahoe-100M** — 100M-cell perturbation atlas underwriting virtual-cell training.
- **GEARS** — graph-based multi-perturbation effect prediction.

Empirical benchmarks across 2025–2026 show that single-cell foundation models lead on representation-quality and zero-shot tasks but do not yet uniformly beat task-specific baselines on every downstream evaluation; the platform exposes both regimes and reports task-conditional performance.

#### RNA Velocity and Transcriptional Dynamics

- **ODE-based**: velocyto, scVelo (stochastic and dynamical), MultiVelo (multi-modal RNA + ATAC), CellRank for fate mapping.
- **Bayesian / deep generative**: veloVI (variational inference with uncertainty quantification), VeloVAE, Pyro-Velocity, cell2fate.
- **Cell-specific kinetics**: DeepVelo (GCN-based gene- and cell-specific splicing/degradation rates), latentVelo, cellDancer, UniTVelo.
- **Metabolic-labeling**: Dynamo (tscRNA-seq, sci-fate, scNT-seq) reconstructing continuous transcriptomic vector fields beyond steady-state assumptions.
- **Regulatory-coupled**: RegVelo unifies velocity with gene regulatory network inference; PRESCIENT for fate prediction under perturbation.

#### Genomic and Cross-Modal Foundation Models

Long-context genomic models close the loop from DNA sequence to RNA expression and function:

- **Evo 2** (Arc Institute + NVIDIA, *Nature* 2026) — 40B-parameter StripedHyena model with 1M-token context spanning DNA, RNA, and protein; trained on 9.3T nucleotides across all domains of life.
- **HyenaDNA**, **Nucleotide Transformer**, **DNABERT-2**, **Caduceus** — long-context genomic encoders.
- **Borzoi** and **Enformer** — sequence-to-expression and sequence-to-RNA-seq-track predictors with tissue-specific outputs.
- **CodonFM** (Arc Institute + NVIDIA, 2026) — codon-grammar foundation model bridging DNA and amino-acid space.

### Best Practices

The platform enforces a small number of opinionated defaults that reflect what works in practice:

- **Data provenance**: every dataset is versioned, hashed, and cross-referenced to RNAcentral (release 26 in 2026, ~45M sequences across 52 expert databases, with gene-level entries for 204 organisms), Rfam, PDB, bpRNA, RNA-Puzzles, CASP-RNA, ROBIN, and the Stanford RNA 3D Folding Challenge.
- **Reproducibility**: containerized environments (Docker + Apptainer), pinned dependency manifests, deterministic seeds where supported, automated benchmark regression on every pull request.
- **Inter-family evaluation**: secondary-structure and 3D models are evaluated on held-out Rfam families and ArchiveII with sequence-identity-clustered splits; within-family numbers are reported alongside but never alone.
- **MSA hygiene**: alignment depth and entropy are tracked per target; predictions on orphan RNAs are flagged and routed to MSA-free pipelines (language-model-only inference).
- **Modification awareness**: any pipeline consuming or producing mRNA respects the modified-nucleoside chemistry (Ψ, m¹Ψ, 5mC, m⁶A) declared in the input.
- **Uncertainty quantification**: confidence is a first-class output — pLDDT, pTM, ipTM, per-pair confidence, ensemble disagreement, and posterior variance for Bayesian velocity and perturbation models.
- **Off-target screening**: every guide-design, ASO-design, or siRNA-design output is paired with transcriptome-wide off-target prediction (Cas13 collateral cleavage prediction for Cas13 guides, seed-region scanning for siRNAs/miRNAs).
- **Multi-modal integration**: sequence, structure (2D and 3D), modification, expression, and interaction layers are joined through shared identifiers, not lossy text descriptions.
- **Scalable compute**: pipelines run on laptops for prototyping and on SLURM, Kubernetes, and Ray clusters for production; pair-tensor and SE(3) operations are kernel-fused where supported by the underlying accelerator.
- **Generalization auditing**: every released model card reports performance on held-out families, cross-species transfer, and degradation curves under sequence-identity reduction.

### Resources and Tools

- **Core Library**: `crick-rna` Python package — sequence and structure I/O, predictor wrappers, embedding generation, design pipelines, modification annotation, splicing analysis, single-cell integration.
- **Datasets**: curated RNAcentral, Rfam, bpRNA, RNA-Puzzles, CASP-RNA, ROBIN, RBPbase, POSTAR3, and CLIP-seq corpora with provenance manifests.
- **Models Hub**: unified inference interfaces for RNA-FM, RhoFold+, NuFold, RiNALMo, AIDO.RNA, ERNIE-RNA, UTR-LM, CaLM, RNAGenesis, SpliceAI, Pangolin, SpliceTransformer, gRNAde, scGPT, Geneformer, veloVI, Dynamo, Evo 2 inference clients, and AlphaFold 3 / Boltz-2 / Chai-1 / Protenix / HelixFold3 wrappers where licensing permits.
- **Benchmarks**: reproducible harnesses for secondary structure (ArchiveII, bpRNA-TS0, bpRNA-new), 3D (CASP-RNA, RNA-Puzzles), splicing (GTEx, ClinVar splice-altering variants), and single-cell (Tahoe-100M, Norman-Adamson-Replogle Perturb-seq).
- **Documentation & Tutorials**: end-to-end walkthroughs for mRNA vaccine design, riboswitch engineering, CRISPR-Cas13 guide design, RNA aptamer discovery, splice-variant interpretation, and single-cell perturbation modeling.
- **Community**: GitHub Issues, Discord, and open office hours.

### Quick Start

```bash
pip install crick-rna
```

```python
from crick_rna import RNASequence, StructurePredictor, ModificationAnnotator, Designer

seq = RNASequence("AUGGCUCUGUGGAUGCGCCUCCUGCCCCUGCUGGCGCUGCUGGCCCUCUGGGGACCUGAC")

# Foundation-model embeddings
embedding = seq.embed(model="aido-rna-1.6b")

# Secondary and tertiary structure
ss = StructurePredictor.predict_2d(seq, model="mxfold2")
tertiary = StructurePredictor.predict_3d(seq, model="rhofold-plus")

# Epitranscriptomic annotation
modifications = ModificationAnnotator.annotate(seq, marks=["m6A", "m5C", "psi"])

# Inverse design conditioned on a target 3D backbone
designs = Designer.inverse_fold(target_backbone="ribozyme.pdb", model="grnade", n=64)
