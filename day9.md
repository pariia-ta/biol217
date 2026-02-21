- Questions

1. How many free viruses are in the BGR_140717 sample?
846

```
grep ">" -c 01_GENOMAD/BGR_140717/BGR_140717_Viruses_Genomad_Output/BGR_140717_modified_summary/*_virus.fna

```

2. How many proviruses are in the BGR_140717 sample?
11

```

grep ">" -c 01_GENOMAD/BGR_140717/BGR_140717_Proviruses_Genomad_Output/proviruses_summary/*_virus.fna

```

3. How many Caudoviricetes viruses are in all samples together? (Use the filtered version)
10764

```

grep -c "Caudoviricetes" 02_CHECK_V/BGR_*/MVP_02_BGR_*_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv

```

4. How many unclassified viruses are in all samples together? (Use the filtered version)
145

```

grep -c "Caudoviricetes" 02_CHECK_V/BGR_*/MVP_02_BGR_*_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv

```

5. What other taxonomies are there across all samples?
Mimiviridae, Phycodnaviridae, Viruses, Autolykiviridae, Retroviridae, Microviridae

```

grep -v "Caudoviricetes" 02_CHECK_V/BGR_*/MVP_02_BGR_*_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv |grep -v "Unclassified" |grep -v "Sample" > 5.csv

```

6. How many High-quality and Complete viruses are in all samples together? (Use the filtered version and focus on the CheckV quality)
5

```
cut -f 8 02_CHECK_V/BGR_*/MVP_02_BGR_*_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv | grep -e "High-quality" -e "Complete" |grep -c ""

```

7. Create a table based on the CheckV quality with the columns Sample, Low-quality, Medium-quality, High-quality, Complete. Fill it with the amount of viruses for each of the categories.
BGR_130305
Low-quality
625
Medium-quality
0
High-quality
0
Complete
0
BGR_130527
Low-quality
693
Medium-quality
1
High-quality
0
Complete
0
BGR_130708
Low-quality
860
Medium-quality
1
High-quality
0
Complete
0
BGR_130829
Low-quality
964
Medium-quality
3
High-quality
0
Complete
0
BGR_130925
Low-quality
856
Medium-quality
3
High-quality
0
Complete
0
BGR_131021
Low-quality
1091
Medium-quality
3
High-quality
0
Complete
1
BGR_131118
Low-quality
613
Medium-quality
3
High-quality
0
Complete
0
BGR_140106
Low-quality
372
Medium-quality
0
High-quality
0
Complete
0
BGR_140121
Low-quality
578
Medium-quality
0
High-quality
0
Complete
1
BGR_140221
Low-quality
724
Medium-quality
1
High-quality
1
Complete
0
BGR_140320
Low-quality
471
Medium-quality
1
High-quality
0
Complete
0
BGR_140423
Low-quality
362
Medium-quality
0
High-quality
0
Complete
0
BGR_140605
Low-quality
531
Medium-quality
2
High-quality
0
Complete
0
BGR_140717
Low-quality
564
Medium-quality
3
High-quality
0
Complete
1
BGR_140821
Low-quality
338
Medium-quality
1
High-quality
1
Complete
0
BGR_140919
Low-quality
485
Medium-quality
1
High-quality
0
Complete
0
BGR_141022
Low-quality
409
Medium-quality
0
High-quality
0
Complete
0
BGR_150108
Low-quality
348
Medium-quality
1
High-quality
0
Complete
0

```

for x in BGR_130305  BGR_130527  BGR_130708  BGR_130829  BGR_130925  BGR_131021  BGR_131118  BGR_140106  BGR_140121  BGR_140221  BGR_140320  BGR_140423  BGR_140605  BGR_140717  BGR_140821  BGR_140919  BGR_141022  BGR_150108; do echo $x; echo  "Low-quality"; cut -f 8 02_CHECK_V/"$x"/MVP_02_"$x"_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv | grep -c "Low-quality" ; echo  "Medium-quality"; cut -f 8 02_CHECK_V/"$x"/MVP_02_"$x"_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv | grep -c "Medium-quality"; echo  "High-quality"; cut -f 8 02_CHECK_V/"$x"/MVP_02_"$x"_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv | grep -c "High-quality"; echo  "Complete"; cut -f 8 02_CHECK_V/"$x"/MVP_02_"$x"_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv | grep -c "Complete"; done

```

8. For the Complete viruses from all samples, extract all the lines (from the same output file you just used to answer previous questions) so we can take a closer look. Also add a header so we know what each column contains.
02_CHECK_V/BGR_131021/MVP_02_BGR_131021_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv:BGR_131021 BGR_131021_NODE_96_length_46113_cov_32.412567 46113 No 53 12 2 Complete High-quality 100.0 DTR (high-confidence) 1.0 11 0.9741 6 26.0838 Viruses;Duplodnaviria;Heunggongvirae;Uroviricota;Caudoviricetes dsDNA Prokaryote 02_CHECK_V/BGR_140121/MVP_02_BGR_140121_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv:BGR_140121 BGR_140121_NODE_54_length_34619_cov_66.823718 34619 No 47 24 0 Complete High-quality 100.0 DTR (high-confidence) 1.0 11 0.9812 10 41.8446 Viruses;Duplodnaviria;Heunggongvirae;Uroviricota;Caudoviricetes dsDNA Prokaryote 02_CHECK_V/BGR_140717/MVP_02_BGR_140717_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv:BGR_140717 BGR_140717_NODE_168_length_31258_cov_37.020094 31258 No 44 21 0 Complete High-quality 100.0 DTR (medium-confidence) 1.0 11 0.9818 10 40.2292 Viruses;Duplodnaviria;Heunggongvirae;Uroviricota;Caudoviricetes dsDNA Prokaryote

```

`grep "Complete" 02_CHECK_V/BGR_/MVP_02_BGR__Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv > 8.csv"

```

9. In what samples were the complete viruses found?

-In what samples were the complete viruses found?
Answer: BGR_131021, BGR_140121 and BGR_140717
-Are they integrated proviruses?
Answer: No
-How long are they?
Answer: 46113, 34619 and 31258 nucleotides
-How many viral hallmark genes do they have?
Answer: 6, 10 and 10
-What percentage of the viral genes are viral hallmark genes?
Answer: 50%, 42% and 48%
-Why are there more genes (gene_count) than viral genes and host genes combined?
Answer: Gene_count are all genes encoded in this sequence. We know for sure that some match known host and virus genes (viral_genes, host_genes). For the other genes, we simply don't know. They are just normal (viral) genes without any matches to the reference databases of geNomad and CheckV.

10. Find the clustering output. What is the difference between the three .tsv files?
 Unfiltered (All output), Filtered (Potential not virus sequences are filtered) and Representative (One representative of each bin is chosen)

11. How many cluster representatives are there?

5375

```

grep -c "" 03_CLUSTERING/MVP_03_All_Sample_Filtered_Relaxed_Merged_Genomad_CheckV_Representative_Virus_Proviruses_Quality_Summary.tsv

```

12. How many of the cluster representatives are proviruses?

91

```

cut -f 5 03_CLUSTERING/MVP_03_All_Sample_Filtered_Relaxed_Merged_Genomad_CheckV_Representative_Virus_Proviruses_Quality_Summary.tsv | grep "Yes" -c

```

13. What clusters do the complete viruses from 8. belong to? How large are the clusters?

BGR_131021_NODE_96_length_46113_cov_32.412567,   BGR_131021_NODE_96_length_46113_cov_32.412567 Members: 6 BGR_140121_NODE_54_length_34619_cov_66.823718,    BGR_140121_NODE_54_length_34619_cov_66.823718 Members: 24 BGR_140717_NODE_168_length_31258_cov_37.020094,    BGR_140717_NODE_168_length_31258_cov_37.020094 Members: 10s

```

grep -e "BGR_131021_NODE_96_length_46113_cov_32.412567" -e "BGR_140121_NODE_54_length_34619_cov_66.823718" -e "BGR_140717_NODE_168_length_31258_cov_37.020094" 03_CLUSTERING/MVP_03_All_Sample_Filtered_Relaxed_Merged_Genomad_CheckV_Representative_Virus_Proviruses_Quality_Summary.tsv for x in BGR_131021_NODE_96_length_46113_cov_32.412567 BGR_140121_NODE_54_length_34619_cov_66.823718 BGR_140717_NODE_168_length_31258_cov_37.020094; do echo $x; cut -f 2 03_CLUSTERING/MVP_03_All_Sample_Filtered_Relaxed_Merged_Genomad_CheckV_Representative_Virus_Proviruses_Quality_Summary.tsv | grep "$x" | grep "," -o |grep "" -c; done

```

14. Now we want to look at abundances. The files can be found in 04_READ_MAPPING, split per sample. Open any of the CoverM files and ignore everything except the first two columns. What does this file tell you, conceptionally? Meaning: What do the lines in this file represent (i.e. what kind of data is described in line 2, line 3, .....), who do the IDs listed in the second column belong to, what kind of data was combined to generate this file, and what can we learn from it?




