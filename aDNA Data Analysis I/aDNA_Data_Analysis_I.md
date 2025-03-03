# aDNA Data Analysis I: recovering mitochondrial genomes from NGS shotgun data

## Scenario
You have received Next Generation Sequencing (NGS) shotgun data for three skeletons from Sant Llàtzer, a medieval leprosarium in Barcelona: **UF104**, **UF703**, **UF801**. The genetic diversity of *M. leprae* strains across medieval Europe and within leprosaria was high. You would like to know if diverse *M. leprae* strains were maintained locally through community transmission or if the pathogens were being introduced by people traveling from outside of Western Europe.

To see if people were traveling from afar to Sant Llàtzer, you are going to process and analyze NGS data to determine the mitochondrial haplogroups of individuals buried within the leprosarium.

## Part 1: Raw sequencing data and anatomy of a `.fastq` file

DNA libraries are mixed together during sequencing. Sequencing centers generally send you **demultiplexed** `.fastq` files. Demultiplexing is the process of separating the sequences into their own unique sample files based on the unique index sequence combinations you added to the libraries during indexing (Fig. 1). 

## Demultiplexing in NGS

<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/images/FIg1demultiplex.png" alt="SE sequencing" width="1000">

*Figure 1: An overview of Next Generation Sequencing steps (NGS).*

- **Single-end (SE) sequencing:** The DNA fragment is sequenced from only the forward direction, generating one read.  
- **Paired-end (PE) sequencing:** The DNA fragment is sequenced from both directions, resulting in two reads per fragment.  

<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/images/FIg2SE.png" alt="SE sequencing" width="400">

*Figure 2: Depiction of single-end sequencing.*

<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/images/FIg3PE.png" alt="PE sequencing" width="900">

*Figure 3: Depiction of paired-end sequencing.*

The libraries you are working with were **PE sequenced**, so each library is represented by two `.fastq` files, **R1** and **R2**. 

Each sequenced read in a `.fastq` file is described by four rows:

1. A header beginning with `@` containing a unique library identifier and sequencing run info.
2. The base calls of the sequence.
3. A spacer line beginning with a `+`.
4. The quality score for each base call, indicating the probability that a base was called in error.

Fastq files are very large and contain millions to billions of reads. Therefore, you are provided with truncated `.fastq` files to investigate:

- [UF703_head_R1.fastq](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/1.fastq_files/UF703_head_R1.fastq) (first several reads from the forward direction) and [UF703_head_R2.fastq](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/1.fastq_files/UF703_head_R2.fastq) (first several reads from the reverse direction)
- [UF703_tail_R1.fastq](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/1.fastq_files/UF703_tail_R1.fastq) (last several reads from the forward direction) and [UF703_tail_R2.fastq](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/1.fastq_files/UF703_tail_R2.fastq) (last several reads from the reverse direction)

## Tasks:
- **What is the header for the forward read `@SRR15512701.3`?**  
- **What is the ID of the corresponding reverse read?**  
- **Do `UF703_head_R1.fastq` and `UF703_head_R2.fastq` have the same number of reads?**  
- **How many reads were demultiplexed to library UF703?**  

## Part 2: Quality control of sequencing data using `fastqc`

Since you can't check every read manually, we use the tool [**Fastqc**](https://github.com/s-andrews/FastQC) to assess `.fastq` files. It reports 10 quality metrics — some more useful to us than others. 

Open [Sant_Llàtzer_mapping_report](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/Sant_Ll%C3%A0tzer_mapping_report.tsv) in a new tab (right click). Since you cannot edit this file directly, download it using the download icon in the top right corner. The extension is `.tsv`, which stands for "tab-separated values." Open this file using excel to be able to edit the spreadsheet.  

Open the **Fastqc reports** to fill in missing cells in the **Total reads or read pairs** column.  

