# Percent GC Calculator 
A shell script writtin in python for inputting a single or multisequence fasta file to report the number of G's C's and total lengh along with the percent GC content of each sequence. 

Synopsis
Script for calculating the percent GC in a fasta file. 
The input file should be in .fa ending, the output file will be <inputfile>.fa.txt

Example input-
>gi|226451773|gb|FJ846591.1
CATTATAGACTGCGTGGTCCGTATTCCCAAGGAGCAGGGAGTTCTGTCCTTCTGGCGCGGTAACCTGGCCAATGTCATCAGATACTTCCCCACCCAGGCTCTTAACTTCGCCTTCAAAGATAAATACAAGCAGATCTTCCTAGGTGGTGTGGACAAGAGGACCCAGTTTTGGCGCTACTTTGCAGGGAATCTGGCATCAG
GTGGTGCCGCAGGGGCCACATCCCTGTGTTTTGTGTACCCTCTTGATTTTGCCCGTACCCGTCTAGCAGCTGATGTGGGTAAAGCTGGAGCTGAAAGGGAATTCCGAGGCCTCGGTGACTGCCTGGTTAAGATCTACAAATCTGATGGGATTAAGGGCCTGTACCAAGGCTTTAACGTGTCTGTGCAGGGTATTATCATC
TACCGAGCCGCCTACTTCGGTATCTATGACACTGCAAAGGGTAAGTTTGCTGTGGGCTTTAAAGTTGTGTTCTTAGGAGACAATTTAAAAGAGCGTTGTACCAACCTAACATTCCAAGAGCTAGAGAGTTTTTTTAATTGCTGAAGGAAGCCAAGATCATCCAGTGCGACCCTCATGCACAGATGACATGTTTAGGGGAT
GTGGGGAAAGGAAGTCAGTAAAACTCTACTTTTTGGTAAAAGCATCTCTTTCCTATTCCCAGGAATGCTTCCGGATCCCAAAAACACTCACATCGTCATCAGCTGGATGATCGCACAGACTGTCACTGCTGTTGCTGGGTTGACTTCCTATCCATTTGA

The program  
```#!/usr/bin/python
#  fastagc.py
#
from sys import argv
# Get args passed
if len(argv) > 1 :
    script, inname = argv
    outname = inname + '.txt'
else :
    print ("No input file specified")
    exit(2)
# Define fasta file parser
def fastaread():
    global record
    name = ''
    seq = ''
    if len(record) < 1 :
        record = input_file.readline()
    if record.startswith(">"):
        name = record.strip()[:-1]
        record = input_file.readline()
    while len(record) > 1 and not record.startswith(">"):
        seq += record.strip()
        record = input_file.readline()
    return name, seq;
#
# Print Start
print ("Analyzing File: %s" % inname)
gene_count = 0
bad_gene_count = 0
record = ''
gene = ''
sequence = ''
# Process file
input_file = open(inname, 'r')
output_file = open(outname,'w')
output_file.write('Gene\tC\tG\tTotal\tCG%\n')
gene, sequence = fastaread()
while len(gene) > 1 :
    gene_count += 1
    print("..processing %s" % gene)
    C_count = sequence.count('C')
    G_count = sequence.count('G')
    total_count = len(sequence)
    if total_count > 0 :
        percentage = float(C_count + G_count) / total_count*100.0
        output_line = '%s\t%i\t%i\t%i\t%f\n' % \
        (gene, C_count, G_count, total_count, percentage)
        output_file.write(output_line)
    else :
        print ("Skipping invalid format ID record")
        print ("gene:",gene)
        bad_gene_count += 1
    gene, sequence = fastaread()
# Done, close files
output_file.close()
input_file.close()
# Print summary
print ("Output File: %s, %i genes found, %i genes skipped" % (outname,gene_count,bad_gene_count))```

make the script exacutable
chmod u+x [script name]

Lets go through this stepwise 

This command checks the input file specified and if it is valid 
```
from sys import argv
# Get args passed
if len(argv) > 1 :
    script, inname = argv
    outname = inname + '.txt'
else :
    print ("No input file specified")
    exit(2)```

This segment defines the difference between the sequence and the header that provides the ID

