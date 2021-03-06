---
title: BioJava:CookBookItaliano:Alphabets
permalink: wiki/BioJava%3ACookBookItaliano%3AAlphabets
---

Come posso ottenere l'alfabeto del DNA, dell'RNA o Proteico?
------------------------------------------------------------

In BioJava gli
[Alfabeti](http://www.biojava.org/docs/api14/org/biojava/bio/symbol/Alphabet.html)
sono collezioni di
[Simboli](http://www.biojava.org/docs/api14/org/biojava/bio/symbol/Symbol.html).
I più comuni alfabeti biologici ([DNA](wp:DNA "wikilink"),
[RNA](wp:RNA "wikilink"), [protein](wp:protein "wikilink"), etc) sono
memorizzati tramite il BioJava
[AlphabetManager](http://www.biojava.org/docs/api14/org/biojava/bio/symbol/AlphabetManager.html)
all'avvio e vi si può accedere tramite il nome.

The [DNA](wp:DNA "wikilink"), [RNA](wp:RNA "wikilink") and
[protein](wp:protein "wikilink") alphabets can also be accessed using
convenient static methods from
[DNATools](http://www.biojava.org/docs/api14/org/biojava/bio/seq/DNATools.html),
[RNATools](http://www.biojava.org/docs/api14/org/biojava/bio/seq/RNATools.html)
and
[ProteinTools](http://www.biojava.org/docs/api14/org/biojava/bio/seq/ProteinTools.html)
respectively.

Both of these approaches are shown in the example below

```java import org.biojava.bio.symbol.\*; import java.util.\*; import
org.biojava.bio.seq.\*;

public class AlphabetExample {

` public static void main(String[] args) {`  
`   Alphabet dna, rna, prot, proteinterm ;`

`   //get the DNA alphabet by name`  
`   dna = AlphabetManager.alphabetForName("DNA");`

`   //get the RNA alphabet by name`  
`   rna = AlphabetManager.alphabetForName("RNA");`

`   //get the Protein alphabet by name`  
`   prot = AlphabetManager.alphabetForName("PROTEIN");`  
`   //get the protein alphabet that includes the * termination Symbol`  
`   prot = AlphabetManager.alphabetForName("PROTEIN-TERM");`

`   //get those same Alphabets from the Tools classes`  
`   dna = DNATools.getDNA();`  
`   rna = RNATools.getRNA();`  
`   prot = ProteinTools.getAlphabet();`  
`   //or the one with the * symbol`  
`   proteinterm  = ProteinTools.getTAlphabet();`

` }`

} ```
