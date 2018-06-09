# TRGN-510---Final-Project-
###### Analysis of a PGP person using germline variant calling

#### 1. Downloading appropriate data
- Find the appropriate PGP person with fasta/fastq file and download it to the local directory.

1. Use https://pgp.med.harvard.edu/data/
2. Navigate to https://my.pgp-hms.org/public_genetic_data and find participant huA2629E.
3. Dowload two fastq files (read1 and read2) of huA2629E.

Now the data should have downloaded into the local working directory.

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

`ls`

Result: '/home/varsha/Alz' should contain the fastq files

11. Exit the server

` exit`






