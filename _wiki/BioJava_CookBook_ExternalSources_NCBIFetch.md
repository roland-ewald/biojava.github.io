---
title: BioJava:CookBook:ExternalSources:NCBIFetch
permalink: wiki/BioJava%3ACookBook%3AExternalSources%3ANCBIFetch
---

How do I get a sequence from NCBI?
----------------------------------

Besides building your very own database-driven sequence repository, most
users will need to fetch sequences from public datasources. A primary
source of sequence information is [NCBI](http://www.ncbi.nlm.nih.gov).
From its very beginning, Biojava was able to get sequences from NCBI
with wrapper objects and methods. Most recently, the implementation of
the Biojavax extension brought forth some changes (for example,
namespaces) and the corresponding objects and methods were modified
accordingly.

This example is a very simple starting point for any user who wants to
get sequence info. However, beware that NCBI is looking over your
shoulder and might limit your access if you are too greedy of their
bandwith. Do not use these objects/methods to build a mirror copy of
GenBank...

```java import org.biojava.bio.BioException; import
org.biojava.bio.symbol.SymbolList; import
org.biojavax.bio.db.ncbi.GenbankRichSequenceDB; import
org.biojavax.bio.seq.RichSequence;

public class NCBIFileReader {

`  public static void main(String[] args) {`  
`       `  
`     RichSequence rs = null;`  
`       `  
`     GenbankRichSequenceDB grsdb = new GenbankRichSequenceDB();`  
`     try{`  
`   // Demonstration of use with GenBank accession number`  
`   rs = grsdb.getRichSequence("M98343");`  
`   System.out.println(rs.getName()+" | "+rs.getDescription());`  
`   SymbolList sl = rs.getInternalSymbolList();`  
`   System.out.println(sl.seqString());`  
`           `  
`   // Demonstration of use with GenBank GI`  
`   rs = grsdb.getRichSequence("182086");           `  
`   System.out.println(rs.getName()+" | "+rs.getDescription());`  
`   sl = rs.getInternalSymbolList();`  
`   System.out.println(sl.seqString());`

`     }`  
`     catch(BioException be){`  
`   be.printStackTrace();`  
`   System.exit(-1);`  
`     }`  
`  }`

} ```
