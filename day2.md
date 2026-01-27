What I did today:

1. Processing the raw reads with FastQC
. activating the environment:
```
micromamba activate 00_anvio
```
. entering the WORK space:
```
cd $WORK
```
. Creating an script in VScode with the template.sh file example:
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
Now, we have fastqc_out file which provides us a report of sequencing quality across all bases. the report shows that the read quality decreases toward the 3' end, suggesting the trimming of last bases.

### FastQC Per Base Quality Plot
![FastQC per base quality](per_base_quality)



