---
title: User:Davidw/cookbook
permalink: wiki/User:Davidw/cookbook
layout: wiki
---

[PLAN](http://bioinfo.noble.org/plan) is a free online service for BLAST
based sequence annotation provided by the Noble institute. At present
the people that run PLAN limit users querie's to 1000 records. This
recipe shows how to split a fasta file containing thousands of records
into smaller files with filenames that include the range of records
included in the file.

``` python
from Bio import SeqIO

in_name = "huge_run.fasta"
records = list(SeqIO.parse(open(in_name, 'r'), "fasta"))
nfiles = len(records)/1000

for filenumber in range(nfiles):
    split_min = filenumber*1000 #(python lists start at 0)
    split_max = ((filenumber+1)*1000)-1
    seqs = records[split_min:split_max]
    out_name = ("_%s-%s." % (split_min+1, split_max+1)).join(in_name.split("."))
    out_handle = open(out_name, 'w')
    SeqIO.write(seqs, out_handle, "fasta")
    out_handle.close()

# That will make a bunch of files with 1000 records, but you will almost always 
# have some "dregs" to mop up

if len(records) / 1000 != 0: #checking for leftovers
    lsplit_min = (nfiles+1)*1000
    last_name = ("_%s-%s." % (lsplit_min, len(records)-1)).join(in_name.split("."))
    last_handle = open(last_name, 'w')
    lseqs = records[lsplit_min:-1]
    SeqIO.write(lseqs, last_handle, "fasta")
    last_handle.close()
else:
    continue
```