```# Define fasta file parser
def fastaread():
    global record
    name = ''
    seq = ''
    if len(record) < 1 :
        record = input_file.readline()
    if record.startswith(">"):
        name = record.strip()[:-1]
        record = input_file.readline()
    while len(record) > 1 and not record.startswith(">"):
        seq += record.strip()
        record = input_file.readline()
    return name, seq;```
    
This segment  Prints to the terminal when each sequence starts getting analysed
```# Print Start
print ("Analyzing File: %s" % inname)
gene_count = 0
bad_gene_count = 0
record = ''
gene = ''
sequence = '' ``` 

Exapmple 

output example-
Analyzing File: sample.fa
..processing >gi|226451773|gb|FJ846591.
..processing >gi|226451771|gb|FJ846590.
..processing >gi|226451769|gb|FJ846589.
..processing >gi|226451767|gb|FJ846588.
..processing >gi|226451765|gb|FJ846587.
..processing >gi|226451763|gb|FJ846586.
..processing >gi|226451761|gb|FJ846585.
..processing >gi|226451759|gb|FJ846584.
..processing >gi|226451757|gb|FJ846583.
..processing >gi|226451755|gb|FJ846582.
..processing >gi|226451753|gb|FJ846581.
..processing >gi|226451751|gb|FJ846580.
..processing >gi|226451749|gb|FJ846579.
..processing >gi|226451747|gb|FJ846578.
..processing >gi|226451745|gb|FJ846577.
..processing >gi|226451743|gb|FJ846576.
..processing >gi|226451741|gb|FJ846575.
..processing >gi|226451739|gb|FJ846574.
..processing >gi|226451737|gb|FJ846573.
..processing >gi|226451735|gb|FJ846572.
Output File: sample.fa.txt, 20 genes found, 0 genes skipped 


This segments is the core of the program , it calculates the amount of C's G's and total sequcne length

```# Process file
input_file = open(inname, 'r')
output_file = open(outname,'w')
output_file.write('Gene\tC\tG\tTotal\tCG%\n')
gene, sequence = fastaread()
while len(gene) > 1 :
    gene_count += 1
    print("..processing %s" % gene)
    C_count = sequence.count('C')
    G_count = sequence.count('G')
    total_count = len(sequence)
    if total_count > 0 :
        percentage = float(C_count + G_count) / total_count*100.0
        output_line = '%s\t%i\t%i\t%i\t%f\n' % \
        (gene, C_count, G_count, total_count, percentage)
        output_file.write(output_line)
    else :
        print ("Skipping invalid format ID record")
        print ("gene:",gene)
        bad_gene_count += 1
    gene, sequence = fastaread() ``` 
    
This small portion closes the files that were open for analysis 
```# Done, close files
output_file.close()
input_file.close() ```
Printing out the outputs into the output file 
```# Print summary
print ("Output File: %s, %i genes found, %i genes skipped" % (outname,gene_count,bad_gene_count)) ```

An example of this output file looks like 

Gene                              C       G     Total   CG%
>gi|226451773|gb|FJ846591.      175     190     759     48.089592
>gi|226451771|gb|FJ846590.      175     190     759     48.089592
>gi|226451769|gb|FJ846589.      175     190     759     48.089592
>gi|226451767|gb|FJ846588.      174     190     759     47.957839
>gi|226451765|gb|FJ846587.      175     190     759     48.089592
>gi|226451763|gb|FJ846586.      175     190     759     48.089592
>gi|226451761|gb|FJ846585.      175     190     759     48.089592
>gi|226451759|gb|FJ846584.      174     190     759     47.957839
>gi|226451757|gb|FJ846583.      175     190     759     48.089592
>gi|226451755|gb|FJ846582.      175     190     759     48.089592
>gi|226451753|gb|FJ846581.      175     190     759     48.089592
>gi|226451751|gb|FJ846580.      175     190     759     48.089592
>gi|226451749|gb|FJ846579.      175     190     759     48.089592
>gi|226451747|gb|FJ846578.      175     190     759     48.089592
>gi|226451745|gb|FJ846577.      174     190     759     47.957839
>gi|226451743|gb|FJ846576.      175     190     759     48.089592
>gi|226451741|gb|FJ846575.      43      35      165     47.272727
>gi|226451739|gb|FJ846574.      45      33      165     47.272727
>gi|226451737|gb|FJ846573.      174     190     759     47.957839
>gi|226451735|gb|FJ846572.      44      35      165     47.878788


