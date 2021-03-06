---
title: BioJava:CookBook:Interfaces:Features
permalink: wiki/BioJava%3ACookBook%3AInterfaces%3AFeatures
---

How do I display Features?
--------------------------

Features are displayed by implementations of the FeatureRenderer
interface. FeatureRenderers work in much the same way as
SequenceRenderers and handle the drawing of the Features from a Sequence
that is held in a SequenceRenderContext.

A SequenceRenderContext has no way of interacting directly with a
FeatureRenderer so a FeatureBlockSequenceRenderer is used to wrap up the
FeatureRenderer and act as a proxy.

The use of a FeatureBlockSequenceRenderer and a FeatureRenderer is
demonstrated in the program below. A screen shot follows the program.

[frame|center|Features in a GUI](image:Featview.jpg "wikilink")

```java import java.awt.\*; import java.awt.event.\*;

import javax.swing.\*;

import org.biojava.bio.\*; import org.biojava.bio.gui.sequence.\*;
import org.biojava.bio.seq.\*; import org.biojava.bio.symbol.\*;

public class FeatureView extends JFrame {

` private Sequence seq;`  
` private JPanel jPanel1 = new JPanel();`

` private MultiLineRenderer mlr = new MultiLineRenderer();`  
` private FeatureRenderer featr = new BasicFeatureRenderer();`  
` private SequenceRenderer seqR = new SymbolSequenceRenderer();`  
` private SequencePanel seqPanel = new SequencePanel();`  
` //the proxy between featr and seqPanel`  
` private FeatureBlockSequenceRenderer fbr = new FeatureBlockSequenceRenderer();`

` public FeatureView() {`  
`   try {`  
`     seq = DNATools.createDNASequence(`  
`         "atcgcgcatgcgcgcgcgcgcgcgctttatagcgatagagatata",`  
`         "dna 1");`

`     //create feature from 10 to 25`  
`     StrandedFeature.Template temp = new StrandedFeature.Template();`  
`     temp.annotation = Annotation.EMPTY_ANNOTATION;`  
`     temp.location = new RangeLocation(10,25);`  
`     temp.source = "";`  
`     temp.strand = StrandedFeature.POSITIVE;`  
`     temp.type = "";`

`     //create another from 30 to 35`  
`     Feature f = seq.createFeature(temp);`  
`     temp = (StrandedFeature.Template)f.makeTemplate();`  
`     temp.location = new RangeLocation(30,35);`  
`     temp.strand = StrandedFeature.NEGATIVE;`  
`     seq.createFeature(temp);`

`     //setup GUI`  
`     init();`  
`   }`  
`   catch(Exception e) {`  
`     e.printStackTrace();`  
`   }`  
` }`

` public static void main(String[] args) {`  
`   FeatureView featureView = new FeatureView();`  
`   featureView.pack();`  
`   featureView.show();`  
` }`

` /**`  
`  * initialize GUI components`  
`  */`  
` private void init() throws Exception {`  
`   this.setTitle("FeatureView");`  
`   this.getContentPane().add(jPanel1, BorderLayout.CENTER);`  
`   jPanel1.add(seqPanel, null);`

`   //Register the FeatureRenderer with the FeatureBlockSequenceRenderer`  
`   fbr.setFeatureRenderer(featr);`

`   //add Renderers to the MultiLineRenderer`  
`   mlr.addRenderer(fbr);`  
`   mlr.addRenderer(seqR);`

`   //set the MultiLineRenderer as the SequencePanels renderer`  
`   seqPanel.setRenderer(mlr);`

`   //set the Sequence to Render`  
`   seqPanel.setSequence(seq);`

`   //display the whole Sequence`  
`   seqPanel.setRange(new RangeLocation(1,seq.length()));`  
` }`

` /**`  
`  * Overridden so program terminates when window closes`  
`  */`  
` protected void processWindowEvent(WindowEvent we){`  
`   if (we.getID() == WindowEvent.WINDOW_CLOSING) {`  
`     System.exit(0);`  
`   }`  
`   else {`  
`     super.processWindowEvent(we);`  
`   }`  
` }`

} ```
