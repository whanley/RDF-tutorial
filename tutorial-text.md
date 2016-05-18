---
title: Making a Small RDF Database
authors:
- Will Hanley
date: 2016-03-08
reviewers:
layout: default
---

# Making a small RDF database

When I'm in the archives, I often come across serial records of some sort--lists, stacks of forms, handwritten and corrected indexes--that I wish to record. One way to do this is an word document: efficient, but not very powerful. It's much easier to count, alphabetize, filter, and otherwise manipulate this data if I record it in a spreadsheet (like Excel) or database (like Filemaker or Access). Often, however, it's not clear to me which fields I might need to record the data. Should I use First name/last name, or whole name? What if some birthdates are exact, others are just years, and some are just ages? Sometimes, too, the types of data shift from scribe to scribe or from year to year. All of this means that I have to change my database, and go back over records I've already read, or leave the series incomplete.

There is a solution: the small RDF database. Once you learn the grammar of this format, it's quite simple to record data in it. There is no need to remake your database as your records change; just add new the new information and carry on. The great advantage comes at the end, because there are powerful querying tools (see the [SPARQL tutorial](http://programminghistorian.org/lessons/graph-databases-and-SPARQL)) and it is easy to link your data to other data sets.

# An example

Suppose you were working with a register of American nationals kept at the US consulate in Alexandria, Egypt in the 1880s. This is a serial record that lists standardized information. It can help to answer questions like how many Americans were registered, what were their occupations, and so on. But the protocols of registration are inconsistent, and it's hard to know how to record the data in a standard format. Consider this page:

{% include figure.html src="../images/RDF-example.jpg" caption="Image of USNA" %}
Source: 350/11/19/3, vol. 24, RG 84, National Archives and Records Administration, College Park, Maryland.

You could transcribe it as a text file, more or less as follows:

```
		Mirzan Marie (left) (x)		|
							        |	
Edwige		daughter of 	Do.		|
Mary Rose	daughter of		Do.		|
Victor John	son of			Do.		|4th Sep'r 94. completed 21 years.			
Victor, Died today 1st November 1905 at 7 a.m.
aged 32 years from Tuberculosi Polmonite. European Hospital.

		Morpurgo D. Brutus (x)		|	Ironmonger died the 7 6 July 1906

Clarisa Hélène	wife 	of	Do.		|
Julia			mother (of Brutus) +| dead from cancer 9th July 1902. 4 a.m.
Libera Rachel	sister	"	"		|
Virginia		daughter	"	"	|
Réné			daughter	"	"	|
Angelino M.		son 	"	"		|
Ugo				son 	"	"		|
Hector			son 	"	"		|
Rodolph			son 	"	"		| dead 4th April 1894. aged 16 years from Diptheria.
Edgar			son		"	"		|
Oscar			son 	"	"		|
Gustave			son		"	"		|

	McFarlane Kenedy Wiley			| American Missionary

Anna Henderson	wife 	of	same	| Alexandria
Mary Evelyn		dauter	of	same	| 31 March 1896
Ralph Harvey	son		of	same	|

	Mogroby Jacob M				| Ombrella Merchant
								| reg. The 1.6.97 acc. To a Passport No 776 dated
Toba Mogroby	wife	of	do		 Vienna Austria 20th April 97.
Moses Mogroby	son 	of	do		 born 10 Feb. 98

X As recorded by Rev'd Dr. S C Ewing ex US Consular Agent on the 20th day of June 1888
```

You could even record the checks, circles, and crosses pencilled in on the left, even if their meaning is uncertain. The names would be searchable in a document; statistics and other data work would have to be undertaken manually or using Python scripts. But this serial data was _already a database_ when it was produced more than a century ago, and it makes sense to transcribe it as data. So, how do we do this?

# The tabular, relational, and hierarchical options
The first approach to consider is to make a table out of this information, like so:

{% include figure.html src="../images/spreadsheet-eg.png" caption="Tabular rendering of data" %}

It's not clear how many columns to create, or which columns are appropriate, even for this single page. As it is, there are two dozen columns, and some information is still hidden. Other pages of the register introduce still more categories. A complete table would have many dozens of columns.

A second option is to turn the information into a relational database, for instance using Filemaker or Access. Each of the four heads of household on this page would have his or her own subsidiary table, listing information about his or her household.

A third option would be to encode the page above using XML tags, marking information for machine extraction and counting using Python, for example. This would require the development of an elaborate and customized schema, however. The document would have to be carefully and consistently encoded in order to permit searches that would reveal, for example, the median number of children per household.

