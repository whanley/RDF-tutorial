---
title: Working with Ontologies
authors:
- Will Hanley
date: 2017-10-04
reviewers:
layout: lesson
---

# Overview


# 1 Conceptualizing your data
OpenRefine as a tool to visualize data's structure, experimentally.

Another thing you can do is reconcile your data with Wikidata or another source.

Use: VIAF, GeoNames, PerioDo, Wikidata, DBPedia


## Competency Questions
A good way to organize your data model is to come up with some competency questions. These are examples of the questions you would like your model to answer. For instance:

- how many people were married?
- what was the birthrate?
- what was the major occupation of Christians?

# 2 Working with a dataset

## How to install Virtuoso
There are various triplestores available. One of the most widely used is [Virtuoso Open-Source](http://virtuoso.openlinksw.com/dataspace/doc/dav/wiki/Main/). Getting Virtuoso up and running on your own computer can be a bit daunting, but there are lots of tips and shortcuts available. Remember, this is a *small* database that you're building. Most of the complexity in triplestores derives from large scale, data serving, and security demands. We're just trying to build a simple dataset for our own use, so we don't have to get everything right.

For **Linux**, follow Virtuoso's installation instructions for [your blend](http://virtuoso.openlinksw.com/dataspace/doc/dav/wiki/Main/VOSBuild#Building%20for%20Linux%20or%20other%20Unix-like%20OS)

You'll have to move the virtuoso.ini file from /etc/virtuoso into the /var/lib/virtuoso/db folder, then execute:

`$ sudo virtuoso-t -f virtuoso.ini`

For **Mac**, install homebrew (see instructions in the [Jekyll tutorial](http://programminghistorian.org/lessons/building-static-sites-with-jekyll-github-pages#command-line-tools-suite-a-idsection2-1a)), then execute:

`$ brew install virtuoso`

For **Windows**, download the [binary](http://virtuoso.openlinksw.com/dataspace/doc/dav/wiki/Main/VOSDownload#Pre-built%20binaries%20for%20Windows) and extract it to somewhere such as c:\virtuoso. Then execute from shell:

```
$ cd c:\virtuoso\database
$ virtuoso-t -f virtuoso.ini
```

# 3 Editing ontologies
Protege

Aim for between two and twelve subclasses or subproperties.
Don't go too far. Only provide as much detail as is necessary to do the work you need. This is not Borjes.

## Next Steps
- set up a sparql endpoint?
- better idea: use someone else's.

### Acknowledgements
I wrote this lesson while Elizabeth and J. Richardson Dilworth Fellow at the [Institute for Advanced Study](ias.edu). I learned about ontologies through work funded in part by the National Endowment for the Humanities' Office of Digital Humanities (grant HD-51269-11), by the German Academic Exchange Service (DAAD), and by Florida State University's Office of Sponsored Research. Thanks to Héctor Pérez Urbina and to Thomas Riechert and other members of the [Agile Knowledge Engineering and Semantic Web](aksw.org) group in Leipzig for their guidance.
