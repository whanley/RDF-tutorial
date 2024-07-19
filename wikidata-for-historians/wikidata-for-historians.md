---
title: Wikidata for historians  
collection: lessons  
layout: lesson  
authors:
- Will Hanley
---

{% include toc.html %}

# Lesson Overview

In this lesson you will learn:
- what kinds of information Wikidata contains, 
- how to explore that data in order to contextualize questions in your historical research, and
- how to edit and add data in Wikidata to store your own data and share it with others.

While this stand-alone lesson focuses on Wikidata, it can also serve to extend your understanding of Linked Open Data. Wikidata is the most user-friendly implementation of this data structure, and it's under constant development. It's a great place to learn about key graph data features, such as schemas and the SPARQL query language, which can be applied in other contexts. After–or before–completing this lesson, users might wish to read Jonathan Blaney's [Introduction to the Principles of Linked Open Data](https://programminghistorian.org/en/lessons/intro-to-linked-data), which covers some of the same ground.

There are no prerequisites for this lesson, but users with a Wikipedia login might wish to log in to Wikidata using the same credential.

# What is Wikidata?

Wikidata is the world's largest open data set. Like any database, it operates according to rigid rules about how information must be structured. Unlike most databases, Wikidata's structure prioritizes open contribution and collaboration protocols, interoperability, and linking between data sets. For historians, this data structure offers something especially attractive: in cases of uncertainty, it can accommodate more than one answer.

