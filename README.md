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

#### 3) Downloading refernce genome 




