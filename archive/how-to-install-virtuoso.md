There are various triplestores available. One of the most widely used is [Virtuoso Open-Source](http://virtuoso.openlinksw.com/dataspace/doc/dav/wiki/Main/). Getting Virtuoso up and running on your own computer can be a bit daunting, but there are lots of tips and shortcuts available. Remember, this is a *small* database that you're building. Most of the complexity in triplestores derives from large scale, data serving, and security demands. We're just trying to build a simple dataset for our own use, so we don't have to get everything right.

For Linux, follow Virtuoso's installation instructions for [your blend](http://virtuoso.openlinksw.com/dataspace/doc/dav/wiki/Main/VOSBuild#Building%20for%20Linux%20or%20other%20Unix-like%20OS) 

You'll have to move the virtuoso.ini file from /etc/virtuoso into the /var/lib/virtuoso/db folder, then execute:

```$ sudo virtuoso-t -f virtuoso.ini```

For Mac, install homebrew (see instructions in the [Jekyll tutorial](http://programminghistorian.org/lessons/building-static-sites-with-jekyll-github-pages#command-line-tools-suite-a-idsection2-1a)), then execute:

```$ brew install virtuoso``` 

For Windows, download the [binary](http://virtuoso.openlinksw.com/dataspace/doc/dav/wiki/Main/VOSDownload#Pre-built%20binaries%20for%20Windows) and extract it to somewhere such as c:\virtuoso. Then execute from shell:

```$ cd c:\virtuoso\database
$ virtuoso-t -f virtuoso.ini```