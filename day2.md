### What I did today:


### Quality Control with FastQC
1. activating the environment:
```
micromamba activate 00_anvio
```
2. entering the WORK space:
```
cd $WORK
```
3. Creating a script in VScode with the template.sh file example:

#!/bin/bash

#SBATCH --job-name=fastqc

#SBATCH --output=fastqc.out

#SBATCH --error=fastqc.err

#SBATCH --nodes=1

#SBATCH --ntasks-per-node=1

#SBATCH --cpus-per-task=12

#SBATCH --mem=32G

#SBATCH --partition=base

#SBATCH --time=03:00:00

#SBATCH --reservation=biol217

#load necessary modules

module load gcc12-env/12.1.0

module load micromamba

eval "$(micromamba shell hook --shell=bash)"
export MAMBA_ROOT_PREFIX=$WORK/.micromamba

micromamba activate 00_anvio 


cd $WORK/metagenomics
mkdir -p fastqc_out
cd 0_raw_reads
fastqc *.fastq.gz -o ../fastqc_out
. submitting the fastqc.sh file:
```
sbatch fastqc.sh
```
4. Now, we have fastqc_out file which provides us a report of sequencing quality across all bases. the report shows that the read quality decreases toward the 3' end, suggesting the trimming of last bases.

### FastQC Per Base Quality Plot
![FastQC per base quality](per_base_quality)

### Preprocess the raw sequencing reads with Fastp
1. again creating a script in VScode:

#!/bin/bash

#SBATCH --job-name=fastp

#SBATCH --output=fastp.out

#SBATCH --error=fastp.err

#SBATCH --nodes=1

#SBATCH --ntasks-per-node=1

#SBATCH --cpus-per-task=12

#SBATCH --mem=32G

#SBATCH --partition=base

#SBATCH --time=03:00:00

#SBATCH --reservation=biol217

#load necessary modules
module load gcc12-env/12.1.0
module load micromamba
eval "$(micromamba shell hook --shell=bash)"
export MAMBA_ROOT_PREFIX=$WORK/.micromamba

micromamba activate 00_anvio 


cd $WORK/metagenomics
mkdir -p fastp_out

fastp -i 0_raw_reads/BGR_130305_mapped_R1.fastq.gz -I 0_raw_reads/BGR_130305_mapped_R2.fastq.gz -o fastp_out/BGR_130305_mapped_R1.fastq.gz -O fastp_out/BGR_130305_mapped_R2.fastq.gz -t 6 -q 20 -h fastp_out/BGR_130305_mapped_R1.html -R BGR_130305_mapped_R2

fastp -i 0_raw_reads/BGR_130527_mapped_R1.fastq.gz -I 0_raw_reads/BGR_130527_mapped_R2.fastq.gz -o fastp_out/BGR_130527_mapped_R1.fastq.gz -O fastp_out/BGR_130527_mapped_R2.fastq.gz -t 6 -q 20 -h fastp_out/BGR_130527_mapped_R1.html -R BGR_130527_mapped_R2

fastp -i 0_raw_reads/BGR_130708_mapped_R1.fastq.gz -I 0_raw_reads/BGR_130708_mapped_R2.fastq.gz -o fastp_out/BGR_130708_mapped_R1.fastq.gz -O fastp_out/BGR_130708_mapped_R2.fastq.gz -t 6 -q 20 -h fastp_out/BGR_130708_mapped_R1.html -R BGR_130708_mapped_R2

2. submit the file in the terminal, using:
```
sbatch fastp.sh
```
3. Now we have fastp_out file which contains trimmed fastq file along with the quality report (six bases were trimmed from the tail and Phred quality score of 20 was used as the filtering threshold)

### Assembling the reads to generate contigs with megahit

1. creating a script in VScode:








2. submitting the assembly.sh file, using:
```
sbatch assembly.sh
```
3. Waiting for one hour and half

4. Now we have final.contigs.fa file

5. Converting the final.contigs.fa to fastg file format (fasta-like graph), using:
```
megahit_toolkit contig2fastg 99 final.contigs.fa > final.contigs.fastg
```
6. Visualize the file (contigs) in Bandage
![Assembly graph visualized in Bandage](bandage_graph)

### Question 
Attach the figure you generated and explain briefly in your own words what you can see?
Each colored line represents a contig in the assembly graph. The connections between lines indicate overlaps between sequences, showing how different contigs are linked.
At the top of the graph, there are fewer and longer contigs, which likely represent well-assembled regions with high coverage.
the bottom part of the graph contains many short, dense, and fragmented contigs, suggesting repetitive regions, low coverage areas.



