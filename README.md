# Base Editor Design Tool

This code designs every possible guide in the CDS region, extending 30 nucleotides into intronic and UTR regions for user-defined transcripts.
It then annotates the possible edits for each guide. In its original implementation, amino acid change predictions were generated for single base edits; 
however, for certain codons (e.g., CCC encoding Proline), the tool does not explicitly account for cases where multiple adjacent cytosines may be edited 
simultaneously. To address this, a custom function was added to predict these additional mutation outcomes. 
Separate files annotating ClinVra SNPs are also generated.

---

## Authors

Mudra Hegde, Ruth Hanna  
Email: mhegde@broadinstitute.org, rhanna@broadinstitute.org  

**Version:** 2.0 (debugged and updated)

**Contributors:** Murugesh Narayanappa (mnarayan@stanford.edu) (debugging and bug fixes)

---

## Inputs

**Input File:**  
`.txt` file with list of Ensembl transcript IDs in the first column and gene symbols in the second column OR FASTA file with nucleotide sequence  

**Variant File:**  
File with ClinVar SNPs (variant_summary.txt). This file can be downloaded from:  
ftp://ftp.ncbi.nlm.nih.gov/pub/clinvar/tab_delimited/variant_summary.txt.gz  

**Input type:**  
Indicate whether the file contains a list of transcripts or a nucleotide sequence  

**Base editor type:**  
Indicate the type of base editor (Rees et al., 2018) for which designs are required. This choice dictates the choice of PAM, sgRNA length, editing window, and type of edit  

**PAM:**  
PAM preference if BE type has not been selected; Default: NGG  

**Edit window:**  
Editing window relative to nucleotide position in sgRNA, if BE type has not been selected; Default: 4–8  

**sgRNA length:**  
Length of sgRNA excluding PAM sequence, if BE type has not been selected; Default: 20  

**Edit:**  
Type of edit made by base editor, if BE type has not been selected; Default: all (annotates both C→T and A→G edits)  

**Intron buffer:**  
Number of bp into the intron to consider for guide design  

**Filter GC:**  
Whether to filter out edits in a GC motif  

**Output name:**  
Name for output folder  

## Appending Missed Mutations from the Base Editor Design Tool:
A custom function has been added to account for previously missed mutations. Please refer to the .Rmd file, which includes both the code and an example.
Purpose: Identify and append these missed dual-edit mutations and their categories to the output dataframe.
Background: The tool predicts amino acid changes by evaluating one nucleotide at a time.However, when a codon contains “AA” or “CC” in the first two positions (before the wobble base), in either strand orientation (5’→3’ or 3’→5’) the base editor is likely to edit both nucleotides simultaneously:
Adenine Base Editor (ABE): AA → GG
Cytosine Base Editor (CBE): CC → TT
Since the tool only evaluates single-nucleotide edits, it does not predict the amino acid outcomes resulting from these probable dual-edit events by the base editor.

