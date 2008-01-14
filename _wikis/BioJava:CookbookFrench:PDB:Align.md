---
title: BioJava:CookbookFrench:PDB:Align
---

Comment calculer un alignement de structures?
---------------------------------------------

BioJava vous permet de faire l'alignement de deux Structures grâce à un
algorithme basé sur une variation d'un algorithme en C++ fourni par
Peter Lackner, Univ. de Salzburg (communication personnelle)

Vous pouvez ensuite envoyer cet alignement pour affichage par Jmol grâce
à cette recette.

<java>

` public static void main(String[] args){`

`           PDBFileReader pdbr = new PDBFileReader();          `  
`           pdbr.setPath("/chemin/vers/mes/PDBFiles/");`  
`           `  
`           `  
`           String pdb1 = "1buz";`  
`           String pdb2 = "1ali";            `  
`           String outputfile = "/ailleurs/alig_"+pdb1+"_"+pdb2+".pdb";`  
`         `

`           // AUCUN BESOIN DE MODIFIER QUOIQUE CE SOIT APRES CETTE LIGNE...`  
`           `  
`           StructurePairAligner sc = new StructurePairAligner();            `  
`           `  
`           // etape 1 : lire les fichiers `  
`           `  
`           System.out.println("aligning " + pdb1 + " vs. " + pdb2);`  
`           `  
`           Structure s1 = pdbr.getStructureById(pdb1);`  
`           Structure s2 = pdbr.getStructureById(pdb2);                       `  
`           // vous n'avez pas besoin d'utiliser les structures completes`  
`           // vous pourriez n'utiliser que les atomes de votre choix ;-)`

`           // etape 2 : faire les calculs`  
`           sc.align(s1,s2);`

`           // si vous desirez plus de controle grace aux parametre d'alignement`  
`           // utilisez un objet de la classe StrucAligParameters`  
`           //StrucAligParameters params = new StrucAligParameters();`  
`           //params.setFragmentLength(8);      `  
`           //sc.align(s1,s2,params); `

`           AlternativeAlignment[] aligs = sc.getAlignments();`  
`           `  
`           //rassemble les resultats similaires ensemble `  
`           ClusterAltAligs.cluster(aligs);`  
`           `  
`          // impression des resultats:`  
`          // l'objet AlternativeAlignment vous donne acces aux matrices de rotation `  
`          //et aux vecteurs de deplacement.`  
`          for (int i=0 ; i< aligs.length; i ++){`  
`              AlternativeAlignment aa = aligs[i];`  
`              System.out.println(aa);              `  
`          }`  
`                     `  
`          // convertir l'objet AlternativeAlignment 1 en fichier PDB`  
`          // afin de l'ouvrir avec le logiciel de visualisation de votre choix`  
`          //(e.g. Jmol, Rasmol)`  
`           `  
`          if ( aligs.length > 0) {`  
`              AlternativeAlignment aa1 =aligs[0];`  
`              String pdbstr = aa1.toPDB(s1,s2);`  
`               `  
`              System.out.println("writing alignment to " + outputfile);`  
`              FileOutputStream out= new FileOutputStream(outputfile); `  
`              PrintStream p =  new PrintStream( out );`  
`       `  
`              p.println (pdbstr);`

`              p.close();`  
`              out.close();`  
`          }                       `  
`       } catch (Exception e){`  
`           e.printStackTrace();`  
`       }`

} </java>