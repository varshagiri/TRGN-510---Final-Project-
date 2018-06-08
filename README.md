# TRGN-510---Final-Project-
###### Analysis of a PGP person and building a pipeline using R to assess the variants in the gene of that person.

#### 1. Downloading appropriate data
- Find the appropriate PGP person with fasta/fastq file and download it to the local directory.

`
  download huA2629E
`

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

7. Get path of local directory

`
   lpwd
`

8. Check contents of local directory

`
   lls
`

9. Download file to the working directory 

`
   put localFile
`

10. Verify if file got transferred 

`ssh varshagi@itg.usc.edu`

`ls`

`cd Alz`

`ls`

Result: \home\varsha\Alz should contain the fastq file

11. Exit the server

` exit`