[UF104_R1_fastqc.html](https://raw.githack.com/Kelzor/Introductory-ancient-DNA-practical/main/aDNA%20Data%20Analysis%20I/2.fastqc_outputs/UF104_R1_fastqc.html)
[UF104_R2_fastqc.html](https://raw.githack.com/Kelzor/Introductory-ancient-DNA-practical/main/aDNA%20Data%20Analysis%20I/2.fastqc_outputs/UF104_R2_fastqc.html)
[UF703_R1_fastqc.html](https://raw.githack.com/Kelzor/Introductory-ancient-DNA-practical/main/aDNA%20Data%20Analysis%20I/2.fastqc_outputs/UF703_R1_fastqc.html)
[UF703_R2_fastqc.html](https://raw.githack.com/Kelzor/Introductory-ancient-DNA-practical/main/aDNA%20Data%20Analysis%20I/2.fastqc_outputs/UF703_R2_fastqc.html)
[UF801_R1_fastqc.html](https://raw.githack.com/Kelzor/Introductory-ancient-DNA-practical/main/aDNA%20Data%20Analysis%20I/2.fastqc_outputs/UF801_R1_fastqc.html)
[UF801_R2_fastqc.html](https://raw.githack.com/Kelzor/Introductory-ancient-DNA-practical/main/aDNA%20Data%20Analysis%20I/2.fastqc_outputs/UF801_R2_fastqc.html)



### Tasks:
- Compare **forward (R1)** and **reverse (R2)** read reports for **UF104**.  
- **Are base quality distributions different?**  
- Pick a metric marked with a red X and explain what it shows about the data.  
- Why are **adapter sequences** overrepresented in **UF104**'s sequencing data?  

You also have **trimmed and merged reads** (using **Adapterremoval2**) and their Fastqc reports.  
- **Why is there only one sample per Fastqc report now?**  
- **How do trimmed/merged reads compare to unmodified reads?**  
- **Why are "per base sequence" and "GC content" flagged?**  
- **Why is sequence length distribution flagged?**  

Fill in the **Total retained reads or read pairs after trimming** and **proportion kept after trimming** columns in the mapping report.  

## Part 3: Alignment of quality filtered sequencing reads

After quality control, the next step is aligning reads to a reference genome (called **mapping**). The reference genome is in **fasta** format — a simple, one-dimensional format for storing consensus sequences. 

- **Open `rCRS.fasta`** — what does fasta format look like?  

The output of mapping is a **.sam/.bam** file:
- **.sam:** Human-readable.  
- **.bam:** Compressed, computer-readable.  

Mandatory fields in `.sam/.bam` include:
- **QNAME:** Sequence read ID.  
- **FLAG:** Bit-coded info about mapping status.  
- **MAPQ:** Mapping quality score.  
- **CIGAR:** A coded explanation of alignment (e.g., `3M7N4M` = 3 mapped bases, 7 bp gap, 4 mapped bases).  

**Filtered files:**  
- **UF703_f4_q25_sortc_markdup.sam** (unmapped reads removed, quality score < 25 removed, duplicates removed).  

### Tasks:
- **Find read `M_SRR15512701.615723`** — what's its mapping quality and CIGAR score?  
- **Are CIGAR strings high-quality? Why?**  

We also use **MapDamage** to rescale base qualities for damaged aDNA reads.  
- **Compare `UF703_f4_q25_sortc_markdup.rescaled.sam` with the original `.sam` file.**  
- **Which lines have rescaled quality scores?**  

## Part 4: Using `qualimap` to determine reference coverage and depth

**Qualimap** assesses genome coverage and depth from `.bam` files. 

- **Open the `.pdf` and `.txt` reports** for your samples.  

Fill in the following columns in the mapping report:
- **Q25 mapped reads after duplicate removal**  
- **Avg length of mapped Q25 reads**  
- **Mean Cov**  
- **SD Cov**  

### Tasks:
- **Interpret a figure from the `.pdf` report** — use the [Qualimap manual](https://hpc.nih.gov/docs/QualimapManual.pdf).  
- Fill in **% ref covered at >=1x** and **% ref covered at >=5x** using `.txt` files.  

Calculate **cluster factor** (library complexity) and **endogenous DNA frequency** using formulas provided.  

## Part 5: MapDamage plots

MapDamage produces aDNA damage plots.  
- **Open `5.MapDamage_outputs`** and review the **fragment misincorporation plots** for each sample.  

### Tasks:
- **What do the top four plots (A, C, T, G frequencies) show?**  
- **Estimate the values for the last four columns in the mapping report from the bottom two plots (C to T and G to A substitutions).**  
- **Based on these plots, do all samples have authentic aDNA?**  

## Part 6: Inspecting variants

Finally, let’s examine genetic variation!  
- Open **IGV** and load the reference genome (`rCRS.fa`).  
- Load the `.bam`, `.rescale.bam`, and `.pmds3filter.bam` files for each sample.  

Manually scan **hypervariable regions** of mtDNA:
- **HVI (16024–16383)**  
- **HVII (57–372)**  

Record polymorphisms like so: **A374C** (A -> C at position 374).

### Tasks:
- **Mark polymorphisms for UF104, UF703, and UF801 in HVI and HVII.**  
- **Compare the three `.bam` files — how do they differ?**  
- **Check variants against known mtDNA polymorphisms at [phylotree.org](https://www.phylotree.org/tree/).**  
- **Can you assign a haplogroup to any individual?**  

---
