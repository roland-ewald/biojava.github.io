---
title: BioJava:Cookbook:SeqIO:Echo
permalink: wiki/BioJava%3ACookbook%3ASeqIO%3AEcho
---

How does sequence I/O work in biojava?
--------------------------------------

Most sequence databases present sequences in some kind of flat file
format such as the EMBL or Fasta formats. Biojava can read a number of
these and convert them into Sequence objects. The SeqIOTools class
provides many static methods that do this for you. In most cases this is
great but you may want to write a parser for a format that is not
supported by biojava. Alternatively, you might not want to generate
Sequence objects. For example if you only wanted the names of all the
sequences in a very large file it would be horribly inefficient to make
them all into full Sequence objects only to call getName() on them and
send them off to the Garbage Collector. If you did this for the entire
nr set of GenBank you could be twiddling your thumbs for ages while the
parser assembles all the features, symbollists and annotations. Using
biojava's sequence I/O API it is possible to make your own parsers. It
is also possible to plug-in your own components with the already
existing parsers to generate a highly customized parsing architecture.

The core of the API are the two interfaces SequenceFormat and
SeqIOListener. The API is heavily based on the event/ call-back model.
Conceptually, an implementation of SequenceFormat knows how to read (and
write) a sequence file of some format. When it reads the file it emits
events based on what it is seeing in the file. The events are passed to
an implementation of SeqIOListener. The SequenceFormat makes callbacks
to the methods of the SeqIOListener. The SequenceFormat also makes use
of a SymbolTokenizer that translates sequence in text based characters
to biojava Symbols

The opportunity for customization really comes in with the
implementation of SeqIOListener. The biojava javadocs show that there
are several implementations of this interface. One obvious thing a
SeqIOListener can do it make a biojava Sequence object. Another thing it
could do would be to 'tee' the events it is recieving to two or more
registered SeqIOListeners that can each do their own thing. The listener
could ignore all the events it is not interested in and pass on to other
listeners a select few events, effectively making it a filter. You could
also filter entire entries passing on ones that meet a certain criteria
to a SequenceBuilder, for example maybe you have a huge file but are
only interested in those records with a certain keyword or those from a
certain species. The listener can even modify events before parsing them
on. This might be useful if you want to add extra information to the
Sequences you are building. If you have the problem described above and
only want to extract the names you could implement a listener that only
has functional code in the setName(String name) method does nothing with
all the other events.

The example below is a class that echos IO events to STDOUT. This class
is useful to see what is happening as a file is being read. It would
also be helpful if you wanted to debug a SequenceFormat class and make
sure it is emitting the correct events at the right time. It would also
help you to write a custom SeqIOListener by showing you which events you
need to block/ listen-for / modify.

### SeqIOEcho.java