Ultimately, it is hard to see that any of these approaches would save much effort over mere transcription. And if the historian wanted to share this data with a colleague, a lot of explanation would be necessary.

In a case like this, a graph data approach might be useful.

# Making a small graph database

Turning this analog page into data is what computer scientists call "pre-processing." Let's go about this in a couple of steps, to show how the logic works. Once you have a bit of experience, you can skip the preliminary steps.

## Step 1: List
First, turn the linear text into a list. Every item on the list is a person that you want to represent in the database. We want to record the name and various other details about each of these persons.

1. Mirzan Marie (left) (x)
								
2. Edwige, daughter of Mirzan Marie.

3. Mary Rose, daughter of Mirzan Marie.

4. Victor John	son of Mirzan Marie. 4th Sep'r 94. completed 21 years.; Victor, Died today 1st November 1905 at 7 a.m. aged 32 years from Tuberculosi Polmonite. European Hospital.

5. Morpurgo D. Brutus (x),	Ironmonger died the ~~7~~6 July 1906

6. Clarisa Hélène, wife of Morpurgo D. Brutus

7. Julia	, mother of Morpurgo D. Brutus, dead from cancer 9th July 1902. 4 a.m.

8. Libera Rachel, sister of Morpurgo D. Brutus.

9. Virginia, daughter of Morpurgo D. Brutus.

10. Réné, daughter of Morpurgo D. Brutus.

11. Angelino M., son of Morpurgo D. Brutus.

12. Ugo, son of Morpurgo D. Brutus.

13. Hector, son of Morpurgo D. Brutus.

14. Rodolph, son of Morpurgo D. Brutus. dead 4th April 1894. aged 16 years from Diptheria.

15. Edgar, son of Morpurgo D. Brutus.

16. Oscar, son of Morpurgo D. Brutus.

17. Gustave, son of Morpurgo D. Brutus.

18. McFarlane Kenedy Wiley, American Missionary, Alexandria, 21 March 1896.

19. Anna Henderson, wife of McFarlane Kenedy Wiley.

20. Mary Evelyn, dauter [sic] of McFarlane Kenedy Wiley.

21. Ralph Harvey, son of McFarlane Kenedy Wiley,

22. Mogroby Jacob M, Ombrella Merchant, reg. the 1.6.97 acc. to a Passport No 776 dated Vienna Austria 20th April 97.

23. Toba Mogroby wife of Mogroby Jacob M.

24. Moses Mogroby son of Mogroby Jacob M, born 10 Feb. 98

25. Rev'd Dr. S C Ewing, ex US Consular Agent, who recorded information marked with an x on the 20th day of June 1888.

