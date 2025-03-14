# Introductory ancient DNA (aDNA) practical
Ancient DNA and Next Generation Sequencing (NGS) practical designed for archaeology students (at the undergraduate and graduate level) with no prior experience with genomics or bioinformatics. 

This exercise uses publicly available data published by [Pfrengle et al., 2021](https://bmcbiol.biomedcentral.com/articles/10.1186/s12915-021-01120-2).

This exercise requires no coding or coding experience, as all relevant files have been generated for you.

Check this [cheat sheet](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA_NGS_file_format_cheat_sheet.pdf) out to test your understanding and see a flowchart of the aDNA processing workflow.


## [aDNA Data Analysis I: recovering mitochondrial genomes from NGS shotgun data](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20I/aDNA_Data_Analysis_I.md)

__Learning objectives__: in this lesson you will determine if shotgun sequencing data contain sufficient reads to assign human mitochrondrial haplogroups. 

You will gain familiarity with the following:

+ Next Generation Sequencing (NGS) file formats
  - FASTQ: Contains raw sequence reads and quality scores.
  - SAM/BAM: Binary alignment formats for storing mapped reads.
  - VCF: Variant Call Format for storing genetic variation data.
+ Quality control metrics used to assess aDNA quantity and quality and the tools used to generate them
  - fastqc
  - qualimap
  - MapDamage
  - PMDtools
+ Necessary considerations when identifying variants in low-coverage, damaged DNA sequencing reads

## [aDNA Data Analysis II: assessment and evolutionary analysis of ancient _Mycobacterium leprae_ genomes](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/aDNA_Data_Analysis_II.md)

__Learning objectives__: in this lesson you will reinforce what you learned in Part I by assessing the quality of reads mapped to the _M. leprae_ reference genome. Additionally, you will learn how to make phylogenetic trees and draw evolutionary inferences from them.

You will 

+ Review all content from Analysis Part I
+ Make phylogenetic trees
  - MEGA11
+ Interpret phylogenetic trees

## Isotopic and Biomolecular Archaeology assignment

You will repeat the exercises you did above on data from 14 additional individuals.

To download the [assignment data](https://github.com/Kelzor/Introductory-ancient-DNA-practical/tree/main/Assignment%20data) from github, go to this [website](https://download-directory.github.io/) and paste the URL of the [mtDNA_files](https://github.com/Kelzor/Introductory-ancient-DNA-practical/tree/main/Assignment%20data/mtDNA_files) and [Mleprae_files](https://github.com/Kelzor/Introductory-ancient-DNA-practical/tree/main/Assignment%20data/Mleprae_files). This will create a zip file for you to download, so you don't have to download all the files individually.

## Ancient DNA specific learning resource and command line introduction:

- [Introduction to ancient metagenomics](https://www.spaam-community.org/intro-to-ancient-metagenomics-book/)

## General NGS learning resources (not aDNA specific)

### Bioinformatics Intros and Trainings

- [Babraham Bioinformatics Training](https://www.bioinformatics.babraham.ac.uk/training.html)  
- [Sequencing Quality Control](https://www.bioinformatics.babraham.ac.uk/training/Sequence_QC_Course/Sequencing%20Quality%20Control.pdf)  

###  Overview of NGS Data Processing

- [NGS Data Processing - DU-Bii](https://du-bii.github.io/module-5-Methodes-Outils/seance1_NGS/slides.html#1)  

###  Fastq Format

- [FASTQ Files Explained - Illumina](https://emea.support.illumina.com/bulletins/2016/04/fastq-files-explained.html)  
- [FASTQ Files - BaseSpace](https://help.basespace.illumina.com/articles/descriptive/fastq-files/)  

###  SAM/BAM Format

- [CIGAR Strings Explanation](https://www.drive5.com/usearch/manual/cigar.html)  
- [SAM Files Explanation](https://www.drive5.com/usearch/manual/sam_files.html)  
- [SAM Format Specification (v1)](https://samtools.github.io/hts-specs/SAMv1.pdf)  
- [Basic SAM/BAM I/O - SeqAn](https://seqan.readthedocs.io/en/seqan-v1.4.2/Tutorial/BasicSamBamIO.html)  
- [Understanding MAPQ Scores in SAM Files](http://www.acgt.me/blog/2014/12/16/understanding-mapq-scores-in-sam-files-does-37-42#:~:text=So%20if%20you%20happened%20to,score%20would%20increase%20to%2030)  

###  Filtering SAMs/BAMs

- [Read Duplicates and Patterned Flow Cells](http://core-genomics.blogspot.com/2016/05/increased-read-duplication-on-patterned.html)  
