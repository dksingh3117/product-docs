.. _omics_intro:


Omics Intro
!!!!!!!!!!!

.. _omics_intro:

Genomics to Clinical Guidelines
@@@@@@@@@@

Genomics can be connected to clinical practice through the diagram shown below:

.. image:: /_static/science/Gen_clin_guidenlines.png

The process starts with acquiring patient genomic data. Next Generation Sequencing (NGS) data usually enter the system as BAM files (analyzed, mapped and annotated) with BAI index files, dependent on genome assembly versions. Microarray gene expression data format depends on the manufacturer (ie, Affy, Illumina) and chip versions. Most other data file formats are flat files, vcf, tab-delimited, often coded.

From these data, clinically relevant alleles (alternative forms of the same gene) and haplotypes (groups of alleles) are identified. 

.. image:: /_static/science/genetorec.png

Alleles and haplotypes can quickly become complex, and new discoveries are constantly made. Fortunately, we can make use of star alleles. Star alleles are haplotype patterns that have been defined at the gene level and, in many cases, associated with protein activity levels. Genetic variants within a haplotype can include single nucleotide polymorphisms (SNPs), Insertion/Deletions (InDels), and copy number variants (CNVs).

.. image:: /_static/science/haplotypes.png

Once we have applied the right nomenclature, we can then start to correlate the specific genomic characteristics of the individual with phenotypes (diseases, health conditions, or reactions ot medications) and combined with drugs and clinical trials to form clinical guidelines for doctors. 

.. image:: /_static/science/genephenotype.png

Guidelines are the implications of the gene-phenotype, gene-drug, or drug-drug interaction. For example, "increased risk of morphine formation following codeine adminstration, leading to higher risk of toxicity." These are then converted to recommendations, which actually tell the health care provider what to do. For example, "Avoid codeine use due to potential for toxicity."

.. image:: /_static/science/implic_recommend.png
