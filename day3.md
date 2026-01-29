What I learned today?

Today, we assembled the contigs from day2 through "binning" process.
The steps that we followed:
1. Evaluating Assembly Quality with quast:
-First, created a script containing the commond and directory paths:

```
 metaquast metagenomics/final.contigs.fa -o metagenomics/fastp_out/metaquast_out/ -t 6 -m 1000
 ```
 and then using:
 ```
 metaquast metagenomics/final.contigs.fa -o metagenomics/fastp_out/metaquast_out/ -t 6 -m 1000
 ```
 This command, results in report.html file which contains the info and quality of assembled contigs:
  ![Quast report](Quast_report)

  ### Questions
  1. What is your N50 value? Why is this value relevant?
  The N50 is 3014bp, which represents the contig length at which 50% of the total assembly length is contained in contigs of that size or longer. A higher N50 shows a more contiguous assembly.

  2. How many contigs are assembled?
  A total of 55,836

  3. What is the total length of the contigs?
  Total length: 147,642,586

  - Mapping Sequencing reads to check for contamination, incorrect binning, and so on.
 -  First, reformatting the fasta file into more efficient file:
  ```
  anvi-script-reformat-fasta final.contigs.fa -o reformatted_output.fasta --min-len 1000 --simplify-names --report-file final.contigs.reformatted.table
  ```
 -  Second, we want to organize and index our contigs. wich makes the mapping process faster, using bowtie program:
  ```
  bowtie2-build reformatted_output.fasta index_output
  ```
  -   







