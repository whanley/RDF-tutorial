---
title: Making a Small RDF Database, first try
authors:
- Will Hanley
date: 2015-11-16
layout: default
---

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
