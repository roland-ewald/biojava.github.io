---
title: BioJava:CookBookItaliano:SeqIO:ReadFasta
---

Come posso leggere una sequenze da un file in formato FASTA?
------------------------------------------------------------

Una dei delle operazioni di I/O più eseguite in bioninformatica è il
caricamento di un flat file in memoria. One of the most frequent I/O
tasks is the reading of a flat file representation of sequence into
memory. SeqIOTools provides some basic static methods to read files into
BioJava. There is actually more than one solution. The more specific is
demonstrated first and the more general second.

### Solution 1

<java> import java.io.\*; import java.util.\*;

import org.biojava.bio.\*; import org.biojava.bio.seq.db.\*; import
org.biojava.bio.seq.io.\*; import org.biojava.bio.symbol.\*;

public class ReadFasta {

` /**`  
`  * The program takes two args: the first is the file name of the Fasta file.`  
`  * The second is the name of the Alphabet. Acceptable names are DNA RNA or PROTEIN.`  
`  */`  
` public static void main(String[] args) {`

`   try {`  
`     //setup file input`  
`     String filename = args[0];`  
`     BufferedInputStream is =`  
`         new BufferedInputStream(new FileInputStream(filename));`

`     //get the appropriate Alphabet`  
`     Alphabet alpha = AlphabetManager.alphabetForName(args[1]);`

`     //get a SequenceDB of all sequences in the file`  
`     SequenceDB db = SeqIOTools.readFasta(is, alpha);`  
`   }`  
`   catch (BioException ex) {`  
`     //not in fasta format or wrong alphabet`  
`     ex.printStackTrace();`  
`   }catch (NoSuchElementException ex) {`  
`     //no fasta sequences in the file`  
`     ex.printStackTrace();`  
`   }catch (FileNotFoundException ex) {`  
`     //problem reading file`  
`     ex.printStackTrace();`  
`   }`  
` }`

} </java>

### Solution 2

<java> import java.io.\*;

import org.biojava.bio.\*; import org.biojava.bio.seq.\*; import
org.biojava.bio.seq.io.\*;

public class ReadFasta2 {

` /**`  
`  * This program will read any file supported by SeqIOTools it takes three`  
`  * arguments, the first is the file name the second is the name of`  
`  * a file format supported by SeqIOTools. eg fasta, genbank etc.`  
`  * The third argument is the alphabet (eg dna, rna, protein).`  
`  *`  
`  * Both the format and alphabet names are case insensitive.`  
`  *`  
`  */`  
` public static void main(String[] args) {`  
`   try {`  
`     //prepare a BufferedReader for file io`  
`     BufferedReader br = new BufferedReader(new FileReader(args[0]));`

`     String format = args[1];`  
`     String alphabet = args[2];`

`     /*`  
`      * get a Sequence Iterator over all the sequences in the file.`  
`      * SeqIOTools.fileToBiojava() returns an Object. If the file read`  
`      * is an alignment format like MSF and Alignment object is returned`  
`      * otherwise a SequenceIterator is returned.`  
`      */`  
`     SequenceIterator iter =`  
`         (SequenceIterator)SeqIOTools.fileToBiojava(format,alphabet, br);`  
`   }`  
`   catch (FileNotFoundException ex) {`  
`     //can't find file specified by args[0]`  
`     ex.printStackTrace();`  
`   }catch (BioException ex) {`  
`     //error parsing requested format`  
`     ex.printStackTrace();`  
`   }`  
` }`

} </java>