## Step 2: Structured list
Now, let's structure this list by describing the kind of details it includes about each person. We can invent relatively arbitrary categories for each type of detail, much as we would if we were putting the information into a table. One move that is novel here: for "ditto," we're substituting a number, which shows which person on the list the "ditto" refers to. Thus, we say that Edwige (#2 on the list) is the daughter of #1 (by which we mean Marie).

```
1. 	Name: Mirzan Marie
	Notes: "left", "x"

2. 	Name: Edwige
	daughter of: #1

3. 	Name: Mary Rose
	daughter of: #1

4. 	Name: Victor John
	son of: #1
	Note: 4 9 1894 "completed 21 years"
	Death date: 1 11 1905, 7 am
	Death age: 32
	Death place: European Hospital
	Death cause: Tuberculosi Polmonite

5.	Name: Morpurgo D. Brutus
	Note: x
	Occupation: Ironmonger
	Death date: 6 7 06

6. 	Name: Clarisa Hélène
	wife of: #5

7. 	Name: Julia
	Mother of: #5
	Death date: 9 7 1902, 4 am
	Death cause: cancer

8.  Name: Libera Rachel
	sister of: #5

9.	Name: Virginia
	daughter of: #5

10.	Name: Réné
	daughter of: #5

11. Name: Angelino M.
	son of: #5

12.	Name: Ugo
	son of: #5

13.	Name: Hector
	son of: #5

14.	Name: Rodolph
	son of: #5
	Death date: 4 4 1894
	Death age: 16
	Death cause: Diptheria

15.	Name: Edgar
	son of:	#5

16. Name: Oscar
	son of: #5

17.	Name: Hector
	son of: #5

18.	Name: McFarlane Kenedy Wiley
	Profession: American Missionary
	Note: Alexandria, 31 3 1896.

19.	Name: Anna Henderson
	wife of: #18

20.	Name: Mary Evelyn
	daughter of : #18 (misspelled "dauter")

21.	Name: Ralph Harvey
	son of: #18

22.	Name: Mogroby Jacob M
	Occupation: Ombrella Merchant
	Registration: on 1.6.97 acc. to a Passport No 776 dated Vienna Austria 20th April 97.

23.	Name: Toba Mogroby
	Wife of: #22

24. Name: Moses Mogroby
	son of:	#24
	Birth date: 10 2 1898

25.	Name: Rev'd Dr. S C Ewing
	Occupation: ex US Consular Agent
	Note: recorded information marked with an x on the 20th day of June 1888.
```

## Step 3: Translate into Machine-readable Language
Now, let's translate this structured list into the [Turtle](https://www.w3.org/TeamSubmission/turtle/) language, so that a machine can read it. This is a bit more tricky.

(It's easiest to use a simple text editor to do this work. For more on simple text editing, see this [plain text tutorial](http://programminghistorian.org/lessons/sustainable-authorship-in-plain-text-using-pandoc-and-markdown)).

This translation starts with a *declaration*, which is a list of abbreviations that explains the vocabulary we use. Just as we did in step 2, we're going to invent some vocabulary to describe the information we've found. We're going to assign that invented vocabulary to our own namespace, which we'll call "mydb". (One pleasure of RDF is that it's easy to invent terms as you go along, and fix or reconcile them later—I'll give an example of that below). RDF allows you to invent your own classification scheme as you go along. That classification scheme lives in your own "namespace," and your first job is to declare that namespace. Make up a name for your namespace and enter it at the top in the following format: `@prefix mydb: <http://myowndata.org#>`. You can use whatever letters you want for "mydb" and whatever address you want for "myowndata.org" (the http:// does not need to refer to a real website).

Although you can invent your own vocabulary (what computer scientists call "schema") for everything,  there are quite a few well-made schemas in common use. It is good practice to adopt their terms whenever you can. In this example, we will use three of the leading schemas: [rdf and rdfs](https://www.w3.org/TR/rdf-schema/) and [foaf](http://xmlns.com/foaf/spec/) (friend of a friend). Eventually, you might want to dig deeper into the features of these schemas. For now, let's see how they can be implemented into the list we produced on step 2.

[all of this can be annotated, with arrows if necessary… show particular terms, why I chose to do things]. 

Every new URI "paragraph" in step 3 is equivalent to a line on the list in step 2. (For the sake of clarity, we'll start the numbering at 1). In this database, each person is a unique object, known by its number (which looks like a web address, but which we can invent on our own). This URI is the first item in each paragraph. 

And we're saying a few other things:
* each of these URIs represents a Person in the foaf (Friend of a Friend) schema. Machines know quite a lot about `foaf:Persons`—like they can have parents, children, genders, and so on—we'll see later what we can do with this. 
* each of these URIs has a label, which is the same as the person's name. This is what's called a literal (a "string" in computer-science-speak), and it appears between quotation marks. Labels are really useful for humans trying to read databases. When we query an RDF database, the machine looks for URIs, but we can tell the machine to answer us by substituting labels that we can read instead.

[Maybe I should only have as many lines here as I have in previous step?]

```turtle
@prefix mydb: 	<http://mydb.org/schema#> .
@prefix foaf:	<http://xmlns.com/foaf/0.1> .
@prefix rdf:	<http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs:	<http://www.w3.org/2000/01/rdf-schema#> .

<mydb.org/id/1> a foaf:Person ;
	rdfs:label "Mirzan Marie" ;
	mydb:note "left" ;
	mydb:note "x" .

<mydb.org/id/2> a foaf:Person ;	
	rdfs:label "Edwige" ;
	mydb:daughterOf <mydb.org/id/1> .

<mydb.org/id/3> a foaf:Person ;	
	rdfs:label "Mary Rose" ;
	mydb:daughterOf <mydb.org/id/1> .

<mydb.org/id/4> a foaf:Person ;	
	rdfs:label "Victor John" ;
	mydb:sonOf <mydb.org/id/1> ;
	mydb:note "4 9 1894 completed 21 years" ;
	mydb:deathDate "1 11 1905, 7 am" ;
	mydb:deathAge 32 ;
	mydb:deathPlace	"European Hospital" ;
	mydb:deathCause	 "Tuberculosi Polmonite" .

<mydb.org/id/5> a foaf:Person ;
	rdfs:label "Morpurgo D. Brutus" ;
	mydb:note "x" ;
	mydb:occupation "Ironmonger" ;
	mydb:deathDate "6 7 06" .

<mydb.org/id/6> a foaf:Person ;	
	rdfs:label "Clarisa Hélène" ;
	mydb:wifeOf <mydb.org/id/5> .

<mydb.org/id/7> a foaf:Person ;	
	rdfs:label "Julia" ;
	mydb:motherOf <mydb.org/id/5> ;
	mydb:deathDate "9 7 1902, 4am" ;
	mydb:deathCause "cancer" .

<mydb.org/id/8> a foaf:Person ;	
	rdfs:label "Libera Rachel" ;
	mydb:sisterOf <mydb.org/id/5> .

<mydb.org/id/9> a foaf:Person ;	
	rdfs:label "Virginia" ;
	mydb:daughterOf <mydb.org/id/5> .

<mydb.org/id/10> a foaf:Person ;	
	rdfs:label "Réné" ;
	mydb:daughterOf <mydb.org/id/5> .

<mydb.org/id/11> a foaf:Person ;	
	rdfs:label "Angelino M." ;
	mydb:sonOf <mydb.org/id/5> .

<mydb.org/id/12> a foaf:Person ;	
	rdfs:label "Ugo" ;
	mydb:sonOf <mydb.org/id/5> .

<mydb.org/id/13> a foaf:Person ;	
	rdfs:label "Hector" ;
	mydb:sonOf <mydb.org/id/5> .

<mydb.org/id/14> a foaf:Person ;	
	rdfs:label "Rodolph" ;
	mydb:sonOf <mydb.org/id/5> .
	mydb:deathDate "4 4 1894" ;
	mydb:deathAge 16 ;
	mydb:deathCause "Diptheria" .

<mydb.org/id/15> a foaf:Person ;	
	rdfs:label "Edgar" ;
	mydb:sonOf <mydb.org/id/5> .

<mydb.org/id/16> a foaf:Person ;	
	rdfs:label "Oscar" ;
	mydb:sonOf <mydb.org/id/5> .

<mydb.org/id/17> a foaf:Person ;	
	rdfs:label "Hector" ;
	mydb:sonOf <mydb.org/id/5> .

<mydb.org/id/18> a foaf:Person ;
	rdfs:label "McFarlane Kenedy Wiley" ;
	mydb:profession "American Missionary" ;
	mydb:note "Alexandria 31 3 1896" .

<mydb.org/id/19> a foaf:Person ;
	rdfs:label "Anna Henderson" ;
	mydb:wifeOf <mydb.org/id/18> .

<mydb.org/id/20> a foaf:Person ;
	rdfs:label "Mary Evelyn" ;
	mydb:daughterOf <mydb.org/id/18> .

<mydb.org/id/21> a foaf:Person ;
	rdfs:label "Ralph Harvey"
	mydb:sonOf <mydb.org/id/18> .

<mydb.org/id/22> a foaf:Person ;
	rdfs:label "Mogroby Jacob M" ;
	mydb:occupation "Ombrella Merchant" ;
	mydb:registration	 "on 1.6.97 acc. to a Passport No 776 dated Vienna Austria 20th April 97".

<mydb.org/id/23> a foaf:Person ;
	rdfs:label "Toba Mogroby" .
	mydb:wifeOf <mydb.org/id/22> .

<mydb.org/id/24> a foaf:Person ;
	rdfs:label "Moses Mogroby" ;
	mydb:sonOf <mydb.org/id/24> ;
	mydb:birthDate "10 2 1898" .

<mydb.org/id/25> a foaf:Person ;
	rdfs:label "Rev'd Dr. S C Ewing" ;
	mydb:occupation "ex US Consular Agent" .
```

Done! This is far from elegant, but it will work. (It won't work for long, but every database needs fixing before long). As you can see, the terms are derived directly from the material itself. Anything that you put behind your own invented namespace (db: in this instance) is yours to invent.

The machine can do a lot with this:
* reasoner makes family tree. Knows that sons and daughters are male and female, etc.

## Step 4: Make some new objects
* make name parts into objects.
* Do something with the "left" annotation: make it an annot/1 of its own?
* Break out passport documents.
* Break out x annotation
* add sequence in list
* adding schema.org declaration for death and birth dates

```turtle
@prefix mydb: 	<http://mydb.org/schema#> .
@prefix foaf:	<http://xmlns.com/foaf/0.1> .
@prefix rdf:	<http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs:	<http://www.w3.org/2000/01/rdf-schema#> .
@prefix schema: <http://schema.org/#> .

<mydb.org/id/1> a foaf:Person ;
	rdfs:label "Mirzan Marie" ;
	foaf:givenName "Marie" ;
	foaf:familyName "Mirzan" ;
	mydb:note "left" ;
	mydb:note "x" .

<mydb.org/id/2> a foaf:Person ;	
	rdfs:label "Edwige" ;
	foaf:givenName "Edwige" ;
	mydb:daughterOf <mydb.org/id/1> .

<mydb.org/id/3> a foaf:Person ;	
	rdfs:label "Mary Rose" ;
	foaf:givenName "Mary Rose" ;
	mydb:daughterOf <mydb.org/id/1> .

<mydb.org/id/4> a foaf:Person ;	
	rdfs:label "Victor John" ;
	foaf:givenName "Victor John" ;
	mydb:sonOf <mydb.org/id/1> ;
	mydb:associatedDate	1894-09-04 ;
	mydb:note "completed 21 years" ;
	schema:deathDate 1905-11-01T07:00:00 ;
	mydb:deathAge 32 ;
	schema:deathPlace "European Hospital" ;
	mydb:deathCause	"Tuberculosi Polmonite" .

<mydb.org/id/5> a foaf:Person ;
	rdfs:label "Morpurgo D. Brutus" ;
	foaf:familyName "Morpurgo" ;
	foaf:givenName "D. Brutus" ;
	mydb:note "x" ;
	mydb:occupation "Ironmonger" ;
	schema:deathDate 1906-07-06 
	mydb:note "7 July 1906 day crossed out".

<mydb.org/id/6> a foaf:Person ;	
	rdfs:label "Clarisa Hélène" ;
	foaf:givenName "Clarisa Hélène" ;
	mydb:wifeOf <mydb.org/id/5> .

<mydb.org/id/7> a foaf:Person ;	
	rdfs:label "Julia" ;
	foaf:givenName "Julia" ;
	mydb:motherOf <mydb.org/id/5> ;
	schema:deathDate 1902-07-09T04:00:00 ;
	mydb:deathCause	"cancer" .

<mydb.org/id/8> a foaf:Person ;	
	rdfs:label "Libera Rachel" ;
	foaf:givenName "Libera Rachel" ;
	mydb:sisterOf <mydb.org/id/5> .

<mydb.org/id/9> a foaf:Person ;	
	rdfs:label "Virginia" ;
	foaf:givenName "Virginia" ;
	mydb:daughterOf <mydb.org/id/5> .

<mydb.org/id/10> a foaf:Person ;	
	rdfs:label "Réné" ;
	foaf:givenName "Réné" ;
	mydb:daughterOf <mydb.org/id/5> .

<mydb.org/id/11> a foaf:Person ;	
	rdfs:label "Angelino M." ;
	foaf:givenName "Angelino M." ;
	mydb:sonOf <mydb.org/id/5> .

<mydb.org/id/12> a foaf:Person ;	
	rdfs:label "Ugo" ;
	foaf:givenName "Ugo" ;
	mydb:sonOf <mydb.org/id/5> .

<mydb.org/id/13> a foaf:Person ;	
	rdfs:label "Hector" ;
	foaf:givenName "Hector" ;
	mydb:sonOf <mydb.org/id/5> .

<mydb.org/id/14> a foaf:Person ;	
	rdfs:label "Rodolph" ;
	foaf:givenName "Rodolph" ;
	mydb:sonOf <mydb.org/id/5> .
	schema:deathDate 1894-04-04 ;
	mydb:deathAge 16 ;
	mydb:deathCause	"Diptheria" .

<mydb.org/id/15> a foaf:Person ;	
	rdfs:label "Edgar" ;
	foaf:givenName "Edgar" ;
	mydb:sonOf <mydb.org/id/5> .

<mydb.org/id/16> a foaf:Person ;	
	rdfs:label "Oscar" ;
	foaf:givenName "Oscar" ;
	mydb:sonOf <mydb.org/id/5> .

<mydb.org/id/17> a foaf:Person ;	
	rdfs:label "Hector" ;
	foaf:givenName "Hector" ;
	mydb:sonOf <mydb.org/id/5> .

<mydb.org/id/18> a foaf:Person ;
	rdfs:label "McFarlane Kenedy Wiley" ;
	foaf:familyName "McFarlane" ;
	foaf:givenName "Kenedy Wiley" ;
	mydb:profession "American Missionary" ;
	mydb:note "Alexandria" ;
	mydb:note 1896-03-31 .

<mydb.org/id/19> a foaf:Person ;
	rdfs:label "Anna Henderson" ;
	foaf:givenName "Anna Henderson" ;
	mydb:wifeOf <mydb.org/id/18> .

<mydb.org/id/20> a foaf:Person ;
	rdfs:label "Mary Evelyn" ;
	foaf:givenName "Mary Evelyn" ;
	mydb:daughterOf <mydb.org/id/18> ;
	mydb:note "misspelled dauter" .

<mydb.org/id/21> a foaf:Person ;
	rdfs:label "Ralph Harvey"
	foaf:givenName "Ralph Harvey" ;
	mydb:sonOf <mydb.org/id/18> .

<mydb.org/id/22> a foaf:Person ;
	rdfs:label "Mogroby Jacob M" ;
	foaf:familyName "Mogroby" ;
	foaf:givenName "Jacob M" ;
	mydb:occupation "Ombrella Merchant" ;
	mydb:registrationDate 1897-06-01 ;
	mydb:registrationDocument <mydb.org/doc/1> ;

<mydb.org/doc/1> a passport ;
	mydb:docNumber 776 ;	
	mydb:docDate 1897-04-20 ;
	mydb:docIssued "Vienna Austria" .	

<mydb.org/id/23> a foaf:Person ;
	rdfs:label "Toba Mogroby" .
	foaf:familyName "Mogroby" ;
	foaf:givenName "Toba" ;
	mydb:wifeOf	<mydb.org/id/22> .

<mydb.org/id/24> a foaf:Person ;
	rdfs:label "Moses Mogroby" ;
	foaf:familyName "Mogroby" ;
	foaf:givenName "Moses" ;
	mydb:sonOf <mydb.org/id/24> ;
	mydb:birthDate	1898-02-10 .

<mydb.org/annot/0> a db:registrationNote ;
	mydb:recordedBy <mydb.org/id/24> ;
	mydb:date 1888-06-20 .

<mydb.org/id/25> a foaf:Person ;
	rdfs:label "Rev'd Dr. S C Ewing" ;
	foaf:familyName "Ewing" ;
	foaf:givenName "S C" ;
	schema:honorificPrefix "Rev'd" ;
	schema:honorificPrefix "Dr." ;
	mydb:occupation "ex US Consular Agent" .
```

Now the machine can supplies surname via link to parent.

## Step 5: Reconcile
* It doesn't matter whether it's "death cause" or "cause of death"--just link them. 
* Deliberately make a mistake with "occupation" and "profession"
* link diseases to Dbpedia

## Step 6: Work with your small database

To work with this data (for instance using SPARQL), you need to load it into a triplestore. There are various triplestores available. One of the most widely used is [Virtuoso Open-Source](http://virtuoso.openlinksw.com/dataspace/doc/dav/wiki/Main/). Getting Virtuoso up and running on your own computer can be a bit daunting, but there are lots of tips and shortcuts available. Remember, this is a *small* database that you're building. Most of the complexity in triplestores derives from large scale, data serving, and security demands. We're just trying to build a simple dataset for our own use, so we don't have to get everything right.

For Linux, follow Virtuoso's installation instructions for [your blend](http://virtuoso.openlinksw.com/dataspace/doc/dav/wiki/Main/VOSBuild#Building%20for%20Linux%20or%20other%20Unix-like%20OS) 

You'll have to move the virtuoso.ini file from /etc/virtuoso into the /var/lib/virtuoso/db folder, then execute:

$ sudo virtuoso-t -f virtuoso.ini

For Mac, install homebrew (see instructions in the [Jekyll tutorial](http://programminghistorian.org/lessons/building-static-sites-with-jekyll-github-pages#command-line-tools-suite-a-idsection2-1a)), then execute:

$ brew install virtuoso 

For Windows, download the [binary](http://virtuoso.openlinksw.com/dataspace/doc/dav/wiki/Main/VOSDownload#Pre-built%20binaries%20for%20Windows) and extract it to somewhere such as c:\virtuoso. Then execute from shell:

$ cd c:\virtuoso\database
$ virtuoso-t -f virtuoso.ini

### Working with your data
Once you have installed Virtuoso, open a web browser and navigate to 
[http://localhost:8890]. This should show you the Virtuoso front end. Log in using the default administrator username and password: dba and dba.

## Step 7: Expose

This is not easy.
