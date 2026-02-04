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
    
    For the visualization parts, mostly we connect to interactive web server, T do so: 
    1. request computing resources
   
    ```
    srun --pty --x11 --partition=interactive --nodes=1 --tasks-per-node=1 --cpus-per-task=1 --mem=10G --time=01:00:00 /bin/bash
    ```

    2. activate anvi'o environments

     ```
     module load gcc12-env/12.1.0
     module load micromamba 2> /dev/null
     micromamba activate $WORK/.micromamba/envs/00_anvio/
     cd $WORK
     ```

     3. Then run my interactive command

     ```
     anvi-profile -i metagenomics/fastp_out/sample1_sorted.bam -c metagenomics/contigs.db -o metagenomics/fastp_out/sample1_profile/
     ```

     4. Then open the new tab on the terminal and using: 

     ```
     ssh -L localhost:8081:localhost:8081 suna246@caucluster.rz.uni-kiel.de
     ```

     The local host number should be based on the link that we get from the previous command on the first terminal tab (here:8081), and also now in this step the sunam number is the interactive local node (246, not 229)

     5. at the end 

     ```
     ssh -L localhost:8081:localhost:8081 n246
     ```

     6. Now we can click on the http://127.0.0.1:8080/ like link and open the visualized data. 

    ![contigs_visualization](contigs_visualization)

- Creating anvio's profile:

To store read mapping results and detailed information about each nucleotide, using

```
anvi-profile -i 130305_sorted_output.bam -c ../contigs.db --output-dir sample1_profile/
anvi-profile -i 130527_sorted_output.bam -c ../contigs.db --output-dir sample2_profile/
anvi-profile -i 130708_sorted_output.bam -c ../contigs.db --output-dir sample3_profile/
```

Now we have three profile databases for each sample.
anvi profile takes:
. A mapped bam file
. A contigs database (contigs.db)
and generates a profile. which contains: 
-coverage: (which organisms are more abundant in each sample)
-detection: (If a genome is present in all samples)
-SNVs (variation)

- Merging all three sample profiles together:

```
anvi-merge sample1_profile/PROFILE.db sample2_profile/PROFILE.db sample3_profile/PROFILE.db -o merged_profiles -c ../contigs.db --enforce-hierarchical-clustering
```

- Binning Contigs into genomes:

In this step, we use two tools for binning:
1. MetaBAT2
2. MaxBin2


```
anvi-cluster-contigs -p merged_profiles/PROFILE.db -c ../contigs.db -C METABAT2 --driver metabat2 --log-file metabat2.log --just-do-it
anvi-summarize -p merged_profiles/PROFILE.db -c ../contigs.db -o SUMMARY_METABAT2 -C METABAT2
```

here, our result is a bin collection 0f 40, which we have it as a report METABAT2 file: 

![METABAT2](METABAT2)


```
anvi-cluster-contigs -p merged_profiles/PROFILE.db -c ../contigs.db -C MAXBIN2 --driver maxbin2 --log-file maxbin2.log --just-do-it
anvi-summarize -p merged_profiles/PROFILE.db -c ../contigs.db -o SUMMARY_MAXBIN2 -C MAXBIN2
```

![MaxBin2](MaxBin2)

- Questions

-How many archaea bins did you get from MetaBAT2?
I got 3 archaea bins from MetaBAT2.

-How many archaea bins did you get from Maxbin2?
I got 1 archaea bin from MaxBin2.



















