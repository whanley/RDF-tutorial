---
title: Small RDF Database tutorial extra ideas
authors: Will Hanley
date: 2016-03-08
---

## Step 3
* show that reasoner could do something with parent-child relationships
* make encoding of dates separate from the way they are recorded in the document itself. Can note that those who know XML may recognize this manner of specifying numbers alongside a literal according to a certain protocol.

## Step 4
* add sequence in list
* Break out x annotation

## Step 6
* Reconcile: link diseases to Dbpedia
* I don't think my profession/occupation idea is well formed.
* describe translation into other formats (XML?), which is a bit advantage. And can be moved from one triplestore to another. good Sustainability.


## Older version leftovers

A simple option is [Fuseki](https://jena.apache.org/documentation/serving_data/). Download it from [this page](https://jena.apache.org/download/index.cgi) (scroll down to the *Apache Jena Fuseki* heading, and download the `apache-jena-fuseki.2.4.0.zip` file. Unzip this file. Then, using the command line (in Terminal in Linux or Mac, or in Command Prompt in Windows), use `cd` (Linux/Mac) or `dir` (Windows) to navigate to the Fuseki folder you created. Then use the command `./fuseki-server` (Linux/Mac) or `./fuseki-server.bat` (Windows) to start the server. Open a web browser and type [`localhost:3030`](localhost:3030) into the address bar, and you are set to go.

We'll now need to upload our data file into Fuseki. Add a new dataset (using in-memory for this temporary experiment), give it a name that makes sense, then click upload data and attach our Turtle file. (When you construct a dataset yourself, you might encounter upload errors if you have made certain syntax errors in typing. Most often, it's a misplaced semicolon or period that is responsible. If you have an error, paste the Turtle code into this [tool](http://www.easyrdf.org/converter), which will locate it for you.)

Now we're set to interact with the data using the SPARQL query language, which as we've seen is the subject of [another *Programming Historian* tutorial](http://programminghistorian.org/lessons/graph-databases-and-SPARQL). Switch to the Fuseki query interface and run `SELECT * WHERE {?s ?p ?o}`, the standard SPARQL query that lists all of the information entered.
