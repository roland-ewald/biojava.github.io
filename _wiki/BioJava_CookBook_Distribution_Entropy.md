---
title: BioJava:CookBook:Distribution:Entropy
permalink: wiki/BioJava%3ACookBook%3ADistribution%3AEntropy
---

How can I find the amount of information or entropy in a Distribution?
----------------------------------------------------------------------

The amount of information or entropy in a Distribution is a reflection
of the redundancy of the Distribution. Shannon information and Entropy
can be calculated using static methods from the DistributionTools class.

Shannon information is returned as a double and reflects the total
information content. The entropy is returned as a HashMap between each
Symbol and its corresponding entropy. The following program calculates
both for a very biased Distribution.

```java import java.util.\*;

import org.biojava.bio.dist.\*; import org.biojava.bio.seq.\*; import
org.biojava.bio.symbol.\*;

public class Entropy {

` public static void main(String[] args) {`

`   Distribution dist = null;`  
`   try {`  
`     //create a biased distribution`  
`     dist =`  
`         DistributionFactory.DEFAULT.createDistribution(DNATools.getDNA());`

`     //set the weight of a to 0.97`  
`     dist.setWeight(DNATools.a(), 0.97);`

`     //set the others to 0.01`  
`     dist.setWeight(DNATools.c(), 0.01);`  
`     dist.setWeight(DNATools.g(), 0.01);`  
`     dist.setWeight(DNATools.t(), 0.01);`  
`   }`  
`   catch (Exception ex) {`  
`     ex.printStackTrace();`  
`     System.exit(-1);`  
`   }`

`   //calculate the information content`  
`   double info = DistributionTools.bitsOfInformation(dist);`  
`   System.out.println("information = "+info+" bits");`  
`   System.out.print("\n");`

`   //calculate the Entropy (using the conventional log base of 2)`  
`   HashMap entropy = DistributionTools.shannonEntropy(dist, 2.0);`

`   //print the Entropy of each residue`  
`   System.out.println("Symbol\tEntropy");`  
`   for (Iterator i = entropy.entrySet().iterator(); i.hasNext(); ) {`  
`     Map.Entry entry = (Map.Entry)i.next();`  
`     Symbol sym = (Symbol)entry.getKey();`  
`     Double val = (Double)entry.getValue();`  
`     System.out.println(sym.getName()+ "\t" +val);`  
`   }`  
` }`

} ```
