---
title: Wikidata for historians  
collection: lessons  
layout: lesson  
authors:
- Will Hanley
---

{% include toc.html %}

In this lesson you will learn what kinds of information Wikidata contains, how to explore that data in order to contextualize questions in your historical research, and how to edit and add data in Wikidata to store your own data and share it with others.

## What is Wikidata?

Wikidata is the world's largest open data set. Like any database, it operates according to rigid rules about how information must be structured. Unlike most databases, Wikidata's structure prioritizes interoperability, linking to data sets, and open contribution.  

Wikidata is a sibling of Wikipedia, and shares its politics of knowledge production and dissemination. The debate over Wikipedia's merits and faults is rich; many historians will agree that it's a convenient place to look up facts, refreshingly broad and democratic in its coverage, but not reliable in its synthesis.

Wikidata's content, on the other hand, is all facts and no synthesis. Without worrying too much about the format, spend a bit of time scrolling through what Wikidata has to say about [Gamal Abdel Nasser](https://www.wikidata.org/wiki/Q39524).

First, you will see many variant versions and spellings of his name, in various languages. A bit further down, a section of "Statements" begins. Some of these statements are transparent, like the sort of thing you'd see on a passport: "sex or gender" is "male", "date of birth." Others (such as "instance of" "human") may be a bit less obvious.

Even further down, you'll find another section with the heading "Identifiers." Here you'll find the unique identifier that dozens of other databases--from the Library of Congress to the Internet Movied Database--use for Abdel Nasser in their systems. And at the very bottom, you will see a list of all of the Wikipedia pages about him.

On the face of things, there's nothing especially enticing about a list of details like this trivia, which can be looked up in lots of places. But Wikidata's value and power lies not in such isolated details, but in the way that it combines these details with all of the other details it contains. It does this with a powerful seach protocol called SPARQL, about which more later.

Here is a trivial example of combined details. Let's say your whimsy leads you to wonder about the age at which twentieth-century leaders took power. There's [a query for that](https://query.wikidata.org/#%23%20Age%20of%20heads%20of%20state%20upon%20taking%20power%2C%201950-1980%0ASELECT%20%3FheadOfState%20%3FheadOfStateLabel%20%3Fage%20%3FcountryLabel%20%3FpositionLabel%20%0A%7B%0A%20%23%20find%20heads%20of%20state%20positions%0A%20hint%3AQuery%20hint%3Aoptimizer%20%22None%22.%0A%20%3Fposition%20wdt%3AP279%2a%20wd%3AQ48352%20.%0A%0A%20%23%20sovereign%20states%20only%0A%20%3Fposition%20wdt%3AP1001%20%3Fcountry%20.%0A%20%3Fcountry%20wdt%3AP31%20wd%3AQ3624078%20.%0A%20%20%0A%20%23%20fetch%20names%20of%20officeholders%0A%20%3FheadOfState%20wdt%3AP39%20%3Fposition%20.%0A%0A%20%23%20birthdates%20of%20officeholders%0A%20%3FheadOfState%20wdt%3AP569%20%3Fdob.%20hint%3APrior%20hint%3ArangeSafe%20true.%20%0A%20%20%0A%20%23%20date%20of%20term%20start%0A%20%3FheadOfState%20p%3AP39%20%3Fstatement%20.%0A%20%3Fstatement%20ps%3AP39%20%3Fposition%20.%20%0A%20%3Fstatement%20pq%3AP580%20%3FtermStart.%20hint%3APrior%20hint%3ArangeSafe%20true.%0A%20FILTER%28%221950-01-01%22%5E%5Exsd%3AdateTime%20%3C%3D%20%3FtermStart%20%26%26%20%3FtermStart%20%3C%20%221980-01-01%22%5E%5Exsd%3AdateTime%29%0A%20BIND%28YEAR%28%3FtermStart%29-YEAR%28%3Fdob%29%20as%20%3Fage%29%0A%20SERVICE%20wikibase%3Alabel%20%7B%20bd%3AserviceParam%20wikibase%3Alanguage%20%22en%22%20%7D%0A%7D%0AORDER%20BY%20DESC%28%3Fage%29)!

[not working right]

This should show us that Abdel Nasser became president when he was 38, in 1954. Comparing him to the hundreds of others listed, we can see that some heads of state first took power at an older age, and some at a younger age. This is a good starting point for data exploration and hypothesis testing. Wikidata supports data-informed contextualization of this kind.

It is essential, however, to recognize what Wikidata does not--and should not be expected--to do. Its knowledge base is vast, but it will always be incomplete. While many or even most of the ages on this list of results are more or less correct, we find some individual answers that don't make sense, and others that are missing. 

This incompleteness is a function of two features of the knowledge base. First, there is no guarantee that the data it contains are accurate. At the time of writing, there were eight references attested for Abdel Nasser's date of birth, but only one for the date when he assumed the Prime Minister's office. That reference is to the [list of Egyptian Prime Ministers in English Wikipedia](https://en.wikipedia.org/wiki/List_of_prime_ministers_of_Egypt)--which might itself be scrutinized. Generally, though, we can reasonably take it on trust that most information of this sort in Wikidata will mostly be right.

A second (and more interesting) issue concerns the structure of knowledge that Wikidata produces, and this issue warrants a section of its own. 

## Wikidata ontology
Categories and semantics are a juicy problem for any historian. This is true in analog scholarly debate, and it is also true in Wikidata and other semantic data structures. 

While there is probably a general consensus on the meaning of "date of birth," the meaning of "head of state" is not so clear. The age query above used the Wikidata item "head of state"([Q48352](https://www.wikidata.org/wiki/Q48352)) to identify Abdel Nasser and his counterparts in other countries. But Wikidata also contains an item labeled "head of government"([Q2285706](https://www.wikidata.org/wiki/Q2285706)). For Abdel Nasser, this query returns his age when he became prime minister rather than president. But this query accurately captures other leaders who are not returned with the "head of state" query. For instance, it returns prime ministers in states (like India?) where the president is largely ceremonial.

These ambiguous results embody the virtue and value of Wikidata for historians. It is not an automatic answer box. It is an elaborate logical structure giving precise answers to abstract questions about which there is no real consensus. Any serious user must use their knowledge of context to mediate between the theoretical and the empirical. Fortunately, this is exactly what historians are trained to do.

Let's take a closer look at the "head of state" and "head of government" items.

Like good historians, we will want to look at this object of knowledge from a few different angles. We can look at its history: who has edited this page, when, and why? We can look at its use in wikidata: in the left hand menu, there's a "what links here" field. If you click on it, you will see a list of the hundreds of items in Wikidata that use "head of state" in some way or another. This particular list is pretty bewildering, but looking at "what links here" for more obscure items can be quite instructive. For example, [Abdel Nasser's birthplace]

Now is the time to introduce a concept that may be new to most historians: class. This is not the class of "class struggle," but a kind of logic for organizing concepts in a data structure. Looking again at the item page, we see that "head of state" is an "instance of" (or kind or example of) "public office," but a "subclass of" "statesperson" and "leader." This latter statement means that all heads of state are statespersons and leaders, but not all statesperson and leaders are heads of state. 

Keen-eyed observers will already detect the presence of a formal taxonomy here. Sure enough, we can look down a step to see all of the public offices that are subclasses of "head of state." Here's a [query that lists them](). 

Historians tend to be sceptical of taxonomic schemes, with good reason. But explicit taxonomies can do great work for us as a means to discover and explore information. No doubt you will find (what you consider to be) errors in this taxonomy. You can certainly "correct" those errors--Wikidata is open--but don't be too hasty. The taxonomies already existing in Wikidata are organic and collectively produced, and they are not exclusive. There are ways to work around the parts you don't agree with--don't change the knowledge base itself until you have learned how to do this.

A big part of our expertise as historians is contextualizing details. For many of us, that's the fun and fascinating work. And that skill is precisely what a researcher needs to interact with the galaxy of isolated factoids in Wikidata. The rest of this lesson shows a few of the main ways to do that.

### How does Wikidata structure its data?

This section, which offers a somewhat deeper dive into Wikidata, is not essential reading for all users of this lesson. Wikidata's engineers have made it possible to bypass much of the technical detail with their graphic search interface.

Other users, however, might wish to know more about the guts of the process. This is especially true for those computational historians interested in linked data, semantic data structures, graph data and the like--some of which is described in other Programming Historian lessons. Wikidata is the world's richest linked database, and is thus a great way to explore this data structure.

One key to understanding Wikidata can be found in every page's URL. Look at Gamal Abdel Nasser's page again, which has the URL `https://www.wikidata.org/wiki/Q39524.` In Wikidata, the idea of this dead Egyptian man begins and ends with the unique identifer Q39524. This Q-number is the essence of the item; everything else is semantics, which is to say unessential labels and statements about this identifier. 

Most historians have some background in postcolonial theory, and can readily identify the value in distinguishing between the signifier and the signified. In the wikidata knowledge base, the signfied are only ever numbers; everything else is a signifier.

P-number is property


## Querying Wikidata

There are no bad questions in Wikidata--only unrealistic questions. The knowledge base should not be expected to provide comprehensive answers to any questions. But it does better with some questions than others.

As in most historical domains, the data trail of dead white men is relatively better represented. Want a list of colleges attended by governors of American states? [This query](https://query.wikidata.org/#SELECT%20%3FcollegeLabel%20%28COUNT%28%3FcollegeLabel%29%20AS%20%3Fcount%29%20%0AWHERE%0A%7B%20%3Foffice%20wdt%3AP279%2a%20wd%3AQ889821.%0A%20%20%3Fgovernor%20wdt%3AP39%20%3Foffice.%0A%20%20%3Fgovernor%20wdt%3AP69%20%3Fcollege.%0A%20%20%3Fcollege%20wdt%3AP625%20%3Fcoord%20.%0A%20%20SERVICE%20wikibase%3Alabel%20%7B%20bd%3AserviceParam%20wikibase%3Alanguage%20%22%5BAUTO_LANGUAGE%5D%2Cen%22.%20%7D%0A%7D%0AGROUP%20BY%20%3FcollegeLabel%20%0AORDER%20BY%20DESC%28%3Fcount%29) is probably about right. Want a list of colleges attended by governors of Egyptian provinces? [This query]() does not yield much.

Generally speaking, questions about specifics, like dates and locations and labels, yield better answers from Wikidata. For example, here's a map of the birthplaces of governors of American states, color coded by half-century of birth. And here's a list of the names of governors of American states in Chinese and Arabic.

These queries come from the Wikidata query service. Playing around with this service is one of the best ways to learn about Wikidata--and about linked data in general. The most powerful way to do this is using the SPARQL query language, which is introduced in another Programming Historian lesson. SPARQL is incredibly flexible, but it takes some learning.

Wikidata offers two great workarounds. First, it offers a long list of example queries. Try them out! All of these queries can be adapted for your own interests, by substituting the item you want for the item the query contains. 
- Cats replace with Presidents of Egypt
- Find one that can be filtered with date to make it historical.

Second, Wikidata offers a graphic query interface. This can't do all of things that SPARQL can do, but it can set up a basic structure for your queries.

## Contributing to Wikidata

Every Wikipedia page links to a wikidata item

Login

See other languages

Add reference

Add birthplace?

Batch editing: subject of another lesson.