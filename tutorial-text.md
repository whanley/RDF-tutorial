---
title: Making a Small RDF Database
authors:
- Will Hanley
date: 2015-11-16
reviewers:
layout: default
---

Note: Matthew Lincoln's programminghistorian SPARQL tutorial explains how to access existing databases (catalogs, especially) that are written in RDF format. This tutorial is about building small RDF databases. A future tutorial will show how to produce an RDF database from an existing table via OpenRefine, focusing on skeleton/schema description.

# Making a small RDF database

When I'm in the archives, I often come across serial records of some sort--lists, stacks of forms, handwritten and corrected indexes--that I wish to record. One way to do this is an word document: efficient, but not very powerful. It's much easier to count, alphabetize, filter, and otherwise manipulate this data if I record it in a spreadsheet (like Excel) or database (like Filemaker or Access). Often, however, it's not clear to me what fields I should use to record the data. First name/last name, or whole name? Some dates are exact, others are just years, and sometimes an age is given. Sometimes the types of data shift over time. All of this means that I have to change my database, and go back over records I've already read, or leave the series incomplete.

[extend rationale: explain relational vs hierarchical vs tabular dbs]

There is a solution: the small RDF database. Once you learn the grammar of this language, it's quite simple to record data in it. There is no need to remake your database as your records change; just add new language and carry on. The great advantage comes at the end, because there are powerful querying tools (see the SPARQL tutorial) and it is easy to link your data to other data sets.

1. Open a text editor. Ideally, this will be a very simple program, like TextEdit (Mac), Notepad (Windows), or Gedit (Linux). But you can also use your normal word processor.

2. Make a declaration. RDF allows you to invent your own classification scheme as you go along. That classification scheme lives in your own "namespace," and your first job is to declare that namespace. Make up a name for your namespace and enter it at the top in the following format:

    @prefix mydb: <http://myowndata.org#> .

You can use whatever letters you want for "mydb" and whatever address you want for "myowndata.org" (the http:// does not need to refer to a real website).

3. Save the file. You will want to save your file as plain text. Careful, because your text editor or word processor might try to save it in a different format.

To save as plain text on TextEdit (Mac) use ⇧⌘T or Format > Make Plain Text. On Notepad (Windows), use File > Save As, and at the bottom of the Save As dialog box, select Plain text (*.txt) from the "Save as type" drop-down list. On Gedit (Linux), you shouldn't have any problem.

To save as plain text on a word processor like Microsoft Word, Libreoffice, or Open Office, use File > Save As, and at the bottom of the Save As dialog box, select Plain text (*.txt) from the "Save as type" drop-down list. When you press save, you may see a warning saying "This document may contain formatting or content that cannot be saved in the currently selected file format "Text"." Confirm that you wish to "use text format."

4. Start entering your data into the document. This is the part that is a bit tricky. You need to learn a new language, called Turtle. But don't worry too much, because the language is very, very simple. In fact, it has only one rule: it is written in "triples," which means that every sentence has exactly three words in it.

No need to learn the language right away, just imitate the format used below.

For example, take this table from a 1 July 1905 newspaper:

```
<http://myowndata.org#London> mydb:timeFromCompany'sOffices 14 ;
mydb:timeFromPostalTelegraphOffices  59 .

<http://myowndata.org#Liverpool> mydb:timeFromCompany'sOffices 14 ;
mydb:timeFromPostalTelegraphOffices  92 .

<http://myowndata.org#Manchester> mydb:timeFromCompany'sOffices 20 ;
mydb:timeFromPostalTelegraphOffices  64 .

<http://myowndata.org#Glasgow> mydb:timeFromCompany'sOffices 21 .

<http://myowndata.org#OtherProvincialOffices> mydb:timeFromPostalTelegraphOffices  64 .
```

Done! This is far from elegant, but it will work. (It won't work for long, but every database needs fixing before long). As you can see, the terms are derived directly from the material itself. Anything that you put behind your own invented namespace (mydb: in this instance) is yours to invent.

5. Get a triplestore. Here's a ranking of some popular triplestores.

6. Query it with SPARQL.

7. Export it for others to use. (as csv--how?)


___________________________________

Suppose you were working with a register of American nationals kept at the US consulate in Alexandria in the 1880s. This is a serial record that lists standardized information. It can help to answer questions like how many Americans were registered, what were their occupations, and so on. But the protocols of registration are inconsistent, and it's hard to know how to record the data in a standard format. Consider this page:

{% include figure.html src="../images/RDF-example.jpg" caption="Image of USNA" %}

One could transcribe it as a text file, more or less as follows:

```
		Mirzan Marie (left) (x)		|
							        |	
Edwige		daughter of 	Do.		|
Mary Rose	daughter of	Do.			|
Victor John	son of		Do.			|4th Sep'r 94. completed 21 years.			
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

One could even record the checks, circles, and crosses pencilled in on the left, even if their meaning is uncertain. The names would be searchable in a document. But statistics and other data work would have to be produced manually or using python searches for standard expressions and the like. But this serial data was already a database when it was produced more than a century ago, and it makes sense to transcribe it as data. So, how do we do this?

# Making a Database: the Tabular, Relational, and Hierarchical Options
The first approach to consider is to make a table out of this information, like so:

{% include figure.html src="../images/filename" caption="Table rendering of data" %}

It's not clear how many columns to create, or which columns are appropriate, even for this single page. As it is, there are 30 columns [count], and some information is still hidden. Other pages of the register introduce still more categories. A complete table would have many dozens of columns.

A second option is to turn the information into a relational database, in Filemaker or Access. Each head of household (four on this page) would have his or her own subsidiary table, listing information about his or her household.

A third option would be to encode the page above using XML tags, marking information for machine extraction and counting using python, for example. This would require the development of an elaborate and customized schema, however. The document would have to be carefully and consistently encoded in order to permit searches that would reveal, for example, the median number of children per household.

Ultimately, it is hard to see that any of these approaches would save much effort over mere transcription. And if the historian wanted to share this data with a colleague, a lot of explanation would be necessary.

In a case like this, a graph data approach might be useful.

# Making a small graph database

Turning this analog page into data is what computer scientists call "pre-processing." Let's go about this in a couple of steps, to show how the logic works. Once you have a bit of experience, you can skip the preliminary steps.

## Step 1: List
First, turn the linear text into a list. Every item on the list is a person that you want to represent in the database. We have some information about each of these persons: their name, as well as various other details.

1. Mirzan Marie (left) (x)
								
2. Edwige, daughter of Mirzan Marie.

3. Mary Rose, daughter of Mirzan Marie.

4. Victor John	son of Mirzan Marie. 4th Sep'r 94. completed 21 years.; Victor, Died today 1st November 1905 at 7 a.m. aged 32 years from Tuberculosi Polmonite. European Hospital.

5. Morpurgo D. Brutus (x),	Ironmonger died the 7 6 July 1906

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

(and we can add one more name from the footnote:)

25. Rev'd Dr. S C Ewing, ex US Consular Agent, who recorded information marked with an x on the 20th day of June 1888.

## Step 2: Structured list
Now, let's structure this list by describing the kind of details it includes about each person. We can invent relatively arbitrary categories for each type of detail, much as we would if we were putting the information into a table. One move that is novel here: for "ditto," we're substituting a number, which shows which person on the list the "ditto" refers to. Thus, we say that Edwige (#2 on the list) is the daughter of #1 (by which we mean Marie).

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

## Step 3: Translate into Machine-readable Language
Now, let's translate this structured list into the turtle language, so that a machine can read it. This is a bit more tricky. But every new URI "paragraph" is equivalent to a line on the list. (for the sake of clarity, we'll start the numbering at 1).

This translation starts with a declaration, which is a list of abbreviations that explains the vocabulary we use. Just as we did in step 2, we're going to invent some vocabulary to describe the information we've found. We're going to assign that invented vocabulary to our own namespace, which we'll call "db". (One pleasure of RDF is that it's easy to invent terms as you go along, and fix or reconcile them later—I'll give an example of that below).

Although you can invent your own vocabulary (what computer scientists call "schema") for everything,  there are quite a few well-made schemas in common use. It is good practice to adopt some of these if you can. In this example, we will use three [?] of the leading schemas: rdf, rdfs, foaf. Eventually, you might want to dig deeper into the schemas, which are listed [here]. For now, let's see how they can be implemented into the list we produced on step 2.

[all of this can be annotated, with arrows if necessary… show particular terms, why I chose to do things]. So, we're saying that every line is a URI—a unique object, known by its number (which looks like a web address, but which we can invent on our own). This URI is the first item in each paragraph. And we're saying a few other things:
each of these URIs represents a Person in the foaf (Friend of a Friend) schema. Machines know quite a lot about `foaf:Persons`—like they can have parents, children, genders, and so on—we'll see later what we can do with this. 
Each of these URIs has a label, which is the same as the person's name. This is what's called a literal (a "string" in computer-science-speak), and it appears between quotation marks. Labels are really useful for humans trying to read databases. When we query an RDF database, the machine looks for URIs, but we can tell the machine to answer us by substituting labels that we can read instead.

[Maybe I should only have as many lines here as I have in previous step?]

```turtle
@prefix db: 	<http://mydb.org/schema#> .
@prefix foaf:	<http://xmlns.com/foaf/0.1> .
@prefix rdf:	<http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs:	<http://www.w3.org/2000/01/rdf-schema#> .

<mydb.org/id/1> a foaf:Person ;
	rdfs:label "Mirzan Marie" ;
	db:note "left" ;
	db:note "x" .

<mydb.org/id/2> a foaf:Person ;	
	rdfs:label "Edwige" ;
	db:daughterOf <mydb.org/id/1> .

<mydb.org/id/3> a foaf:Person ;	
	rdfs:label "Mary Rose" ;
	db:daughterOf <mydb.org/id/1> .

<mydb.org/id/4> a foaf:Person ;	
	rdfs:label "Victor John" ;
	db:sonOf <mydb.org/id/1> ;
	db:note "4 9 1894 completed 21 years" ;
	db:deathDate "1 11 1905, 7 am" ;
	db:deathAge 32 ;
	db:deathPlace	"European Hospital" ;
	db:deathCause	 "Tuberculosi Polmonite" .

<mydb.org/id/5> a foaf:Person ;
	rdfs:label "Morpurgo D. Brutus" ;
	db:note "x" ;
	db:occupation "Ironmonger" ;
	db:deathDate "6 7 06" .

<mydb.org/id/6> a foaf:Person ;	
	rdfs:label "Clarisa Hélène" ;
	bd:wifeOf <mydb.org/id/5> .

<mydb.org/id/7> a foaf:Person ;	
	rdfs:label "Julia" ;
	db:motherOf <mydb.org/id/5> ;
	db:deathDate "9 7 1902, 4am" ;
	db:deathCause "cancer" .

<mydb.org/id/8> a foaf:Person ;	
	rdfs:label "Libera Rachel" ;
	db:sisterOf <mydb.org/id/5> .

<mydb.org/id/9> a foaf:Person ;	
	rdfs:label "Virginia" ;
	db:daughterOf <mydb.org/id/5> .

<mydb.org/id/10> a foaf:Person ;	
	rdfs:label "Réné" ;
	db:daughterOf <mydb.org/id/5> .

<mydb.org/id/11> a foaf:Person ;	
	rdfs:label "Angelino M." ;
	db:sonOf <mydb.org/id/5> .

<mydb.org/id/12> a foaf:Person ;	
	rdfs:label "Ugo" ;
	db:sonOf <mydb.org/id/5> .

<mydb.org/id/13> a foaf:Person ;	
	rdfs:label "Hector" ;
	db:sonOf <mydb.org/id/5> .

<mydb.org/id/14> a foaf:Person ;	
	rdfs:label "Rodolph" ;
	db:sonOf <mydb.org/id/5> .
	db:deathDate "4 4 1894" ;
	db:deathAge 16 ;
	db:deathCause "Diptheria" .

<mydb.org/id/15> a foaf:Person ;	
	rdfs:label "Edgar" ;
	db:sonOf <mydb.org/id/5> .

<mydb.org/id/16> a foaf:Person ;	
	rdfs:label "Oscar" ;
	db:sonOf <mydb.org/id/5> .

<mydb.org/id/17> a foaf:Person ;	
	rdfs:label "Hector" ;
	db:sonOf <mydb.org/id/5> .

<mydb.org/id/18> a foaf:Person ;
	rdfs:label "McFarlane Kenedy Wiley" ;
	db:profession "American Missionary" ;
	db:note "Alexandria 31 3 1896" .

<mydb.org/id/19> a foaf:Person ;
	rdfs:label "Anna Henderson" ;
	db:wifeOf <mydb.org/id/18> .

<mydb.org/id/20> a foaf:Person ;
	rdfs:label "Mary Evelyn" ;
	db:daughterOf <mydb.org/id/18> .

<mydb.org/id/21> a foaf:Person ;
	rdfs:label "Ralph Harvey"
	db:sonOf <mydb.org/id/18> .

<mydb.org/id/22> a foaf:Person ;
	rdfs:label "Mogroby Jacob M" ;
	db:occupation "Ombrella Merchant" ;
	db:registration	 "on 1.6.97 acc. to a Passport No 776 dated Vienna Austria 20th April 97".

<mydb.org/id/23> a foaf:Person ;
	rdfs:label "Toba Mogroby" .
	db:wifeOf <mydb.org/id/22> .

<mydb.org/id/24> a foaf:Person ;
	rdfs:label "Moses Mogroby" ;
	db:sonOf <mydb.org/id/24> ;
	db:birthdate "10 2 1898" .

<mydb.org/id/25> a foaf:Person ;
	rdfs:label "Rev'd Dr. S C Ewing" ;
	db:occupation "ex US Consular Agent" .
```

The machine can do a lot with this:
reasoner makes family tree. Knows that sons and daughters are male and female, etc.

## Step 4: Make some new objects
* make name parts into objects.
* Do something with the "left" annotation: make it an annot/1 of its own?
* Break out passport documents.
* Break out x annotation
* add sequence in list

```
@prefix db: 	<http://mydb.org/schema#> .
@prefix foaf:	<http://xmlns.com/foaf/0.1> .
@prefix rdf:	<http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs:	<http://www.w3.org/2000/01/rdf-schema#> .

<mydb.org/id/1> a foaf:Person ;
	rdfs:label "Mirzan Marie" ;
	annotation "left" ;
	annotation (x) .

<mydb.org/id/2> a foaf:Person ;	
	rdfs:label "Edwige" ;
	daughter of 	<mydb.org/id/1> .

<mydb.org/id/3> a foaf:Person ;	
	rdfs:label "Mary Rose" ;
	daughter of 	<mydb.org/id/1> .

<mydb.org/id/4> a foaf:Person ;	
	rdfs:label "Victor John" ;
	son of 	<mydb.org/id/1> ;
	associated date	4 9 1894 ;
	associated event	"completed 21 years" ;
	death date		1 11 1905 ;
	death time		7 am ;
	death age		32 ;
	death place		European Hospital ;
	death cause		"Tuberculosi Polmonite" .

<mydb.org/id/5> a foaf:Person ;
	rdfs:label "Morpurgo D. Brutus" ;
	annotation (x) ;
	occupation "Ironmonger" ;
	death date 6 7 06 .

<mydb.org/id/6> a foaf:Person ;	
	rdfs:label "Clarisa Hélène" ;
	wife of 	<mydb.org/id/5> .

<mydb.org/id/7> a foaf:Person ;	
	rdfs:label "Julia" ;
	mother of 	<mydb.org/id/5> ;
	death date		9 7 1902 ;
	death time		4 am ;
	death cause		"cancer" .

<mydb.org/id/8> a foaf:Person ;	
	rdfs:label "Libera Rachel" ;
	sister of 	<mydb.org/id/5> .

<mydb.org/id/9> a foaf:Person ;	
	rdfs:label "Virginia" ;
	daughter of 	<mydb.org/id/5> .

<mydb.org/id/10> a foaf:Person ;	
	rdfs:label "Réné" ;
	daughter of 	<mydb.org/id/5> .

<mydb.org/id/11> a foaf:Person ;	
	rdfs:label "Angelino M." ;
	son of 	<mydb.org/id/5> .

<mydb.org/id/12> a foaf:Person ;	
	rdfs:label "Ugo" ;
	son of 	<mydb.org/id/5> .

<mydb.org/id/13> a foaf:Person ;	
	rdfs:label "Hector" ;
	son of 	<mydb.org/id/5> .

<mydb.org/id/14> a foaf:Person ;	
	rdfs:label "Rodolph" ;
	son of 	<mydb.org/id/5> .
	death date		4 4 1894 ;
	death age		16 ;
	death cause		"Diptheria" .

<mydb.org/id/15> a foaf:Person ;	
	rdfs:label "Edgar" ;
	son of 	<mydb.org/id/5> .

<mydb.org/id/16> a foaf:Person ;	
	rdfs:label "Oscar" ;
	son of 	<mydb.org/id/5> .

<mydb.org/id/17> a foaf:Person ;	
	rdfs:label "Hector" ;
	son of 	<mydb.org/id/5> .

<mydb.org/id/18> a foaf:Person ;
	rdfs:label "McFarlane Kenedy Wiley" ;
	profession "American Missionary" ;
	Note 	"Alexandria" ;
	note 	31 3 1896.

<mydb.org/id/19> a foaf:Person ;
	rdfs:label "Anna Henderson" ;
	wife of <mydb.org/id/18> .

<mydb.org/id/20> a foaf:Person ;
	rdfs:label "Mary Evelyn" ;
	daughter of <mydb.org/id/18> ;
	note "misspelled dauter" .

<mydb.org/id/21> a foaf:Person ;
	rdfs:label "Ralph Harvey"
	son of	<mydb.org/id/18> .

<mydb.org/id/22> a foaf:Person ;
	rdfs:label "Mogroby Jacob M" ;
	occupation "Ombrella Merchant" ;
	db:registration	 date 	1 6 1897 ;
	db:registration document <mydb.org/doc/1> ;

<mydb.org/doc/1> a passport ;
	number 776 ;	
	date 20 4 97 ;
	place  Vienna Austria .	

<mydb.org/id/23> a foaf:Person ;
	rdfs:label "Toba Mogroby" .
	Wife of	<mydb.org/id/22> .

<mydb.org/id/24> a foaf:Person ;
	rdfs:label "Moses Mogroby" ;
	son of	<mydb.org/id/24> ;
	birth date 	10 2 1898 .

<mydb.org/annot/0> a db:registrationNote ;
	recorded by <mydb.org/id/24> ;
	date 20 6 1888 .

<mydb.org/id/25> a foaf:Person ;
	rdfs:label "Rev'd Dr. S C Ewing" ;
	occupation "ex US Consular Agent" .
```

Now the machine can supplies surname via link to parent.

## Step 5: Reconcile
* It doesn't matter whether it's "death cause" or "cause of death"--just link them. 
* Deliberately make a mistake with "occupation" and "profession"
* link diseases to Dbpedia

## Step 6: Expose
