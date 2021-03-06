# TRGN-510---Final-Project-



#### Analysis of a PGP person using germline variant calling

The aim of this project is to analyze the data of a PGP person using gremline variant calling. I will first obtain appropriate data from the Harvard PGP database. Next, the fastq data is going to be aligned with a reference genome using BWA software to reproduce the germline variance between the two sequences. The identified result is saved in SAM format and the same is converted into BAM for easier manipulation. Only the anomalies between two sequences are further filtered out using SNPeff and the output will be in VCF. Finally, a front end view will be created using R programming.  


#### 1)Log onto server 

`ssh varshagi@itg.usc.edu`

#### 2)Download data

A. Downloading patient data

Create a directory to store the data: 

`mkdir /home/varshagi/Alz`

`cd Alz`

Download the data into this directory: 

` wget ftp://ftp-trace.ncbi.nih.gov/giab/ftp/data/NA12878/Garvan_NA12878_HG001_HiSeq_Exome/NIST7035_TAAGGCGA_L001_R1_001.fastq.gz `

`wget ftp://ftp-trace.ncbi.nih.gov/giab/ftp/data/NA12878/Garvan_NA12878_HG001_HiSeq_Exome/NIST7035_TAAGGCGA_L001_R2_001.fastq.gz`

Unzip the files:

`gunzip NIST7035_TAAGGCGA_L001_R1_001.fastq.gz NIST7035_TAAGGCGA_L001_R2_001.fastq.gz NIST7035_TAAGGCGA_L002_R1_001.fastq.gz NIST7035_TAAGGCGA_L002_R2_001.fastq.gz`

Confirm the presence of these files:

`ls`

`ls -lh NIST7035_TAAGGCGA_L001_R1_001.fastq NIST7035_TAAGGCGA_L001_R2_001.fastq NIST7035_TAAGGCGA_L002_R1_001.fastq NIST7035_TAAGGCGA_L002_R2_001.fastq` (ls -lh gives the size of the files) 


B. Downloading reference data 

Create a directory: 

`mkdir /home/varshagi/refgen`

`cd refgen`

Download data into refgen:

`wget ftp://ftp.ncbi.nlm.nih.gov/1000genomes/ftp/technical/reference/human_g1k_v37.fasta.fai`

`wget ftp://ftp.ncbi.nlm.nih.gov/1000genomes/ftp/technical/reference/human_g1k_v37.fasta.gz`

Unzip file:

`gunzip human_g1k_v37.fasta`

Check file sizes:

`ls`

`ls -lh human_g1k_v37.fasta`


#### 3)Installing BWA and Samtools

Create a directory to contain all the scripts for this project: 

`mkdir /home/varshagi/scripts`

A. BWA

`mkdir /home/varshagi/bwa_build`

`cd bwa_build`

`git clone https://github.com/lh3/bwa.git`
  
`cd bwa
 make `
 
To verify its function:

`/home/varshagi/bwa_build/bwa/bwa `

If BWA has compiled successfully, the following should be seen:

*Program: bwa (alignment via Burrows-Wheeler transformation)*
*Version: 0.7.17-r1188*
*Contact: Heng Li <lh3@sanger.ac.uk>*

*Usage: bwa <command> [options]*

*Command: index index sequences in the FASTA format*
 *mem BWA-MEM algorithm*
 *fastmap identify super-maximal exact matches*
 *pemerge merge overlapping paired ends (EXPERIMENTAL)*
 *aln gapped/ungapped alignment*
 *samse generate alignment (single ended)*
 *sampe generate alignment (paired ended)*
 *bwasw BWA-SW for long queries*

*shm manage indices in shared memory*
 *fa2pac convert FASTA to PAC format*
 *pac2bwt generate BWT from PAC*
 *pac2bwtgen alternative algorithm for generating BWT*
 *bwtupdate update .bwt to the new format*
 *bwt2sa generate SA from BWT and Occ*

*Note: To use BWA, you need to first index the genome with `bwa index'.*
 There are three alignment algorithms in BWA: `mem', `bwasw', and*
 `aln/samse/sampe'. If you are not sure which to use, try `bwa mem'*
 first. Please `man ./bwa.1' for the manual.*
 
 
B. Samtools
 
`mkdir /home/varshagi/samtools_build`
 
`cd samtools_build `

`wget https://github.com/samtools/samtools/releases/download/1.7/samtools-1.7.tar.bz2 ` 
   
 Unzip and untar data respectively: 
   
 `bunzip2 samtools-1.7.tar.bz2 
  tar -xvf samtools-1.7.tar`
   
 Navigate to working directory:
   
 `cd samtools-1.7`
   
 Configure the script:
   
 `./configure`
   
 *Incase of error in compilation*
   
 `./configure --without-curses`
   
 To make the compiled program in the local directory:
   
 `make DESTDIR=~/bin/samtools_build/samtools-1.7` 
   
 Verify compilation status in the working directory:
   
 `./samtools`
   
 Verify BWA and Samtools exist in the server: 
   
 `cd ~`
 `samtools` checkes for samtools
 `bwa ` checks for bwa 
  
 #### 4)Build an index to make data search faster 
  
