---
title: BioSQL
permalink: wiki/BioSQL
layout: wiki
---

[BioSQL](http://www.biosql.org/wiki/Main_Page) is a joint effort between
the [OBF](http://open-bio.org/) projects (BioPerl, BioJava etc) to
support a shared database schema for storing sequence data. In theory,
you could load a GenBank file into the database with BioPerl, then
extract this from the database as a [SeqRecord](SeqRecord "wikilink")
with featues using Biopython - and get more or less the same thing as if
you had loaded the GenBank file directly using
[SeqIO](SeqIO "wikilink").

Existing documentation for the Biopython interfaces to BioSQL cover
installing Python database adaptors and basic usage of BioSQL:

[HTML](http://biopython.org/DIST/docs/biosql/python_biosql_basic.html) |
[PDF](http://biopython.org/DIST/docs/biosql/python_biosql_basic.pdf)

I hope to use this wiki page to update the above documentation in
future.

### Installation

This is fairly complicated - partly because there are some many options.
For example, you can use a range of different SQL database packages
(we'll focus on MySQL), you can have the database on your own computer
(the assumption here) or on a separate server. Also the details will
vary depending on your operating system.

Installing Required Software
----------------------------

You will need to install some database software plus the associated
python library so that Biopython can "talk" to the database. In this
example we'll talk about the most common choice, MySQL. How you do this
will also depend on your operating system, for example on a Debian or
Ubuntu Linux machine try this:

`sudo apt-get install mysql-common mysql-server mysql-admin python-mysqldb`

It will also be important to have perl (to run some of the setup
scripts) and cvs (to get some BioSQL files). Again, on a Debian or
Ubuntu Linux machine try this:

`sudo apt-get install perl`

You may find perl is already installed.

Setting up the BioSQL Schema
----------------------------

One the software is installed, your next task is to setup a database and
import the BioSQL scheme (i.e. setup the relevant tables within the
database). If you have CVS installed, then on Linux you can download the
latest schema like this (password is 'cvs').

`cd ~`  
`mkdir repository`  
`cd repository`  
`cvs -d :pserver:cvs@code.open-bio.org:/home/repository/biopython checkout biosql`  
`cvs -d :pserver:cvs@code.open-bio.org:/home/repository/biosql checkout biosql-schema`  
`cd biosql-schema`

If you don't want to use CVS, then download the files via the [View CVS
web
interface](http://cvs.open-bio.org/cgi-bin/viewcvs/viewcvs.cgi/?cvsroot=biosql).
Click the Download tarball link to get a tar.gz file containing the
current CVS, and then unzip that.

And then ...

NCBI Taxonomy
-------------

Before you start trying to load sequences into the database, it is a
good idea to load the NCBI taxonomy database using the
scripts/load\_ncbi\_taxonomy.pl script in the BioSQL package. *To do...*