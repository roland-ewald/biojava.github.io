---
title: BioJava:CookbookPortuguese:Blast:Parser
permalink: wiki/BioJava%3ACookbookPortuguese%3ABlast%3AParser
---

Como eu verifico um BLAST Result?
---------------------------------

Grande parte do crédito para este exemplo pertence a Keith James.

Uma tarefa freqüente em bioinformatica é a geração de resultados a
partir de pesquisa BLAST. O BioJava possui a habilidade de analisar
gramaticalmente uma saída "Blast-like" como Blast e HMMER utilizando um
truque que faz a saída Blast produzir eventos SAX que podem ser
utilizados por listeners registrados.

O caminho básico é mostrado a seguir:

    Blast_output --> Gera eventos SAX --> Converte eventos SAX --> Cria objetos de resultado --> Armazena-os em uma lista.

    InputStream --> BLASTLikeSAXParser --> SeqSimilartyAdapter --> BlastLikeSearchBuilder --> List.

A API é muito flexível para a maioria dos propósitos e a receita abaixo
o lhe dará uma idéia de como funciona:

```java import java.io.\*; import java.util.\*;

import org.biojava.bio.program.sax.\*; import
org.biojava.bio.program.ssbind.\*; import org.biojava.bio.search.\*;
import org.biojava.bio.seq.db.\*; import org.xml.sax.\*; import
org.biojava.bio.\*;

public class BlastParser {

` /**`  
`  * args[0] nome do arquivo de saída Blast`  
`  */`  
` public static void main(String[] args) {`  
`   try {`  
`     //obtém o arquivo Blast como Stream`  
`     InputStream is = new FileInputStream(args[0]);`

`     //cria um BlastLikeSAXParser`  
`     BlastLikeSAXParser parser = new BlastLikeSAXParser();`

`     //cria o evento SAX adapter que irá passar eventos para um Handler.`  
`     SeqSimilarityAdapter adapter = new SeqSimilarityAdapter();`

`     //atribui o evento de parser SAX`  
`     parser.setContentHandler(adapter);`

`     //A lista que armazenará o SeqSimilaritySearchResults`  
`     List results = new ArrayList();`

`     //cria o SearchContentHandler que irá gerar SeqSimilaritySearchResults`  
`     //na List resultante`  
`     SearchContentHandler builder = new BlastLikeSearchBuilder(results,`  
`         new DummySequenceDB("queries"), new DummySequenceDBInstallation());`

`     //registra o builder com adapter`  
`     adapter.setSearchContentHandler(builder);`

`     //Verifica o arquivo, após isto a Lista de resultado será populada com      `  
`     //SeqSimilaritySearchResults`  
`     parser.parse(new InputSource(is));`

`     //exibe alguns detalhes blast `  
`     for (Iterator i = results.iterator(); i.hasNext(); ) {`  
`       SeqSimilaritySearchResult result =`  
`           (SeqSimilaritySearchResult)i.next();`

`       Annotation anno = result.getAnnotation();`

`       for (Iterator j = anno.keys().iterator(); j.hasNext(); ) {`  
`         Object key = j.next();`  
`         Object property = anno.getProperty(key);`  
`         System.out.println(key+" : "+property);`  
`       }`  
`       System.out.println("Hits: ");`

`       //lista os acertos`  
`       for (Iterator k = result.getHits().iterator(); k.hasNext(); ) {`  
`         SeqSimilaritySearchHit hit =`  
`             (SeqSimilaritySearchHit)k.next();`  
`         System.out.print("\tmatch: "+hit.getSubjectID());`  
`         System.out.println("\te score: "+hit.getEValue());`  
`       }`

`       System.out.println("\n");`  
`     }`

`   }`  
`   catch (SAXException ex) {`  
`     //erro de XML`  
`     ex.printStackTrace();`  
`   }catch (IOException ex) {`  
`     //erro de IO, provavelmente arquivo não encontrado`  
`     ex.printStackTrace();`  
`   }`  
` }`

} ```
