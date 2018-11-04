# Homework #3
Cynthia I. Rodriguez
## Exercise 1
## Summarize a genome assembly
>Go to the most current download genomes section and download the gzipped fasta file for all chromosomes.
### ANSWER:
``` 
wget “ftp://ftp.flybase.net/genomes/Drosophila_melanogaster/current/fasta/dmel-all-chromosome-r6.24.fasta.gz”
```
### File integrity:
> Verify the file integrity of the gzipped fasta file using a checksum
```
#Download md5sum.txt:
wget “ftp://ftp.flybase.net/genomes/Drosophila_melanogaster/current/fasta/md5sum.txt”
```
```
#Making and checking your own md5sum file against the downloaded md5sum file
md5sum *.gz > md5check.txt
```
```
#checking the files against each other:
md5sum -c md5sum.txt
```
Result:
```
dmel-all-chromosome-r6.24.fasta.gz: OK
```

OR compare the number for the dmel-all-chromosome-r6.24.fasta.gz file by opening both files:

 Result:
 ```
 less md5check.txt
71a25289ae2e630d5247856dc2a67ab1  dmel-all-chromosome-r6.24.fasta.gz
```
```
less md5sum.txt
71a25289ae2e630d5247856dc2a67ab1  dmel-all-chromosome-r6.24.fasta.gz
```
### Calculate the following for the whole genome:
>Total number of nucleotides
Total number of Ns
Total number of sequences

### ANSWER:
```
#Use module jje/kent the faSize tool
module load jje/jjeutils jje/kent
faSize dmel-all-chromosome-r6.24.fasta.gz >faSizeResults
```
```
#To view the file with the information saved from faSize tool on dmel-all-chromosome-r6.24.fasta.gz
less faSizeResults
```
Results:
```
143726002 bases (1152978 N's 142573024 real 142573024 upper 0 lower) in 1870 sequences in 1 files
Total size: mean 76858.8 sd 1382100.2 min 544 (211000022279089) max 32079331 (3R) median 1577
N count: mean 616.6 sd 6960.7
U count: mean 76242.3 sd 1379508.4
L count: mean 0.0 sd 0.0
%0.00 masked total, %0.00 masked real
```
OR To count the number of sequences alone:
```
#counting option in grep: adding the -c This will tell us how many sequences we have in the file since the sequences start by the character ">
$  zgrep -c '^>' dmel-all-chromosome-r6.24.fasta.gz
```
Result:
```
1870
```
##### There are 143726002 nucleotides, 1152978 Ns, 1870 of sequences in the whole genome.
## Exercise 2
## Summarize an annotation file
>Go to the most current download genomes section at flybase.org and download the gzipped gtf annotation file for D. melanogaster.
### ANSWER:
```
#Download it:
wget "ftp://ftp.flybase.net/genomes/Drosophila_melanogaster/current/gtf/dmel-all-r6.24.gtf.gz"
```
### File integrity
>Verify the file integrity of the gzipped gtf annotation using a checksum.

### ANSWER:
```
#Download the md5sum file
wget "ftp://ftp.flybase.net/genomes/Drosophila_melanogaster/current/gtf/md5sum.txt"
```
```
# Making and checking your own mdf file
md5sum *.gz > md5check.txt
```
```
#checking the files against each other:
md5sum -c md5sum.txt
```
RESULT:
```
dmel-all-r6.24.gtf.gz: OK
```
### Print a summary report with the following information:
> 1.Total number of features of each type, sorted from the most common to the least common
### ANSWER:
Find out what a gtf annotation file is: 
https://uswest.ensembl.org/info/website/upload/gff.html 
```
#to get the third column only containing the "features":
awk '{print $3}' dmel-all-r6.24.gtf >unsorted_featurelist.txt
```
```
#to sort it from most common to least common:
sort unsorted_featurelist.txt\
| uniq -c \
| sort -rn \
| nl > sorted_features.txt
```
Results:
```
     1   187315 exon
     5    30591 start_codon
     6    30533 stop_codon
     7    30507 mRNA
     8    17772 gene
     9     2961 ncRNA
    10      485 miRNA
    11      334 pseudogene
    12      312 tRNA
    13      299 snoRNA
    14      262 pre_miRNA
    15      115 rRNA
    16       32 snRNA
```
> 2.Total number of genes per chromosome arm (X, Y, 2L, 2R, 3L, 3R, 4)
### ANSWER:
```
#to get columns 1 and 3 containing the features and the chromosome labels out of the gtf file:
awk '{print $1, $3}' dmel-all-r6.24.gtf >unsorted_featurelist1_3.txt
```
```
#To get the genes per chromosome arm use a regular expression in grep -w and sort it numerically by gene name :
grep -w [XY234][LR]* unsorted_featurelist1_3.txt \
| grep -w gene \
| sort -n \
| uniq -c \
| nl
```
Results:
```
     1	   2676 X gene
     2	    113 Y gene
     3	   3501 2L gene
     4	   3628 2R gene
     5	   3464 3L gene
     6	   4202 3R gene
     7	    111 4 gene
```
##### There are 2676 genes in the X chromosome, 113 genes in the Y chromosome, 3501 genes in the 2L chromosome, 3628 genes in the 2R chromosome, 3464 genes in the 3L chromosome, 4202 genes in the 3R chromosome, and 111 genes in the 4 chromosome.
