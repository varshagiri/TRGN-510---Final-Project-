# TRGN-510---Final-Project-



###### Analysis of a PGP person using germline variant calling

The aim of this project is to analyze the data of a PGP person using gremlin variant calling. I will first obtain appropriate data from the Harvard PGP database. Next, the fastq data is going to be aligned with a reference genome using BWA software to reproduce the germline variance between the two sequences. The identified result is saved in SAM format and the same is converted into BAM for easier manipulation. Only the anomalies between two sequences are further filtered out using SNPeff and the output will be in VCF. Finally, a front end view will be created using R programming.  


#### 1) Downloading appropriate data
- Find the appropriate PGP person with fasta/fastq file and download it to the local directory 

1. Use links https://my.pgp-hms.org/user_file/download/2810 and https://my.pgp-hms.org/user_file/download/2811 to download read 1 and read 2 respectively. 

Now the data should have downloaded into the local working directory.

(Data is obtained from The Harvard Personal Genome Project. Reference link: https://pgp.med.harvard.edu) 

- Transfer the data from local directory to the server using SFTP
1. Log in to ssh and enter password to test access
` 
   ssh varshagi@itg.usc.edu
`

2. Log out once it works

`exit
`

3. Open an SFTP using the same server connection 

`
   sftp varshagi@itg.usc.edu
`

4. Once connection is established, get the path of the remote directory

`
   pwd
`

5. Check for contents of the directory

`
   ls
`

6. Create a directory to transfer the data into

`
   cd Alz
`

7. Get path of local working directory

`
   lpwd
`

8. Check contents of local working directory and make sure where the downloaded files are 

`
   lls
`

9. Download file to the working directory 

`
   put localFile (this is to be replaced by the filename) 
`

10. Verify transfer of file 

`ssh varshagi@itg.usc.edu`

`ls`

`cd Alz`

`ls -sh` (Checks for filesize of the two fastq files)

Result: '/home/varsha/Alz' should contain the fastq files

11. Exit the server

` exit`

#### 2) Installing BWA and Samtools

#### BWA Installation instructions

` mkdir ~/bin/bwa_build 

  cd ~/bin/bwa_build 
  
  git clone https://github.com/lh3/bwa.git
  
  cd bwa
  
  make`
  
  To verify its function:

` ~/bin/bwa_build/bwa/bwa `

Result: `Program: bwa (alignment via Burrows-Wheeler transformation)
Version: 0.7.17-r1188
Contact: Heng Li <lh3@sanger.ac.uk>

Usage: bwa <command> [options]

Command: index index sequences in the FASTA format
 mem BWA-MEM algorithm
 fastmap identify super-maximal exact matches
 pemerge merge overlapping paired ends (EXPERIMENTAL)
 aln gapped/ungapped alignment
 samse generate alignment (single ended)
 sampe generate alignment (paired ended)
 bwasw BWA-SW for long queries

shm manage indices in shared memory
 fa2pac convert FASTA to PAC format
 pac2bwt generate BWT from PAC
 pac2bwtgen alternative algorithm for generating BWT
 bwtupdate update .bwt to the new format
 bwt2sa generate SA from BWT and Occ

Note: To use BWA, you need to first index the genome with `bwa index'.
 There are three alignment algorithms in BWA: `mem', `bwasw', and
 `aln/samse/sampe'. If you are not sure which to use, try `bwa mem'
 first. Please `man ./bwa.1' for the manual. `
 
 #### Samtools Installion instructions:
 
 ` mkdir ~/bin/samtools_build
 
   cd ~/bin/samtools_build `
   
   Download data
   
   ` wget https://github.com/samtools/samtools/releases/download/1.7/samtools-1.7.tar.bz2 ` 
   
   Unzip and untar data respectively 
   
   ` bunzip2 samtools-1.7.tar.bz2 `
   
   `  tar -xvf samtools-1.7.tar `
   
   Navigate to working directory
   
   ` cd samtools-1.7 `
   
   Configuring the script
   
   `./configure`
   
   Incase of error in compilation
   
   `./configure --without-curses`
   
   To make the compiled program in the local directory
   
   `make DESTDIR=~/bin/samtools_build/samtools-1.7` 
   
   Verify compilation status in the working directory
   
   `./samtools`
   
   Verify BWA and Samtools exist in the server 
   
   ` cd ~
   samtools` checkes for samtools
   `bwa ` checks for bwa 
   

#### 3) Downloading refernce genome data 

` mkdir ~/bin/refgen
  cd refgen
  wget -r ftp://gsapubftp-anonymous@ftp.broadinstitute.org/bundle/b37/human_g1k_v37.fasta.fai.gz
  wget ftp://gsapubftp-anonymous@ftp.broadinstitute.org/bundle/b37/human_g1k_v37.fasta.gz`
  
  Build an index to make data search faster 
  
  ` mkdir ~/scripts
    cd ~/scripts 
    vim ~/scripts/build_bwa_index.sh 
    `
  Contents within build_bwa_index.sh
  
  ` #!/bin/bash
DATADIR="/refgen"
mkdir -p ${DATADIR}
######################################### #  
#Running Analysis 
######################################### 
echo Running Analysis echo Path is `pwd`
cd ${DATADIR}
bwa index ${DATADIR}/human_g1k_v37.fasta.gz
`

Change permission

` chmod 755~/scripts/build_bwa_index.sh `

Run in background 

` nohup ~/scripts/build_bwa_index.sh >& ~/scripts/build_bwa_index.out & `

#### 4) Alignment from Fastqs to BAMs

Creating an alignemnt script 

` vim ~/scripts/build_bwa_align.sh `

Script within build_bwa_align:

` #!/bin/bash
`






