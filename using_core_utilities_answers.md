**Solutions for core utilities**

*sequence count*

1. `grep -c '>' sequences.fa`

2. `grep '>' sequences.fa > tempfile.txt`
`wc -l tempfile.txt`

*filter*

`grep '>' sequences.fa | head -n 5`

*one specific line*

`head -n 19 sequences.fa | tail -n 1`

*collecting sequences*

`grep '>' sequences.fa | head -n 3 > newfile.fa` and
`grep '>' sequences.fa | tail -n 3 >> newfile.fa`

*create and unpack tar archive*

1. `tar -cvf sequences.tar sequences.fa`
2. `tar -tvf sequences.tar`
3. `tar -xvf sequences.tar`

*compress and decompress a fasta file using gzip*
`gzip seqences.fa`

`gzip -d sequences.fa.gz` or `gunzip sequences.fa.gz`

