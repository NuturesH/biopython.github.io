---
title: Concatenate nexus
permalink: wiki/Concatenate_nexus
layout: wiki
tags:
 - Cookbook
---

The Problem
-----------

It's become increasingly common to make species-level phylogenetic
inferences from multiple genes. Demographic processes can cause single
gene trees to diverge from species trees so support from multiple genes
for the same tree topology is considered stronger evidence than single
gene inferences (of course, we still need to test that each gene is
telling the same story).

This is usually handled by aligning each gene separately then creating a
single "supermatrix" from the individual files. i.e. You need a single
alignment containing one row for each taxon where the rows are the
concatenated pre-aligned sequences. In NEXUS files (used by PAUP\* and
Mr Bayes) multiple alignments can be explicitly represented as
different' character partitions' within a data matrix that contains one
long sequence for each taxon. In this way you can create a supermatrix
but still apply different transition models to each gene within in it or
run PAUP\*'s Partition Homogeneity Test to check for significant
difference in the rate/topology of each gene tree.

The Bio.Nexus module makes this relatively straight forward.

The Solution
------------

Say we have nexus file for three genes; btCOI.nex, btCOII,nex and
btITS.nex that we want to combine.

``` python
from Bio.Nexus import Nexus
# the combine function takes a list [(name, nexus instance)...], if we provide the
# file handles in a list we can use a list comprehension to make one easily
handles = [open('btCOI.nex', 'r'), open('btCOII.nex', 'r'), open('btITS.nex', 'r')]   
nexi =  [(handle.name, Nexus.Nexus(handle)) for handle in file_list]

combined = Nexus.combine(nexi)
combined.write_nexus_data(filename='btCOMBINED.nex')
```

That was easy! Lets look at our combined file

    #NEXUS
    begin data;
        dimensions ntax=4 nchar=32;
        format datatype=dna missing=? gap=-;
    matrix
    bt1 ATGCGACTAGCAATGCGACTAGCAATGC--GA
    bt2 ATGCGCCTAGCAATGCGCCTAGCAATGCGGAC
    bt3 GTCCGACTAGCAGTCCGACTAGCAGTCCGAC-
    bt4 ????????????????????????GTCCGAC-
    ;
    end;

    begin sets;
    charset btCOII.nex = 13-24;
    charset btCOI.nex = 1-12;
    charset btITS.nex = 25-32;
    charpartition combined = btCOI.nex: 1-12, btCOII.nex: 13-24, btCOII.nex: 25-32;
    end;

Ahh, it was too easy. The ITS file had a taxon that wasn't in other
files so it was added with lots of missing data. Sometimes this might be
the result you want but having a few taxa like this is also a very good
way to make a Partition Homogeneity Test run for a week. Lets write a
function that tests that the same taxa are represented in a set of nexus
instances and provides a useful error message if not (ie, what to delete
from your nexus files if you want them to combine nicely)

``` python
def check_taxa(matrices):  
  '''Checks that nexus instances in a list [(name, instance)...] have 
  the same taxa, provides useful error if not and returns None if
  everything matches
  '''
  first_taxa = matrices[0][1].taxlabels
  for name, matrix in matrices[1:]:
    first_only = [t for t in first_taxa if t not in matrix.taxlabels]
    new_only = [t for t in matrix.taxlabels if t not in first_taxa]
    if first_only:
      missing = ', '.join([t for t in first_only])
      msg = '%s taxa %s not in martix %s' % (nexi[0][0], missing, name)
      raise Nexus.NexusError(msg)
    elif new_only:
      missing = ', '.join([t for t in new_only])
      msg = '%s taxa %s not in all matrices'  % (name, missing)
      raise Nexus.NexusError(msg)
  return None # will only get here if it hasn't thrown an exception


def concat(file_list, same_taxa=True):
  ''' Combine multiple nexus data matrices in one partitioned file.
  By default this will only work if the same taxa are present in each file
  use  same_taxa=False if you are not concerned by this
  '''    
  nexi =  [(handle.name, Nexus.Nexus(handle)) for handle in file_list]
  if same_taxa:
    if not check_taxa(nexi): 
      return Nexus.combine(nexi)
  else:
    return Nexus.combine(nexi)
```

And now, using our new functions:


    >>> handles = [open('btCOI.nex', 'r'), open('btCOII.nex', 'r'), open('btITS.nex', 'r')]
    # If we combine them all we should get an error and the taxon/taxa that caused it
    >>> concat(handles)
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
      File "<stdin>", line 5, in concat
      File "<stdin>", line 16, in check_taxa
    Bio.Nexus.Nexus.NexusError: btITS.nex taxa bt4 not in all matrices

    # But if we use just the first two, which do have matching taxa, it should be fine
    >>> concat(handles[:2]).taxlabels
    ['bt1', 'bt2', 'bt3']

    # Ok, can we still munge them together if we want to?
    >>> concat(handle, same_taxa=False).taxlabels
    ['bt1', 'bt2', 'bt3', 'bt4']

Discussion
----------

The details of the Nexus class are provided in the [API
Domcumentation](http://www.biopython.org/DIST/docs/api/Bio.Nexus.Nexus-pysrc.html)