Wikidata is a sibling of Wikipedia, and shares its [politics of knowledge production and dissemination](https://en.wikipedia.org/wiki/Wikipedia:Five_pillars), as well as the [values of the Wikimedia Foundation](https://wikimediafoundation.org/about/values/#a1-we-are-in-this-mission-together). The debate over Wikipedia's merits and faults is rich; many historians will agree that it's a convenient place to look up facts, refreshingly broad and democratic in its coverage, but it can be unreliable in its synthesis.[^1]

Wikidata's content is very different in nature from that of Wikipedia: all facts and no synthesis. To get an idea of its nature, and without worrying too much about the format, spend a bit of time scrolling through what Wikidata has to tell us about [Gamal Abdel Nasser](https://www.wikidata.org/wiki/Q39524). 

At the top of the page, you will see many variant versions and spellings of his name, in various languages. Wikidata's multilingual functionality is superb. Click on "all entered languages" to see just how true this is. If you wish to interface with the whole knowledge base in a language other than English, log in and click "English" at the top. You may now choose another language (and you can [do more with language on Wikidata](https://www.wikidata.org/wiki/Help:Navigating_Wikidata/User_Options#Language_settings)). 

![Figure 1: Nasser labels and languages](nasser-labels-languages.png)

A bit further down, a section of **Statements** begins. Some of these statements are the sort of transparent information you'd see on a passport: "sex or gender" is "male", "date of birth." Others (such as "instance of" "human") may be a bit less obvious–we'll say more about those later.

Even further down, you'll find another section with the heading **Identifiers**. Here you'll find the unique identifier that dozens of other databases–from the Library of Congress to the Internet Movie Database–use for Abdel Nasser in their systems. This avalanche of identifiers is characteristic of the linked data universe. And at the very bottom, you will see a list of all of the Wikipedia pages about him. 

Unique identifiers are one key to understanding Wikidata. Wikidata's own identifiers can be found in the URL of Abdel Nasser's page, which is `https://www.wikidata.org/wiki/Q39524.` Abdel Nasser and all of the other objects that Wikidata describes are called "items". The Q-number `Q39524`, which is the unique identifier that you find at the end of the URL and at the top of Abdel Nasser's page, is the essence of the item. Everything else is you see on the page is semantics: optional labels and signfiers and statements about this identifier.

It is useful to know that every Wikipedia page has a counterpart item in Wikidata, accessible via a link the left hand tools menu:

![Figure 1: Wikidata item on Wikipedia](wikidata-item-on-wikipedia.png)

Take a look at the Wikidata item associated with Wikipedia page on a subject of interest to you. Click on links on that page–some will make sense, probably; others will not.

On the face of things, there's nothing especially enticing about these lists of details. You can look easily up most of this trivia in any decent reference book. But Wikidata's value does not consist in its isolated factoids. Instead, Wikidata's power derives from the way that it combines these data points with all of the other data it contains. It does this with a powerful seach protocol called SPARQL. 

We'll learn more about SPARQL itself later, but let's start by exploring a trivial example of combined details. Let's say your whimsy leads you to wonder about the age at which mid-twentieth-century leaders took power. Here's [a query that returns that information](https://query.wikidata.org/#%23%20Age%20of%20heads%20of%20state%20upon%20taking%20power%2C%201950-1980%0ASELECT%20DISTINCT%20%3FheadOfState%20%3FheadOfStateLabel%20%28MIN%28%3Fage%29%20as%20%3FageMin%29%20%3FcountryLabel%20%3FpositionLabel%0A%7B%0A%20%23%20find%20heads%20of%20state%20positions%0A%20hint%3AQuery%20hint%3Aoptimizer%20%22None%22.%0A%20%3Fposition%20wdt%3AP279%2a%20wd%3AQ48352%20.%0A%0A%20%23%20sovereign%20states%20only%0A%20%3Fposition%20wdt%3AP1001%20%3Fcountry%20.%0A%20%3Fcountry%20wdt%3AP31%20wd%3AQ3624078%20.%0A%20%20%0A%20%23%20fetch%20names%20of%20officeholders%0A%20%3FheadOfState%20wdt%3AP39%20%3Fposition%20.%0A%0A%20%23%20birthdates%20of%20officeholders%0A%20%3FheadOfState%20wdt%3AP569%20%3Fdob.%20hint%3APrior%20hint%3ArangeSafe%20true.%20%0A%20%20%0A%20%23%20date%20of%20term%20start%0A%20%3FheadOfState%20p%3AP39%20%3Fstatement%20.%0A%20%3Fstatement%20ps%3AP39%20%3Fposition%20.%20%0A%20%3Fstatement%20pq%3AP580%20%3FtermStart.%20hint%3APrior%20hint%3ArangeSafe%20true.%0A%20%20%0A%20%23%201950-1980%20term%20start%20only%0A%20FILTER%28%221950-01-01%22%5E%5Exsd%3AdateTime%20%3C%3D%20%3FtermStart%20%26%26%20%3FtermStart%20%3C%20%221980-01-01%22%5E%5Exsd%3AdateTime%29%0A%20%0A%20%23%20calcuate%20age%20%0A%20BIND%28YEAR%28%3FtermStart%29-YEAR%28%3Fdob%29%20as%20%3Fage%29%0A%20SERVICE%20wikibase%3Alabel%20%7B%20bd%3AserviceParam%20wikibase%3Alanguage%20%22en%22%20%7D%0A%7D%0AGROUP%20BY%20%3FheadOfState%20%3FheadOfStateLabel%20%3FcountryLabel%20%3FpositionLabel%0AORDER%20BY%20DESC%28%3FageMin%29) via the Wikidata query service. To execute the query, press the blue "play" button at the bottom left.

Scrolling down the table of results, which are sorted by age, you will find that Abdel Nasser became president when he was 38 (in 1954). Comparing him to the hundreds of other heads of state listed, we can see that some first took power at an older age, and some at a younger age. If you happen to be interested in leaders, life cycle, and generations, this list is a good starting point for further data exploration and hypothesis testing. 

Wikidata supports data-informed contextualization of this kind. It's easy, with an additional query statement, to enrich your data: [add a continent column](https://query.wikidata.org/#%23%20Age%20of%20heads%20of%20state%20upon%20taking%20power%2C%201950-1980%2C%20with%20continent%20listed%0ASELECT%20DISTINCT%20%3FheadOfState%20%3FheadOfStateLabel%20%28MIN%28%3Fage%29%20as%20%3FageMin%29%20%3FcountryLabel%20%3FpositionLabel%20%3FcontinentLabel%0A%7B%0A%20%23%20find%20heads%20of%20state%20positions%0A%20hint%3AQuery%20hint%3Aoptimizer%20%22None%22.%0A%20%3Fposition%20wdt%3AP279%2a%20wd%3AQ48352%20.%0A%0A%20%23%20sovereign%20states%20only%0A%20%3Fposition%20wdt%3AP1001%20%3Fcountry%20.%0A%20%3Fcountry%20wdt%3AP31%20wd%3AQ3624078%20.%0A%20%3Fcountry%20wdt%3AP30%20%3Fcontinent%20.%0A%20%20%0A%20%23%20fetch%20names%20of%20officeholders%0A%20%3FheadOfState%20wdt%3AP39%20%3Fposition%20.%0A%0A%20%23%20birthdates%20of%20officeholders%0A%20%3FheadOfState%20wdt%3AP569%20%3Fdob.%20hint%3APrior%20hint%3ArangeSafe%20true.%20%0A%20%20%0A%20%23%20date%20of%20term%20start%0A%20%3FheadOfState%20p%3AP39%20%3Fstatement%20.%0A%20%3Fstatement%20ps%3AP39%20%3Fposition%20.%20%0A%20%3Fstatement%20pq%3AP580%20%3FtermStart.%20hint%3APrior%20hint%3ArangeSafe%20true.%0A%20%20%0A%20%23%201950-1980%20term%20start%20only%0A%20FILTER%28%221950-01-01%22%5E%5Exsd%3AdateTime%20%3C%3D%20%3FtermStart%20%26%26%20%3FtermStart%20%3C%20%221980-01-01%22%5E%5Exsd%3AdateTime%29%0A%20%0A%20%23%20calcuate%20age%20%0A%20BIND%28YEAR%28%3FtermStart%29-YEAR%28%3Fdob%29%20as%20%3Fage%29%0A%20SERVICE%20wikibase%3Alabel%20%7B%20bd%3AserviceParam%20wikibase%3Alanguage%20%22en%22%20%7D%0A%7D%0AGROUP%20BY%20%3FheadOfState%20%3FheadOfStateLabel%20%3FcountryLabel%20%3FpositionLabel%20%3FcontinentLabel%0AORDER%20BY%20DESC%28%3FageMin%29), [add a gender column and count of spouses and children](https://query.wikidata.org/#%23%20Age%20of%20heads%20of%20state%20upon%20taking%20power%2C%201950-1980%2C%20with%20gender%2C%20spouse%20and%20child%20count%0ASELECT%20DISTINCT%20%3FheadOfState%20%3FheadOfStateLabel%20%28MIN%28%3Fage%29%20as%20%3FageMin%29%20%3FcountryLabel%20%3FpositionLabel%20%20%3FgenderLabel%20%28COUNT%28%3Fspouse%29%20as%20%3Fspouses%29%20%28COUNT%28%3Fchild%29%20as%20%3Fchildren%29%0A%7B%0A%20%23%20find%20heads%20of%20state%20positions%0A%20hint%3AQuery%20hint%3Aoptimizer%20%22None%22.%0A%20%3Fposition%20wdt%3AP279%2a%20wd%3AQ48352%20.%0A%0A%20%23%20sovereign%20states%20only%0A%20%3Fposition%20wdt%3AP1001%20%3Fcountry%20.%0A%20%3Fcountry%20wdt%3AP31%20wd%3AQ3624078%20.%0A%20%20%0A%20%23%20fetch%20names%20of%20officeholders%0A%20%3FheadOfState%20wdt%3AP39%20%3Fposition%20.%0A%20%20%0A%20%20%23%20fetch%20gender%0A%20%20%3FheadOfState%20wdt%3AP21%20%3Fgender%20.%0A%20%20%0A%20%20%23%20fetch%20spouses%20and%20children%0A%20%20OPTIONAL%7B%3FheadOfState%20wdt%3AP26%20%3Fspouse%20.%7D%0A%20%20OPTIONAL%7B%3FheadOfState%20wdt%3AP40%20%3Fchild%20.%7D%0A%0A%20%23%20birthdates%20of%20officeholders%0A%20%3FheadOfState%20wdt%3AP569%20%3Fdob.%20hint%3APrior%20hint%3ArangeSafe%20true.%20%0A%20%20%0A%20%23%20date%20of%20term%20start%0A%20%3FheadOfState%20p%3AP39%20%3Fstatement%20.%0A%20%3Fstatement%20ps%3AP39%20%3Fposition%20.%20%0A%20%3Fstatement%20pq%3AP580%20%3FtermStart.%20hint%3APrior%20hint%3ArangeSafe%20true.%0A%20%20%0A%20%23%201950-1980%20term%20start%20only%0A%20FILTER%28%221950-01-01%22%5E%5Exsd%3AdateTime%20%3C%3D%20%3FtermStart%20%26%26%20%3FtermStart%20%3C%20%221980-01-01%22%5E%5Exsd%3AdateTime%29%0A%20%0A%20%23%20calcuate%20age%20%0A%20BIND%28YEAR%28%3FtermStart%29-YEAR%28%3Fdob%29%20as%20%3Fage%29%0A%20SERVICE%20wikibase%3Alabel%20%7B%20bd%3AserviceParam%20wikibase%3Alanguage%20%22en%22%20%7D%0A%7D%0AGROUP%20BY%20%3FheadOfState%20%3FheadOfStateLabel%20%3FcountryLabel%20%3FpositionLabel%20%3FgenderLabel%0AORDER%20BY%20DESC%28%3FageMin%29) [**there's something wrong with this query**], or [add a column giving each leader's name in her or his native language](https://query.wikidata.org/#%23%20Age%20of%20heads%20of%20state%20upon%20taking%20power%2C%201950-1980%2C%20with%20name%20in%20native%20language%0ASELECT%20DISTINCT%20%3FheadOfState%20%3FheadOfStateLabel%20%20%3FnameInNativeLanguage%20%28MIN%28%3Fage%29%20as%20%3FageMin%29%20%3FcountryLabel%20%3FpositionLabel%0A%7B%0A%20%23%20find%20heads%20of%20state%20positions%0A%20hint%3AQuery%20hint%3Aoptimizer%20%22None%22.%0A%20%3Fposition%20wdt%3AP279%2a%20wd%3AQ48352%20.%0A%0A%20%23%20sovereign%20states%20only%0A%20%3Fposition%20wdt%3AP1001%20%3Fcountry%20.%0A%20%3Fcountry%20wdt%3AP31%20wd%3AQ3624078%20.%0A%20%20%0A%20%23%20fetch%20names%20of%20officeholders%0A%20%3FheadOfState%20wdt%3AP39%20%3Fposition%20.%0A%20%0A%20%20%23%20fetch%20name%20in%20native%20language%0A%20OPTIONAL%20%7B%3FheadOfState%20wdt%3AP1559%20%3FnameInNativeLanguage%20.%7D%0A%20%0A%20%23%20birthdates%20of%20officeholders%0A%20%3FheadOfState%20wdt%3AP569%20%3Fdob.%20hint%3APrior%20hint%3ArangeSafe%20true.%20%0A%20%20%0A%20%23%20date%20of%20term%20start%0A%20%3FheadOfState%20p%3AP39%20%3Fstatement%20.%0A%20%3Fstatement%20ps%3AP39%20%3Fposition%20.%20%0A%20%3Fstatement%20pq%3AP580%20%3FtermStart.%20hint%3APrior%20hint%3ArangeSafe%20true.%0A%20%20%0A%20%23%201950-1980%20term%20start%20only%0A%20FILTER%28%221950-01-01%22%5E%5Exsd%3AdateTime%20%3C%3D%20%3FtermStart%20%26%26%20%3FtermStart%20%3C%20%221980-01-01%22%5E%5Exsd%3AdateTime%29%0A%20%0A%20%23%20calcuate%20age%20%0A%20BIND%28YEAR%28%3FtermStart%29-YEAR%28%3Fdob%29%20as%20%3Fage%29%0A%20SERVICE%20wikibase%3Alabel%20%7B%20bd%3AserviceParam%20wikibase%3Alanguage%20%22en%22%20%7D%0A%7D%0AGROUP%20BY%20%3FheadOfState%20%3FheadOfStateLabel%20%3FcountryLabel%20%3FpositionLabel%20%3FnameInNativeLanguage%0AORDER%20BY%20DESC%28%3FageMin%29).

It is essential, however, to recognize what Wikidata does not do–and should not be expected to do. The knowledge base is vast, but it will always be incomplete. While many or even most of the data in this list of results are more or less correct, we find some individual answers that don't make sense, and others that are missing.

This incompleteness is a function of two features of the knowledge base. First, there is no guarantee that the data it contains are accurate. At the time of writing, there were eight references attested for Abdel Nasser's date of birth:

![Figure 3: Nasser birthdate references](nasser-birth-references.png)

However, we only have one reference for the date when he assumed the Prime Minister's office. That reference is to the [list of Egyptian Prime Ministers in English Wikipedia](https://en.wikipedia.org/wiki/List_of_prime_ministers_of_Egypt)–which might itself be scrutinized. Generally speaking, at this point in its development, Wikidata's references are poor in quality and quantity. However, in many cases, we can reasonably take it on trust that information of this sort in Wikidata will mostly be right. Our historian's judgment will serve us well when we look at the evidence.

A second (and more interesting) reason for incomplete query results concerns the structure of knowledge that Wikidata produces, and this issue warrants a section of its own. 

# Wikidata's taxonomies
Categories and semantics are a juicy problem for any historian. This is true in analog scholarly debate, and it is also true when we consider Wikidata and other semantic data structures.

While there is (probably) general consensus on the meaning of "date of birth," the meaning of "head of state" is not so clear. The age query above used the Wikidata item "head of state"([Q48352](https://www.wikidata.org/wiki/Q48352)) to identify Abdel Nasser and his counterparts in other countries. But Wikidata also contains an item labeled "head of government"([Q2285706](https://www.wikidata.org/wiki/Q2285706)). Let's take a closer look at the "head of state" and "head of government" items.

It is the time to introduce a concept that may be new to most historians: class. Not the class of "class struggle," silly! In database ontology, class is a kind of logic for organizing concepts in a data structure. Looking again at the item page, we see that "head of state" is an "instance of" (or kind or example of) "public office," but a "subclass of" "statesperson" and "leader." This latter statement means that all heads of state are statespersons and leaders, but not all statesperson and leaders are heads of state. 

Keen-eyed observers will already detect the presence of a formal taxonomy here. Sure enough, we can look down a step to see all of the public offices that are subclasses of "head of state." Here's a [query that lists them](https://query.wikidata.org/#%23Subclass%20of%20head%20of%20state%0ASELECT%20%3FheadOfStateSubclass%20%3FheadOfStateSubclassLabel%20%28COUNT%28%3Fitem%29%20as%20%3Fcount%29%0AWHERE%0A%7B%0A%20%20%3FheadOfStateSubclass%20wdt%3AP279%2a%20wd%3AQ48352.%0A%20%20%3Fitem%20wdt%3AP31%7Cwdt%3AP39%7Cwdt%3AP106%20%3FheadOfStateSubclass.%0A%20%20SERVICE%20wikibase%3Alabel%20%7B%20bd%3AserviceParam%20wikibase%3Alanguage%20%22%5BAUTO_LANGUAGE%5D%2Cen%22.%20%7D%20%0A%7D%0AGROUP%20BY%20%3FheadOfStateSubclass%20%3FheadOfStateSubclassLabel%20%0AORDER%20BY%20DESC%28%3Fcount%29) (ordered by number of instances). You will notice that "head of state" gives us a lot of monarchs and US state governors, among thousands of other positions. 

In contrast, consider this list of the [subclasses of "head of government"](https://query.wikidata.org/#%23Subclass%20of%20head%20of%20government%0ASELECT%20%3FheadOfGovtSubclass%20%3FheadOfGovtSubclassLabel%20%28COUNT%28%3Fitem%29%20as%20%3Fcount%29%0AWHERE%0A%7B%0A%20%20%3FheadOfGovtSubclass%20wdt%3AP279%2a%20wd%3AQ2285706.%0A%20%20%3Fitem%20wdt%3AP31%7Cwdt%3AP39%7Cwdt%3AP106%20%3FheadOfGovtSubclass.%0A%20%20SERVICE%20wikibase%3Alabel%20%7B%20bd%3AserviceParam%20wikibase%3Alanguage%20%22%5BAUTO_LANGUAGE%5D%2Cen%22.%20%7D%20%0A%7D%0AGROUP%20BY%20%3FheadOfGovtSubclass%20%3FheadOfGovtSubclassLabel%20%0AORDER%20BY%20DESC%28%3Fcount%29). "Head of government" is dominated by mayors and prime ministers. ("Captain Regent of San Marino" features high on both lists--obviously the result of a zealous contributor.) Confronted with this mass of examples, you might feel somewhat confused, both by particular instances and by the overall picture. This empirically-generated account is a long way from the synthetic statements of Wikipedia.

These ambiguous results embody the virtue and value of Wikidata for historians. It is not an automatic answer box. It is an elaborate logical structure giving precise answers to abstract questions about which there is no real consensus. Any serious user must use their knowledge of context to mediate between the theoretical and the empirical. Fortunately, this is exactly what historians are trained to do.

Let's run the age of first taking office query we used above, [but use "head of government" instead of "head of state"](https://query.wikidata.org/#%23%20Age%20of%20heads%20of%20government%20upon%20taking%20power%2C%201950-1980%0ASELECT%20DISTINCT%20%3FheadOfGovt%20%3FheadOfGovtLabel%20%28MIN%28%3Fage%29%20as%20%3FageMin%29%20%3FcountryLabel%20%3FpositionLabel%0A%7B%0A%20%23%20find%20heads%20of%20state%20positions%0A%20hint%3AQuery%20hint%3Aoptimizer%20%22None%22.%0A%20%3Fposition%20wdt%3AP279%2a%20wd%3AQ2285706%20.%0A%0A%20%23%20sovereign%20states%20only%0A%20%3Fposition%20wdt%3AP1001%20%3Fcountry%20.%0A%20%3Fcountry%20wdt%3AP31%20wd%3AQ3624078%20.%0A%20%20%0A%20%23%20fetch%20names%20of%20officeholders%0A%20%3FheadOfGovt%20wdt%3AP39%20%3Fposition%20.%0A%0A%20%23%20birthdates%20of%20officeholders%0A%20%3FheadOfGovt%20wdt%3AP569%20%3Fdob.%20hint%3APrior%20hint%3ArangeSafe%20true.%20%0A%20%20%0A%20%23%20date%20of%20term%20start%0A%20%3FheadOfGovt%20p%3AP39%20%3Fstatement%20.%0A%20%3Fstatement%20ps%3AP39%20%3Fposition%20.%20%0A%20%3Fstatement%20pq%3AP580%20%3FtermStart.%20hint%3APrior%20hint%3ArangeSafe%20true.%0A%20%20%0A%20%23%201950-1980%20term%20start%20only%0A%20FILTER%28%221950-01-01%22%5E%5Exsd%3AdateTime%20%3C%3D%20%3FtermStart%20%26%26%20%3FtermStart%20%3C%20%221980-01-01%22%5E%5Exsd%3AdateTime%29%0A%20%0A%20%23%20calcuate%20age%20%0A%20BIND%28YEAR%28%3FtermStart%29-YEAR%28%3Fdob%29%20as%20%3Fage%29%0A%20SERVICE%20wikibase%3Alabel%20%7B%20bd%3AserviceParam%20wikibase%3Alanguage%20%22en%22%20%7D%0A%7D%0AGROUP%20BY%20%3FheadOfGovt%20%3FheadOfGovtLabel%20%3FcountryLabel%20%3FpositionLabel%0AORDER%20BY%20DESC%28%3FageMin%29). This query returns Abdel Nasser's age when he became prime minister rather than president. But this query accurately captures other leaders who are not returned with the "head of state" query. For instance, it returns prime ministers in states (like India?) where the president is largely ceremonial. But it also includes some mayors.

Historians tend to be sceptical of taxonomic schemes, with good reason. But explicit taxonomies can do great work for us as a means to discover and explore information. No doubt you will find (what you consider to be) errors in this taxonomy. You can certainly "correct" those errors–Wikidata is open–but don't be too hasty. The taxonomies already existing in Wikidata are organic and collectively produced, and they are not exclusive. There are ways to work around the parts you don't agree with–don't change the knowledge base itself until you have learned how to do this.

A big part of our expertise as historians is contextualizing details. For many of us, that's the fun and fascinating work. And that skill is precisely what a researcher needs to interact with the galaxy of isolated factoids in Wikidata. The rest of this lesson shows a few of the main ways to do that.

# Querying Wikidata

There are no bad questions in Wikidata–only unrealistic questions. The knowledge base should not be expected to provide comprehensive answers to any questions. But it does better with some questions than others.

As in most historical domains, the data trail of dead white men is relatively better represented. Want a list of colleges attended by governors of American states? [This query](https://query.wikidata.org/#%23%20colleges%20attended%20by%20governors%20of%20US%20states%0ASELECT%20%3FcollegeLabel%20%28COUNT%28%3FcollegeLabel%29%20AS%20%3Fcount%29%20%0AWHERE%0A%7B%20%23%20define%20office%20of%20US%20governor%0A%20%20%3Foffice%20wdt%3AP279%2a%20wd%3AQ889821.%0A%20%20%0A%20%20%23%20find%20officeholders%0A%20%20%3Fgovernor%20wdt%3AP39%20%3Foffice.%0A%20%20%0A%20%20%23%20find%20college%0A%20%20%3Fgovernor%20wdt%3AP69%20%3Fcollege.%0A%20%20SERVICE%20wikibase%3Alabel%20%7B%20bd%3AserviceParam%20wikibase%3Alanguage%20%22%5BAUTO_LANGUAGE%5D%2Cen%22.%20%7D%0A%7D%0AGROUP%20BY%20%3FcollegeLabel%20%0AORDER%20BY%20DESC%28%3Fcount%29) is probably about right (don't forget to press play). Want a list of colleges attended by governors of Chinese provinces? [This query](https://query.wikidata.org/#%23%20colleges%20attended%20by%20governors%20of%20Chinese%20provinces%0ASELECT%20%3FcollegeLabel%20%28COUNT%28%3FcollegeLabel%29%20AS%20%3Fcount%29%20%0AWHERE%0A%7B%20%23%20define%20office%20of%20governor%20of%20Chinese%20provinces%0A%20%20%3Foffice%20wdt%3AP279%2a%20wd%3AQ1540323.%0A%20%20%0A%20%20%23%20find%20officeholders%0A%20%20%3Fgovernor%20wdt%3AP39%20%3Foffice.%0A%20%20%0A%20%20%23%20find%20college%0A%20%20%3Fgovernor%20wdt%3AP69%20%3Fcollege.%0A%20%20SERVICE%20wikibase%3Alabel%20%7B%20bd%3AserviceParam%20wikibase%3Alanguage%20%22%5BAUTO_LANGUAGE%5D%2Cen%22.%20%7D%0A%7D%0AGROUP%20BY%20%3FcollegeLabel%20%0AORDER%20BY%20DESC%28%3Fcount%29) yields less-than-convincing results.

Generally speaking, questions about specifics, like dates and locations and labels, yield better answers from Wikidata. For example, here's a [map of the birthplaces of governors of American states, color coded by half-century of birth](https://query.wikidata.org/#%23%20map%20birthplaces%20of%20governors%20of%20US%20states%2C%20grouped%20by%20half-century%0A%23defaultView%3AMap%0ASELECT%20%3FgovernorLabel%20%3FbirthplaceLabel%20%3Fcoord%20%3Fdob%20%3Fbirthyear%20%3Flayer%0AWHERE%0A%7B%20%23%20define%20office%20of%20US%20governor%0A%20%20%3Foffice%20wdt%3AP279%2a%20wd%3AQ889821.%0A%20%20%0A%20%20%23%20find%20officeholders%0A%20%20%3Fgovernor%20wdt%3AP39%20%3Foffice.%0A%20%20%0A%20%20%23%20find%20birthplace%20and%20birthdate%0A%20%20%3Fgovernor%20wdt%3AP19%20%3Fbirthplace%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20wdt%3AP569%20%3Fdob.%0A%20%20%0A%20%20%23%20geocoordinates%20of%20birthplace%0A%20%20%3Fbirthplace%20wdt%3AP625%20%3Fcoord.%0A%20%20%0A%20%20%23%20group%20birth%20years%0A%20%20BIND%28YEAR%28%3Fdob%29%20as%20%3Fbirthyear%29%0A%20%20BIND%28IF%28%20%28%3Fbirthyear%20%3C%201700%29%2C%20%22Pre-1700%22%2C%20IF%28%28%3Fbirthyear%20%3C%201751%29%2C%20%221700-1750%22%2C%20IF%28%28%3Fbirthyear%20%3C%201801%29%2C%20%221751-1800%22%2C%20IF%28%28%3Fbirthyear%20%3C%201851%29%2C%20%221801-1850%22%2C%20IF%28%28%3Fbirthyear%20%3C%201901%29%2C%20%221851-1900%22%2C%20IF%28%28%3Fbirthyear%20%3C%201951%29%2C%20%221901-1950%22%2C%20%22Post-1950%22%29%20%29%20%29%20%29%20%29%29%20AS%20%3Flayer%20%29%0A%20%20%0A%20%20SERVICE%20wikibase%3Alabel%20%7B%20bd%3AserviceParam%20wikibase%3Alanguage%20%22%5BAUTO_LANGUAGE%5D%2Cen%22.%20%7D%0A%7D%0A). And here's a [list of the names of governors of American states in Russian and Hebrew transliteration](https://query.wikidata.org/#%23%20names%20of%20governors%20of%20US%20states%20in%20Russian%20and%20Hebrew%20transliterations%0ASELECT%20%3Fgovernor%20%3FgovernorLabel%20%3FgovernorLabel_ru%20%3FgovernorLabel_he%0AWHERE%0A%7B%20%23%20define%20office%20of%20US%20governor%0A%20%20%3Foffice%20wdt%3AP279%2a%20wd%3AQ889821.%0A%20%20%0A%20%20%23%20find%20officeholders%0A%20%20%3Fgovernor%20wdt%3AP39%20%3Foffice.%0A%20%20%20%0A%20%20%23%20Russian%20transliteration%0A%20%20OPTIONAL%7B%3Fgovernor%20rdfs%3Alabel%20%3FgovernorLabel_ru%20FILTER%28LANG%28%3FgovernorLabel_ru%29%20%3D%20%22ru%22%29%7D%0A%20%20%0A%20%20%23%20Hebrew%20transliteration%0A%20%20OPTIONAL%7B%3Fgovernor%20rdfs%3Alabel%20%3FgovernorLabel_he%20FILTER%28LANG%28%3FgovernorLabel_he%29%20%3D%20%22he%22%29%7D%0A%20%20%20%20%0A%20%20SERVICE%20wikibase%3Alabel%20%7B%20bd%3AserviceParam%20wikibase%3Alanguage%20%22%5BAUTO_LANGUAGE%5D%2Cen%22.%20%7D%0A%7D%0A). Our internet search engine habits of searching via keyword and string, on the other hand, does not play to Wikidata's strengths. 

It takes a while to get the hang of the [Wikidata query service](https://query.wikidata.org/). But playing around with it is one of the best ways to learn about Wikidata–and about linked data in general. The best way to query this service is using the SPARQL query language, which is [introduced in another (currently retired) Programming Historian lesson](https://programminghistorian.org/en/lessons/retired/graph-databases-and-SPARQL). SPARQL is incredibly powerful, but it takes some learning. Wikidata's own [SPARQL tutorial](https://www.wikidata.org/wiki/Wikidata:SPARQL_tutorial) is very good, but query shortcuts built into Wikidata offer an even more friendly introductory path for new users. 

### Query shortcut I: Wikidata Query Builder 
Second, Wikidata offers a [graphic query builder interface](https://query.wikidata.org/querybuilder/). This can't do all of things that SPARQL can do, but it can set up a basic structure for your queries. 

Let's try a variant of the US governor college query we tried earlier. 

### Query shortcut II: Example SPARQL queries
Wikidata offers a [long list of example queries](https://www.wikidata.org/wiki/Wikidata:SPARQL_query_service/queries/examples#Most_popular_subjects_of_scientific_articles). All of these queries can be adapted for your own interests, by substituting the item you want for the item the query contains. 

Open the [query service](https://query.wikidata.org/), then click on "Examples," then load an example.

![Figure 4: Cats example query](cats-query-example.png)

#### Example A: Cats
Let's start with the first example query listed: Cats. 

![Figure 5: Cats query](cats-substitute.png)

There are six colors of text:
- grey for comments (which start with hashtags)
- red for operators
- black for punctuation
- orange for variables
- green for wildcards ?
- blue for Wikidata items (Q-numbers) and properties (P-numbers)

When you float your cursor over a blue-text item or property, a pop-up will give you its label and description. The cats example is the simplest query: it returns every `?item` that is an "instance of" (`wdt:P31`) a "cat" (`wd:Q146`). By changing the last Q-number, we can search for all instances of something else. For example, try changing `Q146` to `Q3024240`. Float your cursor over this new item to see what it is, then execute the query and skim the results.

***Insight***: the "instance of" property ([P31](https://www.wikidata.org/wiki/Property:P31)) does a huge amount of work in Wikidata and similar data structures. In simple English, line 5 of the query (`?item wdt:P31 wd:Q146`) could be read as "This item is a cat." You will see P31 everywhere in Wikidata. Just as often, you will see [P279](https://www.wikidata.org/wiki/Property:P279), the "subclass of" property. Try changing line 5 to `?item wdt:P279 wd:Q146`, which could be read as "This item is a *kind of* cat." The query yields different results. What's the takeaway? These two properties are the most common properties in Wikidata and they matter a great deal, but their function won't be immediately apparent.

#### Example B: Humans by death date
Let's try adapting another example query. A bit further down the list of examples of simple queries, just below cats, is "Humans who died on a specific date on the English Wikipedia, ordered by label." Click on this one. It's preloaded with August 25, 2001, but you can change the date (in orange) from `"+2001-08-25"` to any other date. Try your birthdate. In the results, the `?sl` column on the right counts the pages that Wikipedia and its sister projects hold on the individual.

***Insight***: Orange text in the Wikidata query service contains variables, like dates and languages, that you can modify for your own purposes.

#### Example C: Popular names
One more example of adapting an existing query? Load the query "[Popular names per birthplace](https://query.wikidata.org/#%23Popular%20names%20per%20birthplace%0A%23defaultView%3ABubbleChart%0ASELECT%20%3Fcid%20%3Ffirstname%20%28COUNT%28%2a%29%20AS%20%3Fcount%29%0AWHERE%0A%7B%0A%20%20%3Fpid%20wdt%3AP19%20wd%3AQ64.%0A%20%20%3Fpid%20wdt%3AP735%20%3Fcid.%0A%20%20OPTIONAL%20%7B%0A%20%20%20%20%3Fcid%20rdfs%3Alabel%20%3Ffirstname%0A%20%20%20%20FILTER%28%28LANG%28%3Ffirstname%29%29%20%3D%20%22en%22%29%0A%20%20%7D%0A%7D%0AGROUP%20BY%20%3Fcid%20%3Ffirstname%0AORDER%20BY%20DESC%28%3Fcount%29%20%3Ffirstname%0ALIMIT%2050)." It's set for a certain city–see if you can figure out which one (hint: float your cursor over the various Q-numbers). As historians, we may be interested in popular names of the past. Adding a couple of lines to the query will filter this name list by date. On line 12, before the curly bracket, add these two lines, which return the birthdates of persons named and filter them by a date range:

```sparql
  ?pid wdt:P569 ?dob.
  FILTER("1800-01-01"^^xsd:dateTime <= ?dob && ?dob < "1900-01-01"^^xsd:dateTime)
```

You could also set the query to a city of interest to you, by changing city the Q-number you found earlier. If you delete that number (but not the `wd:`) and press `control + space` and begin to type the name of the city you choose, the query interface will autofill the Q-number.

***Insight***: We modified this query by adding a statement using the "date of birth" property ([P569](https://www.wikidata.org/wiki/Property:P569)), then filtering the results by date. Adding lines to a SPARQL query is quite a bit more complicated than swapping one Q-number or P-number for another, however. For the time being, browsing the examples for a query that is already structured correctly and substituting numbers. 

## other


Most historians have some background in postcolonial theory, and can readily identify the value in distinguishing between the signifier and the signified. In the wikidata knowledge base, the signfied are only ever numbers; everything else is a signifier.

P-number is property



# Contributing to Wikidata

Socially constructed semantics. You need to find out what properties and items people are already using to describe what you wish to search. This might not be the "right" concepts, according to your view, but you need to be practical.

Like good historians, we will want to look at any object of knowledge from a few different angles. Wikidata items are no exception. We can look at its history: who has edited this page, when, and why? We can look at its use in wikidata: in the left hand menu, there's a "what links here" field. If you click on it, you will see a list of the hundreds of items in Wikidata that use "head of state" in some way or another. This particular list is pretty bewildering, but looking at "what links here" for more obscure items can be quite instructive. For example, [Abdel Nasser's birthplace]


Every Wikipedia page links to a wikidata item

Login

See other languages

Add reference

Add item: must have instance of. Imitate another of its type.

Add birthplace?

Batch editing: subject of another lesson.

## Endnotes
[^1]: Rosenzweig, Roy. “Can History Be Open Source? Wikipedia and the Future of the Past.” *Journal of American History* 93, no. 1 (June 1, 2006): 117–46. https://doi.org/10.2307/4486062.
