What I learned today?

Today, we assembled the contigs from day2 through "binning" process.
The steps that we followed:
- Evaluating Assembly Quality with quast:
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
      -  Now, we mapping reads onto contigs with bowtie2 with the .sam result (Sequence Alignment Map)
      ```
       bowtie2 -1 BGR_130305_mapped_R1.fastq.gz -2 BGR_130305_mapped_R2.fastq.gz -S BGR_130305.sam -x ../index_output --very-fast 
bowtie2 -1 BGR_130527_mapped_R1.fastq.gz -2 BGR_130527_mapped_R2.fastq.gz -S BGR_130527.sam -x ../index_output --very-fast 
bowtie2 -1 BGR_130708_mapped_R1.fastq.gz -2 BGR_130708_mapped_R2.fastq.gz -S BGR_130708.sam -x ../index_output --very-fast 
```
    -  In the next step we convert the .sam file to binary format (.bam).
    ```
    samtools view -Sb BGR_130305.sam > BGR_130305.bam
samtools view -Sb BGR_130305.sam > BGR_130527.bam
samtools view -Sb BGR_130305.sam > BGR_130708.bam
```
    -  at the end, we sorting the mapped reads for higher processing speed, analyzing, visualization, and so on.
    ```
    anvi-init-bam metagenomics/fastp_out/sample1.bam -o metagenomics/fastp_out/sample1_sorted.bam
    ```
- Binning reads into MAGs and figuring out which microbes are present in the samples
    -  To do so first converted my fasta file to more poweful type which could also identify ORFs:
    ```
    anvi-gen-contigs-database -f metagenomics/contigs.anvio.fa -o metagenomics/contigs.db -n biol217
    ```
    -  In the next step we could run hmm search on genes to see if the ORFs that were predicted by (anvi-gen-contigs-database) are similar to any known genes:
    ```
    anvi-run-hmms -c metagenomics/contigs.db --num-threads 4
    ```
        -  To visualize the contigs database:







