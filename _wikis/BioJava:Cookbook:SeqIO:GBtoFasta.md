---
title: BioJava:Cookbook:SeqIO:GBtoFasta
---

How do I extract Sequences from GenBank/ EMBL/ SwissProt etc and write them as Fasta?
-------------------------------------------------------------------------------------

To perform this task we are going to extend the general reader from the
previous demo and include in it the ability to write sequence data in
fasta format. Two examples are provided, one uses the slightly earlier
biojava1.3 pre1 and the second uses the more up to date biojava 1.3

(Using Biojava 1.3 pre 1)

    import org.biojava.bio.seq.io.*;
    import org.biojava.bio.seq.*;
    import java.io.*;

    public class WriteToFasta {

      /**
       * This program will read any file supported by SeqIOTools it takes two
       * arguments, the first is the file name the second is the int constant
       * for the file type in SeqIOTools. See SeqIOTools for possible file types.
       * The constants used are:
       * UNKNOWN = 0;
       * FASTADNA = 1;
       * FASTAPROTEIN = 2;
       * EMBL = 3;
       * GENBANK = 4;
       * SWISSPROT = 5;
       * GENPEPT = 6;
       * MSFDNA = 7;
       * FASTAALIGNDNA = 9;
       * MSFPROTEIN = 10;
       * FASTAALIGNPROTEIN = 11;
       * MSF = 12;
       *
       */
      public static void main(String[] args) {
        try {
          //prepare a BufferedReader for file io
          BufferedReader br = new BufferedReader(new FileReader(args[0]));

          //get the int constant for the file type
          int fileType = Integer.parseInt(args[1]);

          /*
           * get a Sequence Iterator over all the sequences in the file.
           * SeqIOTools.fileToBiojava() returns an Object. If the file read
           * is an alignment format like MSF and Alignment object is returned
           * otherwise a SequenceIterator is returned.
           */
          SequenceIterator iter =
              (SequenceIterator)SeqIOTools.fileToBiojava(fileType, br);

          //and now write it all to FASTA, (you can write to any OutputStream, not just System.out)
          SeqIOTools.writeFasta(System.out, iter);
        }
        catch (Exception ex) {
          ex.printStackTrace();
        }
      }
    }

(Using Biojava 1.3)

    import java.io.*;


    import org.biojava.bio.*;
    import org.biojava.bio.seq.*;
    import org.biojava.bio.seq.io.*;

    public class GeneralReader {

       /**
       * This program will read any file supported by SeqIOTools it takes three
       * arguments, the first is the file name the second is the format type the
       * third is the type of residue being read. Illegal combinations such as
       * SwissProt and DNA will cause an exception.
         *
       * Allowed formats are: (case insensitive).
         *
       * FASTA
       * EMBL
       * GENBANK
       * SWISSPROT (or swiss)
       * GENPEPT
         *
       * Allowed sequence types are: (case insensititve).
         *
       * DNA
       * AA (or Protein)
       * RNA
         *
         */
       public static void main(String[] args) {
           try {
               //prepare a BufferedReader for file io
          BufferedReader br = new BufferedReader(new FileReader(args[0]));

               //the flat file format
          String format = args[1];

               //the Alphabet
          String alpha = args[2];

               //get the int value for the format and alphabet


               /*
           * get a Sequence Iterator over all the sequences in the file.
           * SeqIOTools.fileToBiojava() returns an Object. If the file read
           * is an alignment format like MSF and Alignment object is returned
           * otherwise a SequenceIterator is returned.
                 */
          SequenceIterator iter =
              (SequenceIterator)SeqIOTools.fileToBiojava(format, alpha, br);

               // do something with the sequences
          SeqIOTools.writeFasta(System.out, iter);
           }
           catch (FileNotFoundException ex) {
               //can't find file specified by args[0]
               ex.printStackTrace();
           }catch (BioException ex) {
               //invalid file format name
               ex.printStackTrace();
           }catch (IOException ex){
               //error writing to fasta
               ex.printStackTrace();
           }
       }
    }