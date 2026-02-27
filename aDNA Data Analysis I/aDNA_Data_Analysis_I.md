# aDNA Data Analysis I: recovering mitochondrial genomes from NGS shotgun data

## Scenario
You have received Next Generation Sequencing (NGS) shotgun data for three skeletons from Sant Llàtzer, a medieval leprosarium in Barcelona: **UF104**, **UF703**, **UF801**. The genetic diversity of *M. leprae* strains across medieval Europe and within leprosaria was high. You would like to know if diverse *M. leprae* strains were maintained locally through community transmission or if the pathogens were being introduced by people traveling from outside of Western Europe.

To see if people were traveling from afar to Sant Llàtzer, you are going to process and analyze NGS data to determine the mitochondrial haplogroups of individuals buried within the leprosarium.

## Part 1: Raw sequencing data and anatomy of a `.fastq` file

DNA libraries are mixed together during sequencing. Sequencing centers generally send you **demultiplexed** `.fastq` files. Demultiplexing is the process of separating the sequences into their own unique sample files based on the unique index sequence combinations you added to the libraries during indexing (Fig. 1). 

### Demultiplexing in NGS

<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/images/FIg1demultiplex.png" alt="SE sequencing" width="1000">

*Figure 1: An overview of Next Generation Sequencing steps (NGS).*

- **Single-end (SE) sequencing:** The DNA fragment is sequenced from only the forward direction, generating one read (Fig. 2).  
- **Paired-end (PE) sequencing:** The DNA fragment is sequenced from both directions, resulting in two reads per fragment (Fig. 3).  

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

### Tasks:
- What is the header for the forward read `@SRR15512701.3`?

- What is the ID of the corresponding reverse read?

It is critical that read pairs are kept in the same order in their respective files. If either a forward or reverse read mate is missing, the .fastq file is corrupt, and most tools will not be able to process it.

- `UF703_tail_R1.fastq` and `UF703_tail_R2.fastq` are the last reads of their respective `.fastq` files. How many reads were demultiplexed to library UF703?
  
- Do `UF703_head_R1.fastq` and `UF703_head_R2.fastq` have the same number of reads?

Fastq format encodes base quality scores in American Standard Code for Information Interchange (ASCII) format. This means the probability that a base was called in error is represented by a single character. This is important because every sequenced base corresponds to one quality score, so there are the same number of characters in the base call and quality score lines. A Q value ≥ 30 (Fig. 4) is generally accepted as a low enough probability that a base was called in error. 

A good rule of thumb is if your quality scores look like they’re cursing you (!@*$?@?’./$/$@/=+&#!!!), they are because your data are so bad.

<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/images/Fig.ascii.png" alt="PE sequencing" width="1200">

*Figure 4: Base qualties in ASCII format. Q is Q score, is an integer value representing P_error, the estimated probability of an error. The ASCII column has the ASCII single character code and it's numeric value (you can ignore that numeric value).*


- What is the sequence for the reverse read @SRR15512705.3698378?
  
- Is it a good quality read? Why or why not? (Hint: the ASCII character codes are reported in the fastq file per base. A Q value ≥ 30 is the cutoff for an acceptable probability of error)

- Identify a high quality read. What is the read’s ID? Is it a forward or reverse read?


## Part 2: Quality control of sequencing data using fastqc

Since you can't check every read manually, we use the tool [**Fastqc**](https://github.com/s-andrews/FastQC) to assess `.fastq` files. It reports 10 quality metrics — some more useful to us than others. 

Open [Sant_Llàtzer_mapping_report](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/Sant_Ll%C3%A0tzer_mapping_report.tsv) in a new tab (right click). Since you cannot edit this file directly, download it using the download icon in the top right corner. The extension is `.tsv`, which stands for "tab-separated values." Open this file using excel to be able to edit the spreadsheet.  

Open the **Fastqc reports** to fill in missing cells in the **Total reads or read pairs** column. Remember your data are PE sequenced.

| File Name                      | Link                                                                                             |
|---------------------------------|--------------------------------------------------------------------------------------------------|
| UF104_R1         | [UF104_R1_fastqc.html](https://kelzor.github.io/Introductory-ancient-DNA-practical/aDNA%20Data%20Analysis%20I/2.fastqc_outputs/UF104_R1_fastqc.html) |
| UF104_R2           | [UF104_R2_fastqc.html](https://kelzor.github.io/Introductory-ancient-DNA-practical/aDNA%20Data%20Analysis%20I/2.fastqc_outputs/UF104_R2_fastqc.html) |
| UF703_R1          | [UF703_R1_fastqc.html](https://kelzor.github.io/Introductory-ancient-DNA-practical/aDNA%20Data%20Analysis%20I/2.fastqc_outputs/UF703_R1_fastqc.html) |
| UF703_R2          | [UF703_R2_fastqc.html](https://kelzor.github.io/Introductory-ancient-DNA-practical/aDNA%20Data%20Analysis%20I/2.fastqc_outputs/UF703_R2_fastqc.html) |
| UF801_R1           | [UF801_R1_fastqc.html](https://kelzor.github.io/Introductory-ancient-DNA-practical/aDNA%20Data%20Analysis%20I/2.fastqc_outputs/UF801_R1_fastqc.html) |
| UF801_R2          | [UF801_R2_fastqc.html](https://kelzor.github.io/Introductory-ancient-DNA-practical/aDNA%20Data%20Analysis%20I/2.fastqc_outputs/UF801_R2_fastqc.html) |

### Tasks and questions
- Compare **forward (R1)** and **reverse (R2)** read reports for **UF104**.

- Are base quality distributions different?

- Pick a metric marked with a red X and explain what it shows about the data.



Look at **Overrepresented sequences** and **Adapter content** sections for sample UF104. Looking at the figure below (Fig. 5), answer the following question. 

<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/images/adapterreadthrough.png" alt="adapterreadthrough" width="1000">

*Figure 5: Adapter readthrough.*

- Why are **adapter sequences** overrepresented in **UF104**'s sequencing data? Remember that ancient DNA fragments are short and that sequencing occurs over a set number of base calling cycles.

As you can imagine, artificial adapter sequences attached to biologically real sequences affect how the sequences are aligned to a reference genome. We need to get rid of them before proceeding with analysis. We would also like to get rid of bases with low quality scores, because they can erroneously affect results. 

Another consideration with PE sequenced aDNA is that there is usually substantial overlap in the forward and reverse reads. The best way to deal with this is to merge the read pairs into one sequence. The reads have been **trimmed** to remove adapater readthrough and overlapping reads have been **merged** using [**Adapterremoval2**](https://adapterremoval.readthedocs.io/en/2.3.x/). Here are Fastqc reports generated from the trimmed and merged reads.


| Sample                   | Link                                                                                             |
|---------------------------------|--------------------------------------------------------------------------------------------------|
| UF104 | [UF104_1.fastq.pG.fq_L1.pe.combined_fastqc.html](https://kelzor.github.io/Introductory-ancient-DNA-practical/aDNA%20Data%20Analysis%20I/2.fastqc_outputs/Trimmed_and_merged_fastqc_reports/UF104_1.fastq.pG.fq_L1.pe.combined_fastqc.html) |
| UF703 | [UF703_1.fastq.pG.fq_L1.pe.combined_fastqc.html](https://kelzor.github.io/Introductory-ancient-DNA-practical/aDNA%20Data%20Analysis%20I/2.fastqc_outputs/Trimmed_and_merged_fastqc_reports/UF703_1.fastq.pG.fq_L1.pe.combined_fastqc.html) |
| UF801 | [UF801_1.fastq.pG.fq_L1.pe.combined_fastqc.html](https://kelzor.github.io/Introductory-ancient-DNA-practical/aDNA%20Data%20Analysis%20I/2.fastqc_outputs/Trimmed_and_merged_fastqc_reports/UF801_1.fastq.pG.fq_L1.pe.combined_fastqc.html) |

- Why is there only one sample per Fastqc report now?

- How do the quality metrics of the trimmed and merged reads compare to the fastqc reports of the unmodified .fastq files?

- Why is per base and sequence (GC) content flagged for all quality filtered samples?

- Why is the sequence length distribution flagged for all quality filtered samples?

- Using the trimmed and merged **Fastqc reports**, fill in the `Total retained reads or read pairs after trimming (and merging for ancient)` and `proportion kept after trimming (and merging)` columns in the mapping report.

- The proportion of reads kept after trimming and merging is an important metric to consider when assessing sequencing read and run quality. Think of some reasons why.


## Part 3: Alignment of quality filtered sequencing reads

After quality control, the next step is to align reads (`.fastq` files) to a reference genome, because the reads alone have no information about organism of origin. This is (called **mapping**) It is a critical step in your data analysis pipeline, and depending on how strictly or loosely you perform alignments, you can get false negative or false positive results. Below is a diagram of the mapping process (Fig.6). It is common in aDNA to use [**BWA**](https://bio-bwa.sourceforge.net/) to align reads.

<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/images/Fig4.mapping.png" alt="mapping" width="900">

_Figure 6. Depiction of mapping reads to a reference genome_


The reference genome is in a one-dimensional file format called `fasta`. It is “one-dimensional” because it does not hold any higher-level information about quality, heterozygosity, read support, etc. A reference genome is a consensus sequence. It can be one long sequence or broken up into sections called contigs. These samples have been mapped to the revised Cambridge reference sequence, the primary mitochondrial reference genome.

Open [`rCRS.fasta`](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/mtDNA_reference_genome/rCRS.fa) 

- What does fasta format look like? How would you describe it?
- What is this reference genome sequence of?


The outputs of mapping are `.bam` and `.bai` files. A `.bam` file contains the mapping coordinates and quality scores for each read to the reference genome, and it has a corresponding index filed called a `.bai` that acts like its table of contents. A `.bam` file can be converted into a `.sam` for manual inspection. They convey the same information, but a `.bam` is in binary format to save space (Fig 7).

- `.sam` Human-readable file with huge file size  
- `.bam` Compressed file size but gibberish



<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/images/bam.png" alt="m=bam" width="900">

_Figure 7. Depiction of .bam compressed file gibberish_


`.sam/.bam` files have a mandatory header section. The header usually contains the sample name, the name of the reference genome, and the tool and commands used to generate the alignments. 
Each row corresponds to a unique sequencing read, given the ID `QNAME`. There are 11 mandatory fields, including the `QNAME` and many optional fields. Below is a diagram of a `.sam` file (Fig. 8).

<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/images/samformat.png" alt="m=bam" width="900">

_Figure 8. Information about the mapping and mapping quality of reads in SAM format_

Important fields in `.sam/.bam` include:
- **QNAME:** Sequence read ID.  
- **FLAG:** Bit-coded info about mapping status, whether it mapped but its pair did not (not applicable with merged reads), and whether it was mapped to more than one place in the reference.  
- **MAPQ:** Mapping quality score summarizing how well the read mapped.  
- **CIGAR:** A coded explanation of alignment used to calculate mapping quality (e.g., `3M7N4M` = 3 mapped bases, 7 bp gap, 4 mapped bases (Fig. 9)).  

<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/images/3M7N4M.png" alt="m=bam" width="500">

_Figure 9. Visualisation of a 3M7N4M CIGAR score_

The most simple CIGAR strings represent the highest quality alignments. For example, 68M means all 68 bases mapped.

I have provided you with filtered `.sam` files. Using the `-f 4` FLAG, I filtered out all of the unmapped reads. 
Using `-q 25` MAPQ, I filtered out all the reads with a mapping quality score < 25. 
I have also run a tool that removes PCR and optical duplicated reads, which do not add information. Why do duplicates not add information?

Open [UF703_f4_q25_dedup.sam](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/3.sam_files/UF703_f4_q25_sortc_markdup.sam)


- Do you recognize the read IDs from the .fastq files?

- Find read `M_SRR15512701.615723`. What is the mapping quality? What is the CIGAR score?

- Overall, do these CIGAR strings look like high quality alignments? Why?
  

By now you’re aware that aDNA has characteristic damage that results from miscoding lesions. These miscoding lesions are important for authenticating aDNA, but also they can seem like SNPs/SNVs in the sequenced reads. One way to deal with this is by using a tool called [**MapDamage**](https://ginolhac.github.io/mapDamage/) to rescale the base quality scores for C -> T and G -> A transitions. Rescaled bases have low quality scores so that they will not be called as variants by a variant caller. **MapDamage** takes a `.bam` as input and outputs a rescaled `.bam` file.  

Now open [UF703_f4_q25_dedup.rescaled.sam](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/3.sam_files/UF703_f4_q25_sortc_markdup.rescaled.sam) and compare with [UF703_f4_q25_dedup.sam](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/3.sam_files/UF703_f4_q25_sortc_markdup.sam)

- Look at line 20 of the rescaled `.sam`. Have any of the base quality scores for that read been rescaled?

- Look at line 237 of the rescaled `.sam`. Have any of the base quality scores for that read been rescaled?


## Part 4: Using `qualimap` to determine reference coverage and depth

[**Qualimap**](http://qualimap.conesalab.org/) is a tool that assesses genome coverage and depth from `.bam` files. I have run the tool for you and generated outputs.

| Sample |  Qualimap `.html` report | Qualimap `.txt` report |
|--------|-------------|-------------|
| UF104 | [UF104.html](https://kelzor.github.io/Introductory-ancient-DNA-practical/aDNA%20Data%20Analysis%20I/4.qualimap_outputs/UF104/UF104_report.html) | [UF104.txt](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/4.qualimap_outputs/UF104.txt) |
| UF703 | [UF703.html](https://kelzor.github.io/Introductory-ancient-DNA-practical/aDNA%20Data%20Analysis%20I/4.qualimap_outputs/UF703/qualimapReport.html)  | [UF703.txt](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/4.qualimap_outputs/UF703.txt) |
| UF801 | [UF801.html](https://kelzor.github.io/Introductory-ancient-DNA-practical/aDNA%20Data%20Analysis%20I/4.qualimap_outputs/UF801/qualimapReport.html)  | [UF801.txt](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/4.qualimap_outputs/UF801.txt) |

Open the `.pdf` and `.txt` reports for your samples.  

Fill in the following columns in the mapping report:

`Q25 mapped reads after duplicate removal` 

`Avg length of mapped Q25 reads`  

`Mean Cov` 

`SD Cov`  

### Tasks:

- Choose one of the figures from the Qualimap .pdf report. Explain what this figure/calculation is conveying about your mapped reads. If you are not sure how to interpret the figure, use the [Qualimap manual](http://qualimap.conesalab.org/doc_html/index.html). This is a very dense manual, and a lot of it is not relevant. Looking up obscure details about tool reporting (or asking ChatGPT - how do you think I learned how to format this practical) is 50% of bioinformatics, so it's worth practicing! But feel free to ask me for help.


- Now that you have read through the Qualimap `.pdf` reports, open the `.txt` files. Much but not all of the information from the `.pdf` output is reported here as well. `.txt. file outputs aer useful because you can write code that will loop through them them and pull out the information you need. Most geneticists automate their mapping reports instead of filling it in manually like you're doing. But that's how we learn!!!!

Fill in the next twp columns in the mapping report using the `.txt` files:

`% ref covered at >=1x`

`% ref covered at >=5x`  
  
Some of the metrics you need to assess your data are not generated by a tool, so you need to generate them yourself. **Cluster factor**, also known as **library complexity**, is informative about the number of unique fragments in your library. It tells you on average how many times a read was sequenced. 

For example, cluster values over 2 indicate that on average, every fragment has been sequenced twice, so the library is probably exhausted of DNA from this organism. Depending on how deeply you sequence and if you’ve done targeted enrichment, cluster factors can be huge, anywhere from 20 to 30.

- Calculate `cluster factor` (library complexity) for each sample using the formula in the mapping report header and the values in the columns you’ve already filled. 

Endogenous DNA frequency can be used in tandem with cluster factor to decide whether to sequence your library more deeply or determine how efficient your capture enrichment was.

- Calculate `endogenous DNA frequency` (of mtDNA in this case) for each sample using the formula provided in the mapping report header and the values in the columns you’ve already filled out.

## Part 5: MapDamage plots

There are several tools that produce iconic aDNA “damage plots,” but here we are using the tool [**MapDamage**](https://ginolhac.github.io/mapDamage/) because it also rescales base qualities, which other damage profiling tools do not.  

Open the fragment misincorporation plots for each sample. The four plots at the top indicate base (A, C, T, and G) frequencies within the read (within the grey square) and outside of the read. What are these plots illustrating? *Hint: depurination.*  
The bottom two plots are the most common damage plots that you may have seen before. The red line charts **C to T** substitutions, and the blue line charts **G to A** substitutions.

MapDamage Plot       |
----------------------|
[UF104 mapdamage plot](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/5.MapDamage_outputs/UF104_Fragmisincorporation_plot.pdf)            |
[UF703 mapdamage plot](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/5.MapDamage_outputs/UF703_Fragmisincorporation_plot.pdf)              |
[UF801 mapdamage plot](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/5.MapDamage_outputs/UF801_Fragmisincorporation_plot.pdf)               |

### Tasks:

- From the bottom two plots, estimate the values for the last four columns in your mtDNA mapping report.

- Based on these plots, do all of the samples have authentic aDNA?


## Part 6: Inspecting variants manually

Finally, let’s examine genetic variation!  

**Variant calling** is the process of identifying genetic variants in your sample based on the sequence alignments. Point mutation **SNPs/SNVs** are the most common variants in aDNA. This is because it can be hard to confidently identify structural variants due to incomplete genome recovery.

There are many variant callers (*freebayes*, *GATK Haplotypecaller*, *BCFtools*, *VarScan*, and *GATK UnifiedGenotyper*, just to name a few). Some of them are better for working with aDNA.  

Variant calling produces variant call format (`.vcf`) files. Like the other files we have looked at today, `.vcf` has a standardized format starting with a header with general information about the version, variant calling tool, and a list of abbreviations used in variant reporting and what they mean. Each row corresponds to a variant, and there are several columns of mandatory information for each variant (Fig. 10):


<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/images/vcf.png" alt="m=vcf" width="1000">

_Figure 10. VCF header format_

Instead of using the `.vcf` files to identify variants in our samples, we are going to manually investigate the `.bam` files in **Integrated Genome Browser (IGV)**. Our genetic data are low coverage, so we need to manually inspect SNPs. We will also use this as an opportunity to see how different decontamination methods modified our `.bam` files.

- Download the [rCRS.fasta reference genome](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/mtDNA_reference_genome/rCRS.fa) and its [index file](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/mtDNA_reference_genome/rCRS.fa.fai) to your computer.

- Download [all of the bam files in a `.zip`](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/6.PMDtools_mapdamage_decontaminated_bams/all-bams.zip) to your computer.

- Open [**IGV**](https://igv.org/doc/desktop/), which is available in AppsAnywhere, and load the reference genome (`rCRS.fa`) clicking **“Genomes”** -> **“Load genome from file”** and select the file `rCRS.fa`. The reference genome is now loaded.

- Now go to **“File”** -> **“Load from file”** -> select _all three bams from the same sample_ (`.bam`, `.rescale.bam`, and `.pmds3filter.bam`). You should see three tracks in IGV, one from each `.bam`. It should look like the below image (Fig. 11).

- Load the `.bam`, `.rescale.bam`, and `.pmds3filter.bam` files for one sample at a time

<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/images/IGV.png" alt="m=vcf" width="12000">

_Figure 11. IGV example with three loaded tracks_

The hypervariable regions of mtDNA are useful for analysing lineage diversity, because they are the most polymorphic regions of the mitochondrial genome. There are two hypervariable regions in the mtDNA genome: 

| Region | Positions        |
|--------|------------------|
| HVRI   | 16024–16383      |
| HVRII  | 57–372           |

For each sample, manually scan the hypervariable regions using the arrows at the top. It will be easier for you to zoom in all the way. 

### Tasks:

- Search the hypervariable regions for evidence of true variants, and mark the ones you identify in your notes in a table similar to the one below.

- The way to mark polymorphisms is **reference base + position # + variant base**. For example, `A374C` would be an A -> C transversion at position 347.

- While you are scanning the sites, compare the differences in the three `.bam` files. Remember, `.bam` hasn’t been filtered for contamination, `.rescaled.bam` has had its bases rescaled to low qualities, and `.pmds3filter.bam` has been filtered to remove reads that are likely contaminants.

<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/images/varianttable.png" alt="m=vcf" width="700">

- Check the variants you identify against known mtDNA polymorphisms at [phylotree.org](https://www.phylotree.org/tree/)

- Can you assign a haplogroup to any individual?  



---
