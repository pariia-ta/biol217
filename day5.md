What I did today?

- Taxonomic Assignment
I added taxonomic annotations to the MAGs, using anvi-run-scg-taxonomy.

```
anvi-run-scg-taxonomy -c contigs.db -T 20 -P 2
```

This program makes quick taxonomy estimation for genomes, metagenomes, in contigs.db by using single copy core genes.

```
anvi-estimate-scg-taxonomy -c contigs.db -p $WORK/metagenomics/anvi_contigs/BGR_130305_profile/PROFILE.db --metagenome-mode --compute-scg-coverages --update-profile-db-with-taxonomy > temp_BGR_130305.txt
anvi-estimate-scg-taxonomy -c contigs.db -p $WORK/metagenomics/anvi_contigs/BGR_130527_profile/PROFILE.db --metagenome-mode --compute-scg-coverages --update-profile-db-with-taxonomy > temp_BGR_130527.txt
anvi-estimate-scg-taxonomy -c contigs.db -p $WORK/metagenomics/anvi_contigs/BGR_130708_profile/PROFILE.db --metagenome-mode --compute-scg-coverages --update-profile-db-with-taxonomy > temp_BGR_130708.txt

```

At the end, I got the summary information about my METABAT bins.

```

anvi-summarize -p $WORK/metagenomics/anvi_contigs/BGR_merge/merged_PROFILE.db -c contigs.db -o $WORK/day5/summary_metabat2 -C METABAT2

```

- Questions

-Did you get a species assignment to the ARCHAEA bins previously identified?
METABAT__11: Methanoculleus thermohydrogenotrophicum (low quality)
METABAT__39: Methanoculleus sp012797575 (high quality)
METABAT__32: Methanosarcina flavescens (low quality)

-Does the HIGH-QUALITY assignment of the bin need revision?
No, because the genome is almost complete, the taxonomy is coherent, the markers agree.

- Genome dereplication(BONUS):

First I made the table 