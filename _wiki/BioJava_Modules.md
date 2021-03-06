---
title: BioJava:Modules
permalink: wiki/BioJava%3AModules
---

BioJava Modules
===============

The following list of modules for BioJava have been defined and the
following people have stepped up to become module leader:

BioJava 3.0.X
-------------

Module: biojava3-core Lead: Scooter Willis

Module: biojava3-structure Lead: Andreas Prlic

Module: biojava3-alignment: Lead: Mark Chapman

Module: biojava3-modfinder: Lead: Jianjiong Gao

Module: biojava3-phylo: Lead: Scooter Willis

Module: biojava3-genome: Lead: Andy Yates

Module: biojava3-ws: Lead: Sylvain Foisy

Module: biojava3-aa-prop: Lead: Chuan Hock Koh

Module: biojava3-protein-disorder: Lead: Peter Troshin

If you are looking for a BioJava related project, consider contributing
one of the missing [Feature
Requests](/wiki/BioJava3_Feature_Requests "wikilink").

There are also a number of algorithms where we would be interested in
[Java ports](Algorithm_Java_port "wikilink").

Legacy BioJava 1.8
------------------

Module: biojava-sequence Lead: Richard Holland

`- Bring in Richard's new code that he started to develop on the biojava-3 branch.`  
`- provide a more scaleable and efficient basis for dealing with large sequence files`  
`- consider implementation based on ParallelArray from JSR166 (extra166y package, see `[`http://gee.cs.oswego.edu/dl/concurrency-interest/`](http://gee.cs.oswego.edu/dl/concurrency-interest/)`)`  
`- consider implementation that supports MapReduce as in Apache Hadoop (http://hadoop.apache.org/)`

Module: biojava-alignment Lead: Andreas Dräger

`- refactoring of underlying data structures`  
`- allow better access to underlying dynamic programming data structures`  
`- allow more customizable display of pairwise alignments (HTML/plain text, etc)`

Module : biojava-blast Lead: Mark Schreiber

`- provide access to all details of the blast output`  
`- add support for RPS blast`

Module: biojava-phylo Lead: Scooter Willis

`- provide improved NJtree /Jalview`  

Module: biojava-biosql Lead: Richard Holland

`- merge the new biojava-sequence module with the current biojava-biosql code `  
`- Mark Schreiber wants to work on BioSQL/ JPA bindings`

Module: biojava-das : Lead: Jonathan Warren

`- probably deprecate the old DAS code in BJ and replace it with the up to date Dasobert library`  
`- update dasobert code to 1.6 and make smaller`  
`- add further support for getting new information contained in the registry (validation, on the fly validation, sources by types and cvId).`

Module: biojava-structure Lead: Andreas Prlic

`- add secondary structure assignment`  
`- better integration with 3D viewers (Jmol, RCSB viewers)`

Module: biojava-web services:

`- The details seem still to be under discussion and perhaps we need multiple modules here?`  
`- also what about REST vs. SOAP? To be discussed. People who expressed interest are:`  
`- Niall Haslam,Scooter Willis, Sylvain Foisy`

Module?: biojava-ws-blast

Module?: biojava-ws-biolit

Proposed Module: biojava-j2ee Lead: Mark Schreiber

`- This would probably take the form of SessionBeans and WebServices that can be deployed to Glassfish/ JBoss etc to provide biological services for people who want to make client server or SOA apps.`
