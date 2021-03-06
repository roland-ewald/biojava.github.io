---
title: BioJava:Performance:ReverseComplement
permalink: wiki/BioJava%3APerformance%3AReverseComplement
---

Reverse complement of DNA sequence
==================================

This source code is run in the [example that determines the reverse
complement](/wiki/BioJava:Performance "wikilink") of a DNA sequence.

```java import org.biojava.bio.seq.impl.RevCompSequence; import
org.biojavax.bio.seq.RichSequence; import
org.biojavax.bio.seq.RichSequenceIterator; import
org.biojavax.bio.seq.io.FastaFormat; import
org.biojavax.bio.seq.io.FastaHeader;

import java.io.BufferedReader; import java.io.FileReader;

public class RevComp {

`   public static void main(String[] args) throws Exception {`  
`   `  
`       String fastaLocation;`  
`       if(args.length > 0) {`  
`           fastaLocation = args[0];`  
`       }`  
`       else {`  
`           fastaLocation = "data/revcomp/input.fasta";`  
`       }`  
`       `  
`       long time = System.currentTimeMillis();`  
`       `  
`       FastaHeader fastaHeader = new FastaHeader();`  
`       fastaHeader.setShowAccession(true);`  
`       fastaHeader.setShowDescription(false);`  
`       fastaHeader.setShowIdentifier(false);`  
`       fastaHeader.setShowName(false);`  
`       fastaHeader.setShowNamespace(false);`  
`       fastaHeader.setShowVersion(false);`

`       FastaFormat fastaFormat = new FastaFormat();`  
`       fastaFormat.setHeader(fastaHeader);`  
`       fastaFormat.setLineWidth(60);`

`       BufferedReader br = new BufferedReader(new FileReader(fastaLocation));`  
`       RichSequenceIterator iter = RichSequence.IOTools.readFastaDNA(br, null);`  
`       while(iter.hasNext()) {`  
`           RichSequence seq = iter.nextRichSequence();`  
`           RevCompSequence rev = new RevCompSequence(seq);`  
`           rev.setName(seq.getAccession()+" "+seq.getDescription());`  
`           fastaFormat.writeSequence(rev, System.out);`  
`       }`  
`       `  
`       long finalTime = System.currentTimeMillis();`  
`       System.out.println(finalTime-time+" ms");`  
`   }`

}

```