`cd /home/varshagi/scripts 
 vim /home/varshagi/scripts/build_bwa_index.sh`
 
 build_bwa_index.sh
  
`#!/bin/bash
 mkdir -p /home/varshagi/refgen/
 ######################################### #
 #Running Analysis
 #########################################
 echo Running Analysis echo Path is `pwd`
 cd /home/varshagi/refgen/
 bwa index /home/varshagi/refgen/human_g1k_v37.fasta`

Make the file executable:

`chmod 755 /home/varshagi/scripts/build_bwa_index.sh`

Run in background 

`nohup /home/varshagi/scripts/build_bwa_index.sh >& ~/scripts/build_bwa_index.out &`
 `disown`

#### 5)Alignment from Fastqs to BAMs

Create an alignemnt script: 

`vim /home/varshagi/scripts/bwa_align.sh`

bwa_align.sh

`#!/bin/bash
 mkdir -p /home/varshagi/scripts/bams
 cd /home/varshagi/scripts/bams/
 bwa mem /home/varshagi/refgen/human_g1k_v37.fasta /home/varshagi/Alz/NIST7035_TAAGGCGA_L001_R1_001.fastq    /home/varshagi/Alz/NIST7035_TAAGGCGA_L001_R2_001.fastq >>/home/varshagi/scripts/bams/sample1.sam 2>>error.align1
 bwa mem /home/varshagi/refgen/human_g1k_v37.fasta /home/varshagi/Alz/NIST7035_TAAGGCGA_L002_R1_001.fastq /home/varshagi/Alz/NIST7035_TAAGGCGA_L002_R2_001.fastq >>/home/varshagi/scripts/bams/sample2.sam 2>>error.align2
 samtools fixmate -O bam sample1.sam sample1.bam >& fix.thread1.log
 samtools fixmate -O bam sample2.sam sample2.bam >& fix.thread2.log
 samtools sort sample1.bam >> sample1.sort.bam  2>> sort.thread1.log
 samtools sort sample2.bam >> sample2.sort.bam  2>> sort.thread2.log
 samtools index sample1.sort.bam >& index.thread1.log
 samtools index sample2.sort.bam >& index.thread2.log
 wait
 samtools merge sample.merge.bam sample1.sort.bam sample2.sort.bam >& merge.log
 samtools index sample.merge.bam >& index.log
 rm *.sort.bam *.sam sample1.bam sample2.bam`
 
 Make the file executable: 
 
 `chmod 755 /home/varshagi/scripts/bwa_align.sh`
 
 Run the script:
 
 `~/scripts/bwa_align.sh  >&~/scripts/bwa_align.out &`
 `disown`
 
 Result: *sample.merge.bam.bai* AND *sample.merge.bam*
 
 (Note: Always check errors files like *error.align1*,*error.align2*, etc. incase the result does not show up)

#### 6)Obtaining VCFs from BAMs using freebayes

Install freebayes:

`cd ~`

`git clone --recursive git://github.com/ekg/freebayes.git`

`make`

Check if freebayes works:

`freebayes`

`freebayes -f /home/varshagi/refgen/human_g1k_v37.fasta /home/varshagi/scripts/bams/sample.merge.bam > /home/varshagi/scripts/bams/sample.freebayes.vcf >& log&`

Check if the file has data:

`cd /home/varshagi/scripts/bams/`

`ls`

`ls -lh sample.freebayes.vcf`

#### 7)Deducing results

The important risk factor for Alzheimer’s disease is a gene called Apolipoprotein E (APOE). There are three types of APOE genes - APOE2, APOE3 and APOE4. Everyone has two copies of the gene and the combination will determine the APOE genotype - e2/e2, e2/e3, e2/e4, e3/e3, e3/e4, or e4/e4. 

APOE2 has been known to be the rarest form of APOE. It is also said that carrying even one copy of APOE2 reduces risk of Alzheimer's by 40%.  

APOE3 is the most common allele and does not influence the Alzheimer's risk.

APOE4 increases the risk of Alzheimer's disease. Having only one copy of e4 increases the risk by 2-3 times; while having two e4 alleles increases risk by 12 times.

The APOE alleles are defined by two genetic variants in the gene, rs429358 and rs7412. Chromosome number and position of both rs429358 and rs7412 can be obtained from https://www.ncbi.nlm.nih.gov/snp/rs429358#variant_details and https://www.ncbi.nlm.nih.gov/snp/rs7412#variant_details respectively: 

rs429358: Primary locus 
[GRCh37]chr19:45411941

rs7412: Primary locus 
[GRCh37]chr19:45412079

The above information can be grepped out from our VCF file. 

`grep 45411941 sample.freebayes.vcf` 
`grep 45412079 sample.freebayes.vcf`

The obtained result can be interpreted as one of these conditions:

A) APOE-e3(rs7412-C;rs429358-T) =  It is considered the neutral APOE genotype
B) APOE-e4(rs7412-C;rs429358-C) =  Highest risk of developing Alzheimer's disease 

The person whose genome was analyzed has the APOE genotype APOE-e3/e3 (rs429358 T;T ; rs7412 C;C) and hence does not genetically have a high risk of developing Alzheimer's disease.











