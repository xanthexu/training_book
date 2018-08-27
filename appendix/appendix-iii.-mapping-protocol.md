# Appendix III. Mapping Protocoldsaf

## Directional RNA-seq data - which parameters to choose?

\(adapted from: [https://chipster.csc.ﬁ/manual/library-type-summary.html](https://chipster.csc.ﬁ/manual/library-type-summary.html)\)

Directional RNA-seq methods are gaining popularity. Several protocols and products are available for the library preparation step, and different tools and softwares have different options to take these into account. Since this has caused a lot of confusion due to incoherent parameter naming, we try to clarify this issue a bit here.

To be able to select the right parameters for your data, ﬁrst you need to know which library prep method was used when generating your data. In general, there are three types of library preps:

* un-stranded 
* "second-strand" = directional, where the ﬁrst read of the read pair \(or in case of single end reads, the only read\) is from the transcript strand 
* "ﬁrst-strand" = directional, where the ﬁrst read \(or the only read in case of SE\) is from the opposite strand.

![Summary of library type protocols](../.gitbook/assets/summary_of_library_type_protocols.png)

\(borrowed from: [http://onetipperday.sterding.com/2012/07/how-to-tell-which-library-type-to-use.html](http://onetipperday.sterding.com/2012/07/how-to-tell-which-library-type-to-use.html)\)

The reads on the left are from the same strand as the transcript, and their pairs on the right are from the opposing strand. The number above the read states which read it is, the ﬁrst \(/1\) or the second \(/2\). Thus, perhaps a bit unintuitively, the ﬁrst case, "fr-ﬁrststrand" is such that the ﬁrst read \(/1\) is actually from the opposing strand as the transcript, and second read \(/2\) is from the transcript strand.

**Why is this so important?** If you use wrong directionality parameter, in the read counting step the reads are considered to be from the wrong strand. This means that in the cases where there are no genes on that other strand, you won't get any hits, and if there are genes in the same location on the other strand, your reads are counted for that wrong gene.

**How can I check I chose correctly?** It's a good idea to check that!

**Using the tool** [RNA-seq strandedness inference and inner distance estimation using RseQC](https://chipster.csc.fi/manual/rseqc_infer_rnaseq_experiment.html): We added this tool under the **Quality control** category to help you. The tool aligns subsets of the input FASTQ ﬁles against the reference genome, and this alignment is then compared to the reference annotation to deduce the strandedness. Make sure you select the correct reference when running the tool. Check out the help page of this tool for more information!

**In aligners like HISAT2 and Tophat** you can also do a comparison and check the mapping rate. Take a small subset of your reads and run HISAT2/TopHat with the different parameters and compare the results, and check the log ﬁle.

**In HTSeq** you can also run the tool with different options and check the number of reads that are not counted for any gene \(=the "no-feature reads"\). \(In Chipster, open ﬁle htseq-count-info.txt\).

**Be extra careful to assign the paired ﬁles correctly!** Using these parameters assumes you are giving the ﬁles in speciﬁc order: read1, read2. In Chipster always check from the parameters window that your ﬁles are assigned correctly.

Below we list some common library preparation kits and their corresponding parameters in different tools. **Is your kit missing from the list?** If you have the data generated with that kit and ﬁgure out the library type, please let us know too, so we can add that kit to the list below.

_Unstranded:_

Information regarding the strand is not conserved \(it is lost during the ampliﬁcation of the mRNA fragments\).

**Kits:**

* TruSeq RNA Sample Prep kit

**Parameters:**

* HISAT2 / TopHat / Cufﬂinks / Cuffdiff: library-type fr-unstranded 
* HTSeq: stranded -- no

_Directional, ﬁrst strand:_

The second read \(read 2\) is from the original RNA strand/template, ﬁrst read \(read 1\) is from the opposite strand. The information of the strand is preserved as the original RNA strand is degradated due to the dUTPs incorporated in the second synthesis step.

**Kits:**

* All dUTP methods, NSR, NNSR 
* TruSeq Stranded Total RNA Sample Prep Kit 
* TruSeq Stranded mRNA Sample Prep Kit 
* NEB Ultra Directional RNA Library Prep Kit 
* Agilent SureSelect Strand-Speciﬁc

**Parameters:**

* HISAT2 / TopHat / Cufﬂinks / Cuffdiff: library-type fr-ﬁrst \(`bowtie -fr`\)
* strand HTSeq: stranded -- reverse \(`featurecount -s 2`\)

_Directional, second strand:_

The ﬁrst read \(read 1\) is from the original RNA strand/template, second read \(read 2\) is from the opposite strand. The directionality is preserved, as different adapters are ligated to different ends of the fragment.

**Kits:**

* Directional Illumina \(Ligation\), Standard SOLiD 
* ScriptSeq v2 RNA-Seq Library Preparation Kit \(ribo-seq\)
* SMARTer Stranded Total RNA 
* Encore Complete RNA-Seq Library Systems 
* NuGEN SoLo

**Parameters:**

* HISAT2 / TopHat / Cufﬂinks / Cuffdiff: library-type fr-secondstrand \(`bowtie -rf`\)
* HTSeq: stranded -- yes \(`featurecount -s 1`\)

Note also that the --fr/--rf/--ff or "Order of mates to align" parameter in Bowtie has similar sounding parameter options: \[--fr: "Forward/reverse", --rf: "Reverse/Forward", --ff: "Forward/forward"\]. However, these parameters are a bit different story, as they explain how the paired end reads are oriented towards each other \(-&gt; &lt;-, -&gt; -&gt; or &lt;- -&gt;\). The default \(--fr, -&gt; &lt;-\) is appropriate for Illumina's paired-end reads: it means that read 1 appears upstream of the reverse complement of read 2, or vice versa. When running TopHat, the library-type parameter is delivered to Bowtie, so the user doesn't have to worry about that too much.

