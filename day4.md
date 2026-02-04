What I did today:
Today I tried to examine and improve the quality of my bins.

- MAGs quality

  -    Genome completeness:

here I want to estimate the completness and redundancy of each bin (hypothetically MAG) that I binned yesterday by MetaBat2, using: 

```
anvi-estimate-genome-completeness -c metagenomics/contigs.db -p metagenomics/fastp_out/merged_profiles/PROFILE.db -C METABAT2
```

The data also provided by report.html file on day3.

- Customizing the bins

In this step, again I entered the interactive session to open the link
 using this command: 

 ```
 anvi-interactive -p metagenomics/fastp_out/merged_profiles/PROFILE.db -c metagenomics/contigs.db -C METABAT2
 ```

 ![merged_profile](merged_profile)

 - Questions

 -Which binning strategy gives you the best quality for the archaea bins? 
 MetaBat2

 -How many archaea bins do you get that are of High quality? 1 archaea with 94.74% completeness and 2.63% redundancy.

 How many bacteria bins do you get that are of High quality? if we consider the 95% threshold for completeness and under 5% redundancy, I got 1 bacteria.


 - Refining archaea bins

 In this step, I copied the file MetaBat__39, to change it (refine it).

 - Detecting chimeras in MAGs by using GUNC

 Here I checked if the bins have any potential contamination (chimeras) with GUNC, using:

 ```
 gunc run -i metagenomics/fastp_out/refine_archaea39/METABAT__39/METABAT__39-contigs.fa -r $WORK/databases/gunc/gunc_db_progenomes2.1.dmnd --out_dir metagenomics/fastp_out/gunc_out --detailed_output --threads 12
 ```

 