```java /\*

`* SeqIOEcho.java`  
`*`  
`* Created on May 10, 2005, 2:39 PM`  
`*/`

import java.io.BufferedReader; import java.io.FileReader; import
java.util.Iterator; import org.biojava.bio.Annotation; import
org.biojava.bio.seq.Feature; import
org.biojava.bio.seq.io.SeqIOListener; import
org.biojava.bio.seq.io.SequenceFormat; import
org.biojava.bio.seq.io.SymbolTokenization; import
org.biojava.bio.symbol.Alphabet; import
org.biojava.bio.symbol.AlphabetManager; import
org.biojava.bio.symbol.SimpleSymbolList; import
org.biojava.bio.symbol.Symbol;

/\*\*

`* A SeqIOListener that reports events being emitted by a format object`  
`* @author Mark Schreiber`  
`*/`

public class SeqIOEcho implements SeqIOListener {

`   int tab = 0;`  
`   `  
`   `  
`   /** Creates a new instance of SeqIOEcho */`  
`   public SeqIOEcho() {`  
`       `  
`   }`

`   public void setURI(String uri) {`  
`       System.out.println(tabOut()+"Call to setURI(String uri)");`  
`       tab++;`  
`       System.out.println(tabOut()+"uri: "+uri);`  
`       tab--;`  
`   }`

`   public void setName(String name) {`  
`       System.out.println(tabOut()+"Call to setName(String name)");`  
`       tab++;`  
`       System.out.println(tabOut()+"name: "+name);`  
`       tab--;`  
`   }`

`   public void startFeature(Feature.Template templ){`  
`       tab++;`  
`       System.out.println(tabOut()+"Call to startFeature(Feature.Template templ)");`  
`       tab++;`  
`       System.out.println(tabOut()+"type: "+templ.type);`  
`       System.out.println(tabOut()+"source: "+templ.source);`  
`       System.out.println(tabOut()+"location: "+templ.location);`  
`       tab--;`  
`   }`

`   public void addSymbols(Alphabet alpha, Symbol[] syms, int start, int length) {`  
`       System.out.println(tabOut()+`  
`               "Call to addSymbols(Alphabet alpha, Symbol[] syms, int start, int length)");`  
`       tab++;`  
`       System.out.println(tabOut()+"alpha: "+alpha.getName());`  
`       System.out.println(tabOut()+"syms.length: "+syms.length);`  
`       System.out.println(tabOut()+"start: "+start);`  
`       System.out.println(tabOut()+"length: "+length);`  
`       `  
`       SimpleSymbolList ssl = new SimpleSymbolList(alpha);`  
`       try{`  
`           for(int i = start; i < length; i++){`  
`               ssl.addSymbol(syms[i]);`  
`           }`  
`       }catch(Exception e){`  
`           e.printStackTrace();`  
`       }`  
`       System.out.println(tabOut()+"Symbol[]: "+ssl.seqString());`  
`       tab--;`  
`   }`

`   public void startSequence() {`  
`       `  
`       System.out.println(tabOut()+"Call to startSequence()");`  
`       tab++;`  
`   }`

`   public void addSequenceProperty(Object key, Object value) {`  
`       System.out.println(tabOut()+"Call to addSequenceProperty(Object key, Object value) ");`  
`       tab++;`  
`       System.out.println(tabOut()+"key: "+key);`  
`       System.out.println(tabOut()+"value: "+value);`  
`       tab--;`  
`   }`

`   public void endFeature() {`  
`       tab--;`  
`       System.out.println(tabOut()+"Call to endFeature()");`  
`   }`

`   public void endSequence() {`  
`       tab--;`  
`       System.out.println(tabOut()+"Call to endSequence()");`  
`   }`

`   public void addFeatureProperty(Object key, Object value) {`  
`       System.out.println(tabOut()+"Call to addFeatureProperty(Object key, Object value)");`  
`       tab++;`  
`       System.out.println(tabOut()+"key: "+key);`  
`       System.out.println(tabOut()+"value: "+value);`  
`       tab--;`  
`   }`  
`   `  
`   `  
`   private String tabOut(){`  
`       StringBuffer sb = new StringBuffer();`  
`       for(int i = 0; i < tab; i++){`  
`           sb.append("\t");`  
`       }`  
`       return sb.toString();`  
`   }`  
`   `  
`   private void dumpAnnotation(Annotation anno){`  
`       System.out.println(tabOut()+"Annotation: "+anno.getClass().getName());`  
`       tab++;`  
`       for(Iterator i = anno.keys().iterator(); i.hasNext();){`  
`           Object key = i.next();`  
`           Object val = anno.getProperty(key);`  
`           System.out.println(tabOut()+"key: "+key+" value: "+val);`  
`       }`  
`       tab--;`  
`   }`  
`   `  
`    /**`  
`     * Run the program. The file name, format class name and alphabet name`  
`     * are all supplied to the command line.`  
`     * @param args arg[0] the file containing the sequences`  
`     * arg[1] the fully qualified name of the format class to be used`  
`     * (eg "org.biojava.bio.seq.io.FastaFormat")`  
`     * arg[2] the case sensitive name of the alphabet (eg "DNA" or "Protein");`  
`     */`  
`   public static void main(String[] args) throws Exception{`  
`       BufferedReader br = new BufferedReader(new FileReader(args[0]));`  
`       `  
`       Class formatClass = Class.forName(args[1]);`  
`       SequenceFormat format = (SequenceFormat)formatClass.newInstance();`  
`       SeqIOListener echo = new SeqIOEcho();`  
`       SymbolTokenization toke = `  
`               AlphabetManager.alphabetForName(args[2]).getTokenization("token");`  
`   `  
`       boolean moreSeq = false;`  
`       do{`  
`           moreSeq = format.readSequence(br, toke, echo);`  
`       }while(moreSeq);`  
`       `  
`   }`

} ```
