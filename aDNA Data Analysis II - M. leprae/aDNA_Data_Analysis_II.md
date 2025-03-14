# aDNA Data Analysis II: assessment and evolutionary analysis of ancient Mycobacterium leprae genomes

## Scenario

You have received NGS *Mycobacterium leprae* capture data for three skeletons from Sant Llàtzer, a medieval leprosarium in Barcelona: **UF104**, **UF703**, **UF801**. 

The genetic diversity of *M. leprae* strains across medieval Europe and within leprosaria was high. You would like to know if leprosy infections in the medieval leprosarium of Sant Llátzer in Barcelona were caused by a high genetic diversity of lineages as well.

## Part 1: Using Qualimap to determin reference coverage and depth

Since you have already assessed these raw data (*aDNA Data Analysis I*), you will begin this practical by using **Qualimap** to assess genome coverage and depth from `.bam` files that were generated by mapping reads to the *Mycobacterium leprae* reference genome. 

I have run the tool for you, and you have the outputs: a `.pdf` report and a `.txt` file for each sample.

Open [Sant_Llàtzer_M.leprae_mapping_report](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/M.leprae%20mapping%20report.tsv) in a new tab (right click). Since you cannot edit this file directly, download it using the download icon in the top right corner. The extension is `.tsv`, which stands for "tab-separated values." Open this file using excel to be able to edit the spreadsheet.

[**Qualimap**](http://qualimap.conesalab.org/) is a tool that assesses genome coverage and depth from `.bam` files. I have run the tool for you and generated outputs.

|  Qualimap `.pdf` report | Qualimap `.txt` report |
|-------------|-------------|
| [UF104.pdf](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/1.Qualimap%20outputs/UF104_report.pdf) | [UF104.txt](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/1.Qualimap%20outputs/UF104_genome_results.txt) |
|  [UF703.pdf](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/1.Qualimap%20outputs/UF703_report.pdf)  | [UF703.txt](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/1.Qualimap%20outputs/UF703_genome_results.txt) |
| [UF801.pdf](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/1.Qualimap%20outputs/UF801_report.pdf)  | [UF801.txt](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/1.Qualimap%20outputs/UF801_genome_results.txt) |

Open the `.pdf` reports for your samples.

Fill in the following columns in the mapping report:

- `Q25 mapped reads after duplicate removal` 
- `Avg length of mapped Q25 reads`
- `Mean Cov`
- `SD Cov`  

Now that you have read through the `.pdf` reports, open the **Qualimap** `.txt` files. Much — but not all — of the information from the `.pdf` is reported here as well. 

Text file outputs are useful because you can write code that will loop through `.txt` files and pull out the information you need. Most geneticists automate their mapping reports so that they do not have to manually extract each piece of information.  

Complete the next two columns in your **Sant Llátzer mapping report** using the **Qualimap** `.txt` files.:

- `% ref covered at >=1x`  
- `% ref covered at >=5x`  

Some of the metrics you need to assess your data are not generated by a tool, so you need to generate them yourself. **Cluster factor**, also known as **library complexity**, is informative about the number of unique fragments in your library. It tells you on average how many times a read was sequenced. **Library complexity** refers to how many unique DNA fragments are in your sequencing data. One way to measure this is by looking at the number or percentage of **duplicate reads**, which are reads that start and end at the same positions when aligned to a reference genome. They occur during the multiple rounds of PCR necessary to prepare a library for sequencing.

For example:
- **Cluster values over 2** indicate that, on average, every fragment has been sequenced twice — meaning the library is likely exhausted of DNA from this organism.
- Depending on how deeply you sequence and whether you’ve done targeted enrichment, **cluster factors can be huge**, ranging anywhere between **20 and 30**.

In your mapping report, **Calculate cluster factor** for each sample using the formula in the column header and the values in the columns you’ve already filled out.

**Endogenous DNA Frequency**

Endogenous DNA frequency can be used alongside cluster factor to decide whether to sequence your library more deeply or to assess how efficient your capture enrichment was. Since these *M. leprae* data were obtained through targeted capture enrichment, endogenous DNA frequency will show how effective the capture experiment was.

**Calculate the frequency of endogenous DNA** (*M. leprae* in this case) for each sample using the formula in the column header cell and the values in the columns you’ve already filled out.

## Part 2: MapDamage plots

Review: 

Remember that **deamination** results in the chemical modification (loss of an amine group) of cytosine such that it becomes uracil (Fig. 1). This is probematic because uracils are complementary to adenine, not guanine like the original cytosine base. Therefore, in subsequent PCRs, the original cytosine becomes a thymine in the forward direction. In the reverse direction of the fragment, the guanine becomes an adenine base.

<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/images/depurination.png" alt="deamination" width="1000">

*Figure 1: Depiction of depurination and deamination (from [Orlando et al., 2021](https://www.nature.com/articles/s43586-020-00011-0)).*

Uracils can be removed by treating the DNA extracts with USER enzyme as depicted below (Fig. 2). USER enzyme is a combination of two enzymes, principally uracil DNA glycosylase (UDG). 

<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/images/USER_enzyme.png" alt="USER" width="700">

*Figure 2: Depiction of USER enzyme treatment (from [NEB](https://www.neb.com/en-gb/products/m5505-user-enzyme)).*

You may remember, however, that deamination can be useful for authenticating DNA as ancient, so we may not want to remove all uracils. [Rohland and colleauges (2015)](https://royalsocietypublishing.org/doi/10.1098/rstb.2013.0624) developed a method termed "partial UDG treatment" that excises uracils except those in the terminal positions of the fragments (Fig. 3). This is useful becuase it perserves the damage signature but isolates it to the terminal ~2 bases, whereas normally it is distributed deeper into the fragment. 

<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/images/effect-of-UDG-treatment.png" alt="partialUDG" width="700">

*Figure 3: Depiction of full versus partial USER (UDG) enzyme treatment. **A** Full UDG treatment depiction and **B** resulting damange plot. **C** Partial UDG treatment and **D** resulting damage plot (from [Introduction to ancient metagenomics resource Fig 2.20](https://www.spaam-community.org/intro-to-ancient-metagenomics-book/introduction-to-ancient-dna.html#how-dna-degrades)).*


There are several tools for generating aDNA "damage plots," but here we are using [**MapDamage**](https://ginolhac.github.io/mapDamage/) because it also **rescales base qualities**, which other damage profiling tools do not. 

**Open the MapDamage outputs**

|  MapDamage `.pdf` reports |
|-------|
| [UF104-MapDamage.pdf](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/2.MapDamage%20outputs/UF104_Fragmisincorporation_plot.pdf) |
| [UF703-MapDamage.pdf](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/2.MapDamage%20outputs/UF703_Fragmisincorporation_plot.pdf) |
| [UF801-MapDamage.pdf](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/2.MapDamage%20outputs/UF801_Fragmisincorporation_plot.pdf) |

Look at the **fragment misincorporation plots** for each sample:

   - The **four plots at the top** show base frequencies (A, C, T, and G) within the read (inside the grey square) and outside the read.
   - These plots illustrate patterns of **depurination** — a type of aDNA damage where purines (A and G) are lost over time (Fig. 1).

The **two plots at the bottom** are the most recognizable damage plots cause by **deamination** (Fig. 1):
   
   - The **red line** charts **C to T substitutions**.
   - The **blue line** charts **G to A substitutions**.


### Tasks:

- **Estimate the values** for the last four columns in your ***M. leprae* mapping report** using the bottom two plots.
 
- **Assess authenticity**: Based on these plots, do all the samples show patterns consistent with **authentic ancient DNA**?

- What are the multiple ways you could interpret no sign of deamination damage in the MapDamage plot?


## Part 3: Basics of phylogenetic inference

As we prepare to build **phylogenies** to identify which *M. leprae* lineages the Sant Llàtzer strains belong to, it’s important to understand how phylogenetic trees are constructed and how they depict evolutionary relationships among organisms.

The **European Molecular Biology Laboratory (EMBL)** provides beginner-friendly training on a range of bioinformatics topics, including phylogenetics.

### Tasks:

Visit the [EMBL Introduction to Phylogenetics course](https://www.ebi.ac.uk/training/online/courses/introduction-to-phylogenetics/what-is-phylogenetics/)

**Complete the following sections**:
- What is phylogenetics?
- Why is phylogenetics important?
- What is a phylogeny?
- Major stages in phylogenetic analyses
- Summary

**Test your understanding** by completing the short quiz at the end.

This foundation will help you better interpret the phylogenetic trees we generate in the following section.

<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/images/satges-of-phylogenetic-analysis.png" alt="phyloanalysis" width="700">

*Figure 4: Stages of phylogenetic analysis [EMBL's stages in phylogenetic analysis](https://www.ebi.ac.uk/training/online/courses/introduction-to-phylogenetics/major-stages-in-phylogenetic-analyses/).*


This is how our research maps on to EMBL's outline of phylogenetic analysis:


| **Stage** | **Connect to our data** |
|-----------|-------------------------|
|1. Question |Was *M. leprae* genetic diversity at Sant Llàtzer high? |
|2. Appropriate model and parameters | Our inference models: <br> **Maximum parsimony (MP):** character-based method that prioritizes the phylogeny with the fewest state transformation (substitutions) <br> **Maximum likelihood (ML):** generalized time reversible (GTR) model of nucleotide substitution; most general model that allows all transitions and transversions to occur with independent frequencies |
|3. Data collection | NGS sequencing data from Sant Llàtzer residents |
|4. Which orthologous (comparable) sequences? |  Since we are working with one organism, this is simple. *M. leprae* has a relatively short genome (~3.3 million bp), is haploid (only one copy of the genome that does not recombine), and exhibits little horizontal gene transfer (trading of genomic elements with other bacteria), we are using variant sites across the entire *M. leprae* genome. |
|5. Sequence alignment | Build **Multiple-sequence SNP alignments** (explained in Part 4) |
|6. Estimate tree given the data and model | Build MP and ML trees in MEGA 11 |
|7. Estimate the error | What is the bootstrap support? Do the tree topologies match? |
|8. Have we answered our question?   |  To be determined! |
|9. New biological insight   | **The delicious goal of science!!!** |


## Part 4: Building phylogenies using multiple sequence alignments


The file type we will use to build phylogenies is called `.fasta`. We discussed this file type during aDNA Data Analysis I. It is a one-dimensional file that only contains the nucleotide bases assigned to a sample. A `.fasta` file can contain sequences from many different organisms/samples, but each new sequence must be signified with a header that begins with `>`. The `.fasta` we will be examining includes alignments from many different samples.

Our `.fasta` contains sequences from samples aligned to the _M. leprae_ reference genome, which is why it is called a **multiple sequence alignment (MSA)**. Each base in the MSA represents a position in the sequence that is aligned across all sequences. To help visualize the information contained in our MSA, I have organized the data in Table 1. You can see that each sample has the same amount of bases called and that they correspond to the same reference genome coordinate based on the position in which they occur. For example, `516_Samoa_2014` deviated from `2188-2007_Brazil_2016` and `ARLP-23_Ethiopia_2015` in the second base, where `516_Samoa_2014` has an adenine and the other two samples have a guanine. 

*Table 1: Simple demonstration of the information contained in a MSA. Some bases are marked in bold that differentiate the samples.*
| Sample | Sequence |
|-----------------|------------------------------------------------------------------------------------|
|516_Samoa_2014 | G**A**G**N**GGCGCTCCC**A**GCC**CN**T**T**CTGGGGGGGCAGCCNGAGGGTGAGGGGGGGGGGCGGTGC |
|2188-2007_Brazil_2016 | GGGGGGCGCTCCCNGCCTCTCCCGAGAGGGCAGCCTGGGGGTGGGGGGAGAGGGCGGTGC |
|ARLP-23_Ethiopia_2015 | GGGGGGCGCTCCCGGCCTCTCTTGGGGGGGCAGCCTGGGGGTGGGAGGGGGGAGCGGTGC |

### Tasks:

Open the [M.leprae_SantLlatzer_MSA.fasta](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/4.Multi-sequence%20SNP%20alignments/M.leprae_SantLlatzer_MSA.fasta) in a new window.

- How many *M. leprae* samples are represented in this file? (hint: how many `>` are there?)

Our multiple sequence alignment (MSA) contains aligned positions for all the **variant sites** among the samples. Since samples will not share all the same SNVs/SNPs, many of the base calls in our `MSA.fasta` are reference calls.

- How many SNV/SNPs differentiate these samples? (Hint: Pick a sample and use your cursor to highlight all the base pairs for that sample.)

Let’s visualize the same SNV/SNP across three different data types in increasing amount of information the files contain:

1. The multiple sequence alignment: ['M.leprae_SantLlatzer_MSA.fasta'](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/4.Multi-sequence%20SNP%20alignments/M.leprae_SantLlatzer_MSA.fasta)
2. [A SNP table](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/4.Multi-sequence%20SNP%20alignments/M.leprae_SNV_SNP_table.tsv) that contains the reference genome coordinates of the called variants and which base is the reference or variant [allele](https://www.genome.gov/genetics-glossary/Allele)
3. The [`UF801_leprae.bam`](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/4.Multi-sequence%20SNP%20alignments/UF801_leprae.bam), [`UF801_leprae.bai`](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/4.Multi-sequence%20SNP%20alignments/UF801_leprae.bai), and the reference genome [`M_leprae_TN.fasta`](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/M.%20leprae%20reference%20genome/M_leprae_TN.fasta) and corresponding index file [`.fai`](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/M.%20leprae%20reference%20genome/M_leprae_TN.fasta.fai) which you will need to download and view in IGV. Make sure to open the *M. leprae* reference genome: `Genomes` -> `Load Genome from File` -> `M_leprae_TN.fasta`

The SNP table is a table of every variant position called among the samples. Using the SNP table, answer the following questions.

- What is the genomic position of the first variant?
  
- What is the reference allele at this position?
 
- What base does sample UF801 have at this position?

Let’s view this same variant in the `M.leprae_SantLlatzer_MSA.fasta`

- What is the first base in the SNP alignment of sample UF801?
  
- Is it what you expect?

Finally, look at the data for this variant before it was called and “flattened” into a consensus format in the `.fasta` file. Look at `UF801_leprae.bam` in IGV, and zoom all the way in using the `+` button in the top right corner.

- What are you visualizing in IGV?

- What variant is identifiable at base 73?
  
- How many reads are covering base 73?
  
- Do you think there is strong evidence that this is a true variant?

Repeat the steps above for the second variant in the SNP table (the variant in row 3). A `.` indicates the sample has the reference allele at that position.


## Part 5: Building maximum parsimony trees in MEGA11 ***(newer PCs in Kiln lab have MEGA 11 locally installed and it runs well - the old computers do not have it locally installed and through apps anywhere it takes a long time. It also took a long time on someone's personal macbook. - revise parameters so that it is less computationally next year)**

[Molecular Evolutionary Genetics Analysis (MEGA)](https://www.megasoftware.net/) is a free-to-download tool for making phylogentic tree and inspecting sequence alignments, among many other things. The developers offer a [walkthrough](https://www.megasoftware.net/web_help_11/index.htm#t=Introduction.htm) and [tutorial videos](https://www.megasoftware.net/videos), of you are interested in learning more.

### Tasks:

First you will build a maximum parsimony tree of comparative samples I have chosen to represent the known worldwide genetic diversity of *M. leprae*.

Download [M.leprae_comparative.fasta](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/5.Maximum%20parsimony/M.leprae_comparative.fasta) to your computer.

1. Load the data into MEGA 11 by clicking on `Data` and `Open a File/Session` as shown below and loading the file `M.leprae_comparative.fasta`.

<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/images/1MEGA.png" alt="MEGA1" width="900">

2. You will have a series of prompts. Respond as follows:
 
`Analyze` -> `Nucleotide Sequences` -> `Ok` -> `No` (to protein coding questions)

3. Once the data are loaded, view the alignments by clicking on the `TA` square as highlighted below.

<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/images/2MEGA.png" alt="MEGA2" width="700">

4. Each of the 40 samples corresponds to a row, and each column is a position in the SNP alignment. Just like the SNPtable, `.` indicates the position in that sample has the reference base.

5. In the bottom left corner, you can see how many variant sites are included in this alignment `1,336`.

6. You can close the data window or leave it open.

7. Go back to the main window and select `Phylogeny` and `Construct/Test Maximum Parsimony Tree(s)` as shown below.

<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/images/3MEGA.png" alt="MEGA3" width="700">

8. Select `Yes` that you would like to use the currently active data.

9. You will see a lot of parameter options, but you only need to change one field. Change `Test of Phylogeny` to `Bootstrap method` as shown below and select `Ok`. The run should be pretty fast. You’ve (probably) made your first phylogenetic tree!

<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/images/4MEGA.png" alt="MEGA4" width="700">

Let’s investigate the phylogeny. Select `Layout` on the left side and adjust the `Tree Width` to make the branches and names easier to see. The samples are named in this construct: 

`ID` `Country/state of isolation` `Year isolated in CE` `host species if not human`

Above the tree select the `Bootstrap Tree` view. This will add bootstrap values to the tree. To make this tree, MEGA11 just iterated through 100 different replications to calculate parsimony (a value set in the screenshot above). A bootstrap value is the percentage of replicates that resulted in this organization. For example, _M. lepromatosis_ shared a node with `516`, `518`, `CM1`, `Kanazawa`, `Kyoto-1`, `S10`, and `Jorgen-507` in 100% of replicates (see blue dot below).

<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/images/5MEGA.png" alt="MEGA5" width="900">

Bootstraps are used to determine the probability of the final tree. Values of >90 offer very strong support.

The maximum parsimony algorithm, however, doesn’t account for everything we know about our data. We know the direction of evolution that the maximum parsimony analysis doesn’t account for. We need to manually identify the outgroup, _M. lepromatosis_, to root the tree so that we can see the evolutionary history of each lineage.

To root the tree, right-click around the tree and select `Root Tree`. Next, click on the branch leading to _M. lepromatosis_.

<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/images/6MEGA.png" alt="MEGA6" width="900">

This is the tree topology that we will add our samples `UF104`, `UF703`, and `UF801` to. You can minimize this tree window. Let’s add our samples and see what happens!

## Part 6: Building maximum likelihood trees in MEGA11: garbage in, garbage out

Now we will build maximum likelihood (ML) trees in MEGA11. ML is considered a more robust technique for phylogenetic analysis than maximum parsimony. The ML approach searches for the tree that has the highest probability of giving rise to the observed data.

A ML approach is better at accommodating branches (lineages) that evolve at different rates. This is relevant for pathogens, because their genomes can evolve relatively quickly depending on infectivity, transmissibility, and access to hosts. Further, using a ML approach, branch lengths are scaled by the number of substitutions, so you can visualize heterogeneity in evolutionary rates by comparing the branch lengths.

Build a ML tree using our samples. Download the [M.leprae_SantLlatzer_MSA.fasta](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/6.Maximum%20likelihood/M.leprae_SantLlatzer_MSA.fasta) to your computer.

Like before in MEGA11 to load our MSA:

`Data` -> `Open a File/Session` -> `M.leprae_SantLlatzer_MSA.fasta` -> `Yes` -> `No` -> `Analyze` -> `Ok` -> `No`

To make the maximum likelihood tree: 

`Phylogeny` -> `Construct/Test Maximum Likelihood Tree` -> `Yes use the active data`

<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/images/7MEGA.png" alt="MEGA7" width="700">

Set up your run to look like the following:

<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/images/8MEGA.png" alt="MEGA8" width="800">

This will take about 10-15 minutes to run. While you are waiting, you can look over a version of the MP tree you made that has branch labels added: [Labeled worldwide *M. leprae* phylogeny](https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/6.Maximum%20likelihood/Comparative_M.leprae_MP_tree_with_branch_labels.pdf)

- Which branches do the ancient samples belong to?

Once the ML run finishes, look at the resulting topology. 

- Does it look strange to you? In what way?

Root the tree, and see how that changes things. Right click on the whitespace -> `Root Tree` -> click on `_M. lepromatosis_`.

`UF104` is still behaving strangely. It has a very long branch length. Inpsect the alignment data to see if you can find out why. Go to the main MEGA11 window and select the `TA square` to bring up the alignments. Scroll down to sample `UF104` (it should be row #40).

- What do you notice about the variant calls for UF104? What does it mean?

- Why do you think the branch length is so long?

Go back to the _M. leprae_ mapping report.

- What information from the mapping report suggests that `UF104` does not contain authentic *M. leprae* aDNA? 

Remove `UF104` from the MSA and make another ML tree. Open `M.leprae_SantLlatzer_MSA.fasta` in a text editor and find sample `UF104`. Highlight the header and all the variant calls and erase them (back space). Save the edited file.

Upload the new MSA into MEGA11 as before: `Data` -> `Open File/Session` -> `modified_M.leprae_SantLlatzer_MSA.fasta` -> `Yes` -> `No` -> `Analyze` -> `Ok` -> `No`

On the main MEGA11 window, select `Phylogeny` -> `Construct/Test Maximum Likelihood Tree` -> `Yes`

Your ML run setup should look like this (same as above):

<img src="https://github.com/Kelzor/Introductory-ancient-DNA-practical/blob/main/aDNA%20Data%20Analysis%20II%20-%20M.%20leprae/images/9MEGA.png" alt="MEGA9" width="800">

This will take another 10-15 minutes to run. 

Once it is finished, don’t forget to root it. `Adjust Layout` -> `Tree Width` to make the tree easier to interpret.

It is a good indicator that your phylogenetic inference reflects the true evolutionary relationships when the unrooted and rooted trees have the same topology.

- Do the rooted and unrooted ML tree have the same topology?

- In which branch(es) do the Sant Llátzer *M. leprae* genomes fall?

- What other samples are in this branch? How old are they and where were they sampled?

[A great resource for learning how to interpret pathogen phylogenetic trees](https://artic.network/how-to-read-a-tree.html)

