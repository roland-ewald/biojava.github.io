---
title: SVNmigrationHistory
---

back to [CVS\_to\_SVN\_Migration](CVS_to_SVN_Migration "wikilink")

### Steps during the migration process

A) Test the migration procedure to ensure nothing gets lost

B) We will declare a CVS freeze at a certain date (giving you enough
time to commit all your latest changes to CVS). At this point we will
also do a quick BioJava release (version 1.5.1)

C) After the freeze the final svn migration will happen

D) from that moment on all future Biojava development will happen via
svn, CVS will remain frozen.

Progress Status
---------------

### CVS freeze

The freeze of the CVS has been declared for **Wednesday December 12th,
2007**. Please commit any code to CVS before that date.

In the days after the repository will be migrated to subversion (SVN) .
From then on all future development will be happening in the new SVN
repository. All code (+ history) will be available via SVN.

After the CVS freeze we will also do a (minor) BioJava release. This
will be BioJava version 1.5.1

### Testing the migration procedure

The testing phase has finished.

This procedure will be used for the conversion: (a hack of cvs2svn)
[1](http://hoopajoo.net/misc/converting_to_svn.html)

Thanks to George Hartzell for doing the actual conversion.

To test the svn dump [statsvn](http://www.statsvn.org/) is being run on
the whole repository. This allows to check if the history has been
converted correctly and if there are any problems within the repository.

#### Problems found during testing phase

A few problems have been found:

##### Attic files

These six files caused problems:

      ERROR: A CVS repository cannot contain both biojava/biojava-live/demos/ssaha/ResultPrinter.java,v and biojava/biojava-live/demos/ssaha/Attic/ResultPrinter.java,v
      ERROR: A CVS repository cannot contain both biojava/biojava-live/demos/ssaha/SSAHASeq.java,v and biojava/biojava-live/demos/ssaha/Attic/SSAHASeq.java,v
      ERROR: A CVS repository cannot contain both biojava/biojava-live/demos/ssaha/CreateDNAFastaHashTable.java,v and biojava/biojava-live/demos/ssaha/Attic/CreateDNAFastaHashTable.java,v
      ERROR: A CVS repository cannot contain both biojava/biojava-live/demos/ssaha/SSAHA.java,v and biojava/biojava-live/demos/ssaha/Attic/SSAHA.java,v
      ERROR: A CVS repository cannot contain both biojava/biojava-live/demos/ssaha/CreateEmblHashTable.java,v and biojava/biojava-live/demos/ssaha/Attic/CreateEmblHashTable.java,v
      ERROR: A CVS repository cannot contain both
      biojava/biojava-live/src/org/biojava/bio/annodb/IndexedAnnotationDB.java,v
      and
      biojava/biojava-live/src/org/biojava/bio/annodb/Attic/IndexedAnnotationDB.java,v

Here's a reference that discusses the issue:

` `[`http://cvs2svn.tigris.org/faq.html#atticprob`](http://cvs2svn.tigris.org/faq.html#atticprob)

To resolve this problems the files in the Attic subdirectory are getting
removed and not being imported to SVN

##### Upper case / lower case problems

There are a few files in the repository that have a upper case / lower
case file name clash with another file. To resolve this they are going
to be renamed.

    /biojava-live/branches/mattandtom/src/org/biocorba 
    /biojava-live/branches/mattandtom/src/org/biojava/bridge/biocorba 
    /biojava-draft/biojava-live/tags/biojava/src/org/biocorba 
    /biojava-live/tags/biojava/src/org/biojava/bridge/biocorba

##### Escape characters in log statements

svn log -v --xml gives this error message:

svn log: An invalid XML character (Unicode: 0x1b) was found in the
element content of the document.

this seems to be a bug in SVN:
<http://subversion.tigris.org/issues/show_bug.cgi?id=2866>

after manually removing the 2 escape characters from the log statements
svn log -v --xml works fine.
