---
title: GSOC
permalink: wiki/GSOC
layout: wiki
---

Introduction
------------

The Open Bioinformatics foundation successfully [applied to participate
in the Google Summer of
Code](http://www.open-bio.org/wiki/Google_Summer_of_Code).

Please read the [GSoC page at the Open Bioinformatics
Foundation](http://www.open-bio.org/wiki/Google_Summer_of_Code) and the
[main Google Summer of Code page](http://code.google.com/soc) for more
details about the program.

Mentor List
-----------

Usually, each BioPython proposal has one or more mentors assigned to it.
Nevertheless, we encourage potential students to contact the mailing
list with their own ideas for proposals. There is therefore not a set
list of 'available' mentors, since it highly depends on which projects
are proposed every year.

Past mentors include:

-   [James Casbon](http://casbon.me/)
-   [Brad Chapman](https://github.com/chapmanb)
-   [Peter Cock](http://www.hutton.ac.uk/staff/peter-cock)
-   [Thomas Hamelryck](http://wiki.binf.ku.dk/User:Thomas_Hamelryck)
-   [Reece Hart](http://www.linkedin.com/in/reece)
-   [João Rodrigues](http://nmr.chem.uu.nl/~joao)
-   [Eric Talevich](http://etal.myweb.uga.edu/)

Proposals
---------

### 2013

The BioPython proposals for 2013 will be published here once discussed.
We encourage potential students to join the mailing lists and actively
participate in these discussions, either by submitting their own ideas
or contributing to improving existing ones.

Past Proposals
--------------

### 2012

#### SearchIO

Rationale  
Biopython has general APIs for parsing and writing assorted sequence
file formats (SeqIO), multiple sequence alignments (AlignIO),
phylogenetic trees (Phylo) and motifs (Bio.Motif). An obvious omission
is something equivalent to BioPerl's SearchIO. The goal of this proposal
is to develop an easy-to-use Python interface in the same style as
SeqIO, AlignIO, etc but for pairwise search results. This would aim to
cover EMBOSS muscle & water, BLAST XML, BLAST tabular, HMMER, Bill
Pearson's FASTA alignments, and so on.

Approach  
Much of the low level parsing code to handle these file formats already
exists in Biopython, and much as the SeqIO and AlignIO modules are
linked and share code, similar links apply to the proposed SearchIO
module when using pairwise alignment file formats. However, SearchIO
will also support pairwise search results where the pairwise sequence
alignment itself is not available (e.g. the default BLAST
tabular output). A crucial aspect of this work will be to design a
pairwise-search-result object heirachy that reflects this, probably with
a subclass inheriting from both the pairwise-search-result and the
existing MultipleSequenceAlignment object. Beyond the initial challenge
of an iterator based parsing and writing framework, random access akin
to the Bio.SeqIO.index and index\_db functionality would be most
desirable for working with large datasets.

Challenges  
The project will cover a range of important file formats from major
Bioinformatics tools, thus will require familiarity with running these
tools, and understanding their output and its meaning. Inter-converting
file formats is part of this.

Difficulty and needed skills  
Medium/Hard depending on how many objectives are attempted. The student
needs to be fluent in Python and have knowledge of the
BioPython codebase. Experience with all of the command line tools listed
would be clear advantages, as would first hand experience using
BioPerl's SearchIO. You will also need to know or learn the git version
control system.

Mentors  
Peter Cock

#### Representation and manipulation of genomic variants

Rationale  
Computational analysis of genomic variation requires the ability to
reliably communicate and manipulate variants. The goal of this project
is to provide facilities within BioPython to represent sequence
variation objects, convert them to and from common human and file
representations, and provide common manipulations on them.

Approach & Goals  

-   Object representation
    -   identify variation types to be represented (SNV, CNV, repeats,
        inversions, etc)
    -   develop internal machine representation for variation types
    -   ensure coverage of essential standards, including HGVS, GFF, VCF
-   External representations
    -   write parser and generators between objects and external string
        and file formats
-   Manipulations
    -   canonicalize variations with more than one valid representation
        (e.g., ins versus dup and left shifting repeats).
    -   develop coordinate mapping between genomic, cDNA, and protein
        sequences (HGVS)
-   Other
    -   release code to appropriate community efforts and write short
        manuscript
    -   implement web service for HGVS conversion

Difficulty and needed skills  
Easy-to-Medium depending on how many objectives are attempted. The
student will need have skills in most or all of: basic molecular biology
(genomes, transcripts, proteins), genomic variation, Python, BioPython,
Perl, BioPerl, NCBI Eutilities and/or Ensembl API. Experience with
computer grammars is highly desirable. You will also need to know or
learn the git version control system.

Mentors  
Reece Hart (Locus Development, San Francisco); Brad Chapman; James
Casbon

### 2011

### 2010

### 2009