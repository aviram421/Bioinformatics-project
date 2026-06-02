# Contributions of mRNA Sequence and Secondary Structure to Roquin-1 Binding Specificity

**Aviram Siginur, Amit Rendlich**  
Introduction to Bioinformatics – 136158

---

## Overview

This project investigates the relative contributions of mRNA sequence composition and RNA secondary structure to the binding specificity of **Roquin-1 (RC3H1)**, an RNA-binding protein (RBP) that regulates mRNA stability in immune cells.

Roquin-1 is classically associated with binding the **Constitutive Decay Element (CDE)** — a stem-loop motif in the 3′UTR of target transcripts. However, many PAR-CLIP binding sites lack a canonical CDE and instead contain degenerate, AU-rich sequences. This project examines whether RNA secondary structure (specifically loop accessibility) plays a role in determining binding specificity beyond sequence alone.

---

## Dataset

- **PAR-CLIP** data from human HEK293 cells expressing Roquin-1 (RC3H1)
- ~3,800 target mRNAs and >16,000 binding sites
- Format: BED8 (peak summit and binding score included)
- GEO accession: [GSM1144507](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSM1144507)
- Coordinates lifted to **hg38** prior to analysis

---

## Methods

### 1. Preprocessing
- BED8 binding sites were imported into R using `GenomicRanges` and `rtracklayer`
- Sites were annotated with `TxDb.Hsapiens.UCSC.hg38.knownGene` to classify genomic regions
- Binding sites overlapping 3′UTRs were extracted
- 51-nucleotide windows centered on the peak summit were generated and exported as FASTA

### 2. Sequence Motif Analysis
- Enriched k-mers (lengths 5–7) were identified using **DRIMust**
- Motifs visualized on volcano plots (enrichment vs. significance)
- 6-mers selected as optimal length; degenerate sequence logos generated
- Motif enrichment validated against shuffled background sequences using **RBPmap** and Fisher's exact test

### 3. Secondary Structure Analysis
- RNA secondary structures predicted with **ViennaRNA (RNAfold)**
- ΔG values compared between binding and shuffled background sequences (Wilcoxon test)
- Motif loop/stem localization computed per sequence: motifs classified as *loop-like* when <50% of bases are paired
- Loop enrichment compared between binding and background using Fisher's exact test
- Structural context preserved even in highly stable (low ΔG) outlier sequences

### 4. Structural Visualization
- Representative binding/background sequence pairs visualized with RNA fold diagrams
- 3D binding simulation referenced from **PDB** (ROQ domain–mRNA complex)

---

## Tools

| Tool | Purpose |
|---|---|
| R / RStudio | Data processing and visualization |
| UCSC Genome Browser | Genomic annotation and coordinate conversion |
| DRIMust | Discovery of enriched RNA sequence motifs |
| RBPmap | Validation of motif enrichment in binding sequences |
| ViennaRNA (RNAfold) | RNA secondary structure prediction and ΔG calculation |
| PDB | 3D structural reference for Roquin-1–mRNA binding |

---

## Key Results

- **85.3%** of binding sites are located in 3′UTRs, consistent with the literature
- Enriched motifs are **degenerate and AU-rich**; canonical CDE (UCYRYGA) is underrepresented
- Binding sequences have significantly **lower predicted ΔG** (Wilcoxon p ≈ 1.27 × 10⁻¹¹²), indicating greater global structural stability
- Enriched motifs preferentially occur in **loop-like regions** (loop enrichment ≈ 1.133, Fisher p ≈ 0.015)
- Loop localization is preserved even in highly stable sequences (low ΔG outliers), confirming that **motif accessibility is independent of global stability**
- Fisher's exact test confirms that **structural accessibility (loop) is more important than stability alone** for binding

---

## Conclusions

The results support a **dual-layer control model** for Roquin-1 binding:

1. The **ROQ domain** recognizes global RNA secondary structure (stable stem-loop)
2. The **Zinc finger domain** recognizes AU-rich sequence motifs in accessible loop regions

Binding requires both a **stable stem-loop structure** and an **accessible loop motif** — consistent with known structural data from PAR-CLIP experiments and PDB co-crystal structures. This dual mechanism enables specific post-transcriptional regulation while preventing spurious binding to AU-rich transcripts without appropriate structure.

---

## References

1. Murakawa, Y., Hinz, M., Mothes, J. et al. RC3H1 post-transcriptionally regulates A20 mRNA and modulates the activity of the IKK/NF-κB pathway. *Nat Commun* 6, 7367 (2015). https://doi.org/10.1038/ncomms8367
2. Paz I, Kosti I, Ares M Jr, Cline M, Mandel-Gutfreund Y. RBPmap: a web server for mapping binding sites of RNA-binding proteins. *Nucleic Acids Res.* 2014.
3. Leibovich L, Paz I, Yakhini Z, Mandel-Gutfreund Y. DRIMust: a web server for Discovering Rank Imbalanced Motifs Using Suffix Trees. *Nucleic Acids Res.* 2013.
4. Lorenz R, Bernhart SH, et al. ViennaRNA Package 2.0. *Algorithms for Molecular Biology* 6(1):26, 2011. doi:10.1186/1748-7188-6-26
5. Berman HM et al. The Protein Data Bank. *Nucleic Acids Research* 28:235–242 (2000). https://doi.org/10.1093/nar/28.1.235
