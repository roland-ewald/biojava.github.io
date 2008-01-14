---
title: BioJava:CookBook:PDB:header
---

### How can I access the header information of a PDB file?

new: BioJava in CVS now can parse the COMPND and SOURCE header files.
Thanks to Jules Jacobsen (EBI) for providing the patch. The contained
information is availabe via the Compounds class that can be accessed
from structure.getCompounds().

<java> public static void main(String[] args){

`       String code =  "1aoi";`

`       PDBFileReader pdbreader = new PDBFileReader();`  
`       pdbreader.setPath("/Path/To/PDBFiles/");`  
`       pdbreader.setParseSecStruc(true);`  
`       pdbreader.setAlignSeqRes(true);`  
`       pdbreader.setAutoFetch(true);`

`       try{`  
`           Structure struc = pdbreader.getStructureById(code);`  
`           Map`<String,Object>` m = struc.getHeader();`

`           Set`<String>` keys = m.keySet();`  
`           for (String key: keys) {`  
`               System.out.println(key +": " +  m.get(key));`  
`           }`

`           System.out.println("available compounds:");`  
`           List`<Compound>` compounds = struc.getCompounds();`  
`           for (Compound compound:compounds){`  
`               System.out.println(compound);`  
`           }`  
`           `

`       } catch (Exception e) {`  
`           e.printStackTrace();`  
`       }`  
`   }`

</java>

gives the following output:

    title: COMPLEX BETWEEN NUCLEOSOME CORE PARTICLE (H3,H4,H2A,H2B) AND 146 BP LONG DNA FRAGMENT 
    technique: X-RAY DIFFRACTION 
    classification: DNA BINDING PROTEIN/DNA
    depDate: 03-JUL-97
    modDate: 01-APR-03
    idCode: 1AOI
    resolution: 2.8
    available compounds:
    Compound: 1 HISTONE H3 Chains: ChainId: A E Engineered: YES OrganismScientific: XENOPUS LAEVIS OrganismCommon: AFRICAN CLAWED FROG ExpressionSystem: ESCHERICHIA COLI Fragment: HISTONE H3 
    Compound: 2 HISTONE H4 Chains: ChainId: B F Engineered: YES OrganismScientific: XENOPUS LAEVIS OrganismCommon: AFRICAN CLAWED FROG ExpressionSystem: ESCHERICHIA COLI ExpressionSystemOtherDetails: SYNTHETIC GENE, OPTIMIZED CODON USAGE FOR Fragment: HISTONE H4 
    Compound: 3 HISTONE H2A Chains: ChainId: C G Engineered: YES OrganismScientific: XENOPUS LAEVIS OrganismCommon: AFRICAN CLAWED FROG ExpressionSystem: ESCHERICHIA COLI Fragment: HISTONE H2A 
    Compound: 4 HISTONE H2B Chains: ChainId: D H Engineered: YES Mutation: YES OrganismScientific: XENOPUS LAEVIS OrganismCommon: AFRICAN CLAWED FROG ExpressionSystem: ESCHERICHIA COLI Fragment: HISTONE H2B 
    Compound: 5 PALINDROMIC 146 BP DNA REPEAT 8/9 FROM HUMAN X- CHROMOSOME ALPHA SATELLITE DNA Chains: ChainId: I J Engineered: YES Synthetic: YES 

Next: <BioJava:CookBook:PDB:seqres> - How to deal with SEQRES and ATOM
records