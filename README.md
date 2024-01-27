# **tagIMPUTE**

# **tagIMPUTE v1.0 : Tag-based imputation**

**tagIMPUTE**\
is a command-line program for the imputation of untyped SNPs.

**tagIMPUTE**\
is based on a few flanking SNPs that can optimally predict the SNP under imputation. For more details, see Hu and Lin (2010).

## **SYNOPSIS**

**tagIMPUTE** \[**-gfile** genofile\] \[-**efile** exterfile\] \[**-h**\] \[**-hfile** haplofile\] \[**-ofile** outfile\] \[**-no_fix**\] \[**-no_remove**\] \[**-speed**\] \[**-panel** #trios #duos #singletons\] \[**-tag**\] \[**-window**\] \[**-max**\] \[**-min**\]

## **OPTIONS**

+----------------+---------------------------+------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| Option         | Parameter                 | Default                            | Description                                                                                                                              |
+:===============+:==========================+:===================================+:=========================================================================================================================================+
| **-gfile**     | {genofile}                | *`genotype.dat`*                   | Specify the genotype file                                                                                                                |
+----------------+---------------------------+------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **-efile**     | {exterfile}               | *`external.dat`*                   | Specify the external file                                                                                                                |
+----------------+---------------------------+------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **-h**         |                           | *`No haplotype file`*              | Use the haplotype file                                                                                                                   |
+----------------+---------------------------+------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **-hfile**     | {haplofile}               | *`haplotype.dat`*                  | Specify the haplotype file                                                                                                               |
+----------------+---------------------------+------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **-ofile**     | {outfile}                 | *`imp_geno.out`*                   | Specify the output file                                                                                                                  |
+----------------+---------------------------+------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **-no_fix**    |                           | Perform internal check             | Turn off internal check                                                                                                                  |
+----------------+---------------------------+------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **-no_remove** |                           | Remove SNPs that cannot be aligned | Turn off default removal of SNPs that cannot be aligned                                                                                  |
+----------------+---------------------------+------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **-speed**     |                           |                                    | -   1) Skip imputation of untyped SNPs that are not in the haplotype file.                                                               |
|                |                           |                                    |                                                                                                                                          |
|                |                           |                                    | -   2) Exclude SNPs not in the haplotype file as predictors.                                                                             |
|                |                           |                                    |                                                                                                                                          |
|                |                           |                                    | -   This will significantly speed up the program, but may lose important untyped SNPs. This flag is meaningful only when **-h** is used. |
+----------------+---------------------------+------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **-panel**     | {#trio, #duo, #singleton} | 30 trios, 0 duo, 0 singleton       | Specify the number of trios, duos and singletons in the reference panel, respectively                                                    |
+----------------+---------------------------+------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **-tag**       | {#tags}                   | 4                                  | Specify the number of tag SNPs used to impute the SNP of interest.                                                                       |
+----------------+---------------------------+------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **-window**    | {win size}                | 50,000(bp)                         | Specify the maximum distance to the untyped SNP within which typed SNPs are identified as candidate tags.                                |
+----------------+---------------------------+------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **-max**       | {#SNPs}                   | 20                                 | Specify the maximum number of typed SNPs identified as candidate tags                                                                    |
+----------------+---------------------------+------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **-min**       | {#SNPs}                   | 8                                  | Specify the minimum number of types SNPs identified as candidate tags                                                                    |
+----------------+---------------------------+------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------+

**tagIMPUTE** is based on a fixed number of tag SNPs (**-tag**) to impute both typed SNPs with partially missing data and untyped SNPs that are on an external panel. For each SNP under imputation, **tagIMPUTE** first identifies candidate tags within a distance (**-window**) and then finds the predefined number of SNPs that yields the largest MD measure (Nicolae, 2006, Genetic Epidemiology, 30, 703-717). If the number of candidate tags within that distance is less than the minimum (**-min**), the distance is enlarged until the minimum number is met. If the number of candidate tags within the distance exceeds the maximum (**-max**), only the closest maximum number of SNPs are considered as candidate tags. We perform an internal check to see whether the strand alignment between the study and external panel can be determined from (a) the allele labels (at non A/T and G/C SNPs), and (b) allele frequencies (at A/T and G/C SNPs). SNPs that cannot be aligned are removed from the data. The internal check and removal can be turned off by using the **-no_fix** and **-no_remove** flag. Phased haplotypes can be supplied by **-h** to facilitate the selection of tag SNPs. The **-speed** flag can further speed up the selection process at the cost of skipping SNPs that are not phased.

## **INPUT FILES**

### **genotype file**

The same as the [genotype file](http://dlin.web.unc.edu/software/snpmstat/#input_genotype_file) for [SNPMStat](http://dlin.web.unc.edu/software/snpmstat/)

### **external genotype file**

The same as the [external genotype file](http://dlin.web.unc.edu/software/snpmstat/#input_external_file) for [SNPMStat](http://dlin.web.unc.edu/software/snpmstat/)

### **haplotype file**

The same as the [haplotype file](http://dlin.web.unc.edu/software/snpmstat/#input_haplotype_file) for [SNPMStat](http://dlin.web.unc.edu/software/snpmstat/)

## **OUTPUT**

Untyped SNPs with allele frequencies of 0.0 or 1.0 in the reference panel are excluded, and so are the typed SNPs with allele frequencies of 0.0 or 1.0 in the study data. The posterior probabilities of genotypes, in the order of AA, AG and GG for an A/G SNP, are output to *`imp_geno.out`* by default, unless the intended file name is specified by **-output**. The first column shows the proportion of non-missing genotypes for typed SNPs and ‘no’ for untyped SNPs.

## **EXAMPLE**

The example includes a genotype file “`GAWgeno.dat`“, an external file “`GAWexter.dat`“, and a phased haplotype file “`GAWhaplo.dat`“.

Enter the command

> \$ tagIMPUTE -gfile GAWgeno.dat -efile GAWexter.dat -h -hfile GAWhaplo.dat -speed -ofile GAW_imp.out

to obtain the results given in “`GAW_imp.out`“.

## **REFERENCE**

Hu, Y. J. and Lin, D. Y. (2010), “Analysis of Untyped SNPs: Maximum Likelihood and Imputation Methods”, *Genetic Epidemiology*, 34, 803-815.

## **DOWNLOAD**

#### **tagIMPUTE for Linux \[updated July 13 2010\]**

executable (zip archive) **»** [tagIMPUTE-1.0-linux.zip](http://dlin.web.unc.edu/wp-content/uploads/sites/1568/2013/01/tagIMPUTE-1.0-linux.zip)

#### **Example files \[updated July 13 2010\]**

zip archive **»** [tagIMPUTE-1.0-example.zip](http://dlin.web.unc.edu/wp-content/uploads/sites/1568/2013/01/tagIMPUTE-1.0-example.zip)

## **VERSION HISTORY**

| Version | Date      | Description             |
|:--------|:----------|:------------------------|
| 1.0     | Jul. 2010 | First version released. |
