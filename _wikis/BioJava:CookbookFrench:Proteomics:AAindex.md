---
title: BioJava:CookbookFrench:Proteomics:AAindex
---

Note: The classes mentioned in this article are not integrated in the
BioJava 1.4 library, yet. However, they are avaiable via
[CVS](http://cvs.biojava.org).

Comment analyser les propriétés d'une séquence protéique en utilisant la base de données *Amino Acid Index*?
------------------------------------------------------------------------------------------------------------

Afin d'analyser les propriétés des symboles contenus dans une séquence
d'acides aminés (par exemple, l'hydrophobicité moyenne d'une protéine) ,
vous n'avez qu'à utiliser l'interface *SymbolPropertyTable*. La méthode
**getDoubleValue()** qui s'y trouve retourne la valeur numérique pour un
acide aminé donn&eacute tel qu'une valeur négative ou positive indiquant
l'hydrophobicité d'un acide aminé par rapport aux autres acides aminés.
La base de données [*Amino Acid
Index*](http://www.genome.ad.jp/dbget/aaindex.html) contient plus de 500
différentes tables de propriété d'acide aminé toutes contenue dans une
simple fichier en format texte appellé
[*aaindex1*](ftp://ftp.genome.ad.jp/pub/db/genomenet/aaindex/aaindex1).
[AAindex1](http://www.genome.jp/dbget-bin/show_man?aaindex) désigne
également le nom du format de ce fichier.

Le fichier *aaindex1* se charge en mémoire via l'utilisation de la
classe *AAindexStreamReader*. Ensuite, il est possible de parcourir les
différentes tables de propriété grâce à la méthode **nextTable()**, qui
retourne chaque table comme un objet de type *AAindex* implémentant
l'interface *SymbolPropertyTable*. Si le fichier doit être maintenu en
mémoire pour accès aléatoire (en utilisant le nom de la table comme
clé), il est possible de le faire via un objet de type
*SimpleSymbolPropertyTableDB* dont le contenu est initialisé avec un
objet de type *AAindexStreamReader*.

L'exemple suivant montre comment calculer l'hydrophobicité moyeenne pour
une séquence de prot&eacute'ine (l'exemple ici contient les 20 acides
aminés) en utilisant l'information contenue dans la table CIDH920105 du
fichier *aaindex1*:

    SimpleSymbolPropertyTableDB db = new SimpleSymbolPropertyTableDB(new AAindexStreamReader(new FileReader("aaindex1")));
    AAindex hydrophobicity = (AAindex) db.table("CIDH920105");
    SymbolList symbols = ProteinTools.createProtein("ARNDCEQGHILKMFPSTWYV");
    double hp = 0.0;
    for (int i = 1; i <= symbols.length(); i++) {
        hp += hydrophobicity.getDoubleValue(symbols.symbolAt(i));
    }
    System.out.println("Average hydrophobicity: " + Double.toString(hp / symbols.length()));