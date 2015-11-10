<!-- it's odd that in the table of contents that some words need to be
connected with dash while others with underscore in order to make linking work
when rendered on github page -->

1. [NAME](#name)
2. [DESCRIPTION](#description)
3. [CITATION](#citation)
4. [VERSION](#version)
5. [AUTHOR](#author)
6. [INSTALLATION](#installation)
	1. [Dependencies](#dependencies)
	2. [Procedure](#procedure)
	3. [No administrator privileges?](#no-administrator-privileges)
7. [RUNNING GRINDER](#running-grinder)
8. [REFERENCE SEQUENCE DATABASE](#reference-sequence-database)
9. [CLI EXAMPLES](#cli-examples)
10. [CLI REQUIRED ARGUMENTS](#cli-required-arguments)
11. [CLI OPTIONAL ARGUMENTS](#cli-optional-arguments)
12. [CLI OUTPUT](#cli-output)
13. [API EXAMPLES](#api-examples)
14. [API METHODS](#api-methods)
	1. [new](#new)
	2. [next\_lib](#next_lib)
	3. [next\_read](#next_read)
	4. [get\_random\_seed](#get_random_seed)
15. [COPYRIGHT](#copyright)
16. [BUGS](#bugs)

------------------------------------------------------------------------
NAME
====

grinder - A versatile omics shotgun and amplicon sequencing read
simulator

------------------------------------------------------------------------

DESCRIPTION
===========

Grinder is a versatile program to create random shotgun and amplicon
sequence libraries based on DNA, RNA or proteic reference sequences
provided in a FASTA file.

Grinder can produce genomic, metagenomic, transcriptomic,
metatranscriptomic, proteomic, metaproteomic shotgun and amplicon
datasets from current sequencing technologies such as Sanger, 454,
Illumina. These simulated datasets can be used to test the accuracy of
bioinformatic tools under specific hypothesis, e.g. with or without
sequencing errors, or with low or high community diversity. Grinder may
also be used to help decide between alternative sequencing methods for a
sequence-based project, e.g. should the library be paired-end or not,
how many reads should be sequenced.

Grinder features include:

-   shotgun or amplicon read libraries

-   omics support to generate genomic, transcriptomic, proteomic,
    metagenomic, metatranscriptomic or metaproteomic datasets

-   arbitrary read length distribution and number of reads

-   simulation of PCR and sequencing errors (chimeras, point mutations,
    homopolymers)

-   support for paired-end (mate pair) datasets

-   specific rank-abundance settings or manually given abundance for
    each genome, gene or protein

-   creation of datasets with a given richness (alpha diversity)

-   independent datasets can share a variable number of genomes (beta
    diversity)

-   modeling of the bias created by varying genome lengths or gene copy
    number

-   profile mechanism to store preferred options

-   available to biologists or power users through multiple interfaces:
    GUI, CLI and API

Briefly, given a FASTA file containing reference sequence (genomes,
genes, transcripts or proteins), Grinder performs the following steps:

1.  Read the reference sequences, and for amplicon datasets, extracts
    full-length reference PCR amplicons using the provided degenerate
    PCR primers.

2.  Determine the community structure based on the provided alpha
    diversity (number of reference sequences in the library), beta
    diversity (number of reference sequences in common between several
    independent libraries) and specified rank- abundance model.

3.  Take shotgun reads from the reference sequences or amplicon reads
    from the full- length reference PCR amplicons. The reads may be
    paired-end reads when an insert size distribution is specified. The
    length of the reads depends on the provided read length distribution
    and their abundance depends on the relative abundance in the
    community structure. Genome length may also biases the number of
    reads to take for shotgun datasets at this step. Similarly, for
    amplicon datasets, the number of copies of the target gene in the
    reference genomes may bias the number of reads to take.

4.  Alter reads by inserting sequencing errors (indels, substitutions
    and homopolymer errors) following a position-specific model to
    simulate reads created by current sequencing technologies (Sanger,
    454, Illumina). Write the reads and their quality scores in FASTA,
    QUAL and FASTQ files.

------------------------------------------------------------------------

CITATION
========

If you use Grinder in your research, please cite:

       Angly FE, Willner D, Rohwer F, Hugenholtz P, Tyson GW (2012), Grinder: a
       versatile amplicon and shotgun sequence simulator, Nucleic Acids Reseach

Available from <http://dx.doi.org/10.1093/nar/gks251>.

------------------------------------------------------------------------

VERSION
=======

This document refers to grinder version 0.5.2

------------------------------------------------------------------------

AUTHOR
======

Florent Angly \<<florent.angly@gmail.com>\>

------------------------------------------------------------------------

INSTALLATION
============

Dependencies
------------

You need to install these dependencies first:

-   Perl (\>= 5.6)

    <http://www.perl.com/download.csp>

-   make

    Many systems have make installed by default. If your system does
    not, you should install the implementation of make of your choice,
    e.g. GNU make: <http://www.gnu.org/s/make/>

The following CPAN Perl modules are dependencies that will be installed
automatically for you:

-   Bioperl modules (\>=1.6.901).

    Note that some unreleased Bioperl modules have been included in
    Grinder.

-   Getopt::Euclid (\>= 0.3.4)

-   List::Util

    First released with Perl v5.7.3

-   Math::Random::MT (\>= 1.13)

-   version (\>= 0.77)

    First released with Perl v5.9.0

Procedure
---------

To install Grinder globally on your system, run the following commands
in a terminal or command prompt:

On Linux, Unix, MacOS:

       perl Makefile.PL
       make

And finally, with administrator privileges:

       make install

On Windows, run the same commands but with nmake instead of make.

No administrator privileges?
----------------------------

If you do not have administrator privileges, Grinder needs to be
installed in your home directory.

First, follow the instructions to install local::lib at
<http://search.cpan.org/~apeiron/local-lib-1.008004/lib/local/lib.pm#The_bootstrapping_technique>.
After local::lib is installed, every Perl module that you install
manually or through the CPAN command-line application will be installed
in your home directory.

Then, install Grinder by following the instructions detailed in the
"Procedure" section.

------------------------------------------------------------------------

RUNNING GRINDER
===============

After installation, you can run Grinder using a command-line interface
(CLI), an application programming interface (API) or a graphical user
interface (GUI) in Galaxy.

To get the usage of the CLI, type:

      grinder --help

More information, including the documentation of the Grinder API, which
allows you to run Grinder from within other Perl programs, is available
by typing:

      perldoc Grinder

To run the GUI, refer to the Galaxy documentation at
<http://wiki.g2.bx.psu.edu/FrontPage>.

The 'utils' folder included in the Grinder package contains some
utilities:

**average genome size:**

:   This calculates the average genome size (in bp) of a simulated
    random library produced by Grinder.

**change\_paired\_read\_orientation:**

:   This reverses the orientation of each second mate-pair read (ID
    ending in /2) in a FASTA file.

------------------------------------------------------------------------

REFERENCE SEQUENCE DATABASE
===========================

A variety of FASTA databases can be used as input for Grinder. For
example, the GreenGenes database
(<http://greengenes.lbl.gov/Download/Sequence_Data/Fasta_data_files/Isolated_named_strains_16S_aligned.fasta>)
contains over 180,000 16S rRNA clone sequences from various species
which would be appropriate to produce a 16S rRNA amplicon dataset. A set
of over 41,000 OTU representative sequences and their affiliation in
seven different taxonomic sytems can also be used for the same purpose
(<http://greengenes.lbl.gov/Download/OTUs/gg_otus_6oct2010/rep_set/gg_97_otus_6oct2010.fasta>
and
<http://greengenes.lbl.gov/Download/OTUs/gg_otus_6oct2010/taxonomies/>).
The RDP (<http://rdp.cme.msu.edu/download/release10_27_unaligned.fa.gz>)
and Silva
(<http://www.arb-silva.de/no_cache/download/archive/release_108/Exports/>)
databases also provide many 16S rRNA sequences and Silva includes
eukaryotic sequences. While 16S rRNA is a popular gene, datasets
containing any type of gene could be used in the same fashion to
generate simulated amplicon datasets, provided appropriate primers are
used.

The \>2,400 curated microbial genome sequences in the NCBI RefSeq
collection (<ftp://ftp.ncbi.nih.gov/refseq/release/microbial/>) would
also be suitable for producing 16S rRNA simulated datasets (using the
adequate primers). However, the lower diversity of this database
compared to the previous two makes it more appropriate for producing
artificial microbial metagenomes. Individual genomes from this database
are also very suitable for the simulation of single or double-barreled
shotgun libraries. Similarly, the RefSeq database contains over 3,100
curated viral sequences (<ftp://ftp.ncbi.nih.gov/refseq/release/viral/>)
which can be used to produce artificial viral metagenomes.

Quite a few eukaryotic organisms have been sequenced and their genome or
genes can be the basis for simulating genomic, transcriptomic (RNA-seq)
or proteomic datasets. For example, you can use the human genome
available at <ftp://ftp.ncbi.nih.gov/refseq/H_sapiens/RefSeqGene/>, the
human transcripts downloadable from
<ftp://ftp.ncbi.nih.gov/refseq/H_sapiens/mRNA_Prot/human.rna.fna.gz> or
the human proteome at
<ftp://ftp.ncbi.nih.gov/refseq/H_sapiens/mRNA_Prot/human.protein.faa.gz>.

------------------------------------------------------------------------

CLI EXAMPLES
============

Here are a few examples that illustrate the use of Grinder in a
terminal:

1.  A shotgun DNA library with a coverage of 0.1X

           grinder -reference_file genomes.fna -coverage_fold 0.1

2.  Same thing but save the result files in a specific folder and with a
    specific name

           grinder -reference_file genomes.fna -coverage_fold 0.1 -base_name my_name -output_dir my_dir

3.  A DNA shotgun library with 1000 reads

           grinder -reference_file genomes.fna -total_reads 1000

4.  A DNA shotgun library where species are distributed according to a
    power law

           grinder -reference_file genomes.fna -abundance_model powerlaw 0.1

5.  A DNA shotgun library with 123 genomes taken random from the given
    genomes

           grinder -reference_file genomes.fna -diversity 123

6.  Two DNA shotgun libraries that have 50% of the species in common

           grinder -reference_file genomes.fna -num_libraries 2 -shared_perc 50

7.  Two DNA shotgun library with no species in common and distributed
    according to a exponential rank-abundance model. Note that because
    the parameter value for the exponential model is omitted, each
    library uses a different randomly chosen value:

           grinder -reference_file genomes.fna -num_libraries 2 -abundance_model exponential

8.  A DNA shotgun library where species relative abundances are manually
    specified

           grinder -reference_file genomes.fna -abundance_file my_abundances.txt

9.  A DNA shotgun library with Sanger reads

           grinder -reference_file genomes.fna -read_dist 800 -mutation_dist linear 1 2 -mutation_ratio 80 20

10. A DNA shotgun library with first-generation 454 reads

           grinder -reference_file genomes.fna -read_dist 100 normal 10 -homopolymer_dist balzer

11. A paired-end DNA shotgun library, where the insert size is normally
    distributed around 2.5 kbp and has 0.2 kbp standard deviation

           grinder -reference_file genomes.fna -insert_dist 2500 normal 200

12. A transcriptomic dataset

           grinder -reference_file transcripts.fna

13. A unidirectional transcriptomic dataset

           grinder -reference_file transcripts.fna -unidirectional 1

    Note the use of -unidirectional 1 to prevent reads to be taken from
    the reverse- complement of the reference sequences.

14. A proteomic dataset

           grinder -reference_file proteins.faa -unidirectional 1

15. A 16S rRNA amplicon library

           grinder -reference_file 16Sgenes.fna -forward_reverse 16Sprimers.fna -length_bias 0 -unidirectional 1

    Note the use of -length\_bias 0 because reference sequence length
    should not affect the relative abundance of amplicons.

16. The same amplicon library with 20% of chimeric reads (90% bimera,
    10% trimera)

           grinder -reference_file 16Sgenes.fna -forward_reverse 16Sprimers.fna -length_bias 0 -unidirectional 1 -chimera_perc 20 -chimera_dist 90 10

17. Three 16S rRNA amplicon libraries with specified MIDs and no
    reference sequences in common

           grinder -reference_file 16Sgenes.fna -forward_reverse 16Sprimers.fna -length_bias 0 -unidirectional 1 -num_libraries 3 -multiplex_ids MIDs.fna

18. Reading reference sequences from the standard input, which allows
    you to decompress FASTA files on the fly:

           zcat microbial_db.fna.gz | grinder -reference_file - -total_reads 100

------------------------------------------------------------------------

CLI REQUIRED ARGUMENTS
======================

**-rf \<reference\_file\> | -reference\_file \<reference\_file\> | -gf \<reference\_file\> | -genome\_file \<reference\_file\>**

:   FASTA file that contains the input reference sequences (full
    genomes, 16S rRNA genes, transcripts, proteins...) or '-' to read
    them from the standard input. See the README file for examples of
    databases you can use and where to get them from. Default: -

------------------------------------------------------------------------

CLI OPTIONAL ARGUMENTS
======================

**-tr \<total\_reads\> | -total\_reads \<total\_reads\>**

:   Number of shotgun or amplicon reads to generate for each library. Do
    not specify this if you specify the fold coverage. Default: 100

**-cf \<coverage\_fold\> | -coverage\_fold \<coverage\_fold\>**

:   Desired fold coverage of the input reference sequences (the output
    FASTA length divided by the input FASTA length). Do not specify this
    if you specify the number of reads directly.

**-rd \<read\_dist\>... | -read\_dist \<read\_dist\>...**

:   Desired shotgun or amplicon read length distribution specified as:
    average length, distribution ('uniform' or 'normal') and standard
    deviation.

    Only the first element is required. Examples:

          All reads exactly 101 bp long (Illumina GA 2x): 101
          Uniform read distribution around 100+-10 bp: 100 uniform 10
          Reads normally distributed with an average of 800 and a standard deviation of 100
            bp (Sanger reads): 800 normal 100
          Reads normally distributed with an average of 450 and a standard deviation of 50
            bp (454 GS-FLX Ti): 450 normal 50

    Reference sequences smaller than the specified read length are not
    used. Default: 100

**-id \<insert\_dist\>... | -insert\_dist \<insert\_dist\>...**

:   Create paired-end or mate-pair reads spanning the given insert
    length. Important: the insert is defined in the biological sense,
    i.e. its length includes the length of both reads and of the stretch
    of DNA between them: 0 : off, or: insert size distribution in bp, in
    the same format as the read length distribution (a typical value is
    2,500 bp for mate pairs) Two distinct reads are generated whether or
    not the mate pair overlaps. Default: 0

**-mo \<mate\_orientation\> | -mate\_orientation \<mate\_orientation\>**

:   When generating paired-end or mate-pair reads (see
    \<insert\_dist\>), specify the orientation of the reads (F: forward,
    R: reverse):

           FR:  ---> <---  e.g. Sanger, Illumina paired-end, IonTorrent mate-pair
           FF:  ---> --->  e.g. 454
           RF:  <--- --->  e.g. Illumina mate-pair
           RR:  <--- <---

    Default: FR

**-ec \<exclude\_chars\> | -exclude\_chars \<exclude\_chars\>**

:   Do not create reads containing any of the specified characters (case
    insensitive). For example, use 'NX' to prevent reads with
    ambiguities (N or X). Grinder will error if it fails to find a
    suitable read (or pair of reads) after 10 attempts. Consider using
    \<delete\_chars\>, which may be more appropriate for your case.
    Default: ''

**-dc \<delete\_chars\> | -delete\_chars \<delete\_chars\>**

:   Remove the specified characters from the reference sequences
    (case-insensitive), e.g. '-\~\*' to remove gaps (- or \~) or
    terminator (\*). Removing these characters is done once, when
    reading the reference sequences, prior to taking reads. Hence it is
    more efficient than \<exclude\_chars\>. Default:

**-fr \<forward\_reverse\> | -forward\_reverse \<forward\_reverse\>**

:   Use DNA amplicon sequencing using a forward and reverse PCR primer
    sequence provided in a FASTA file. The reference sequences and their
    reverse complement will be searched for PCR primer matches. The
    primer sequences should use the IUPAC convention for degenerate
    residues and the reference sequences that that do not match the
    specified primers are excluded. If your reference sequences are full
    genomes, it is recommended to use \<copy\_bias\> = 1 and
    \<length\_bias\> = 0 to generate amplicon reads. To sequence from
    the forward strand, set \<unidirectional\> to 1 and put the forward
    primer first and reverse primer second in the FASTA file. To
    sequence from the reverse strand, invert the primers in the FASTA
    file and use \<unidirectional\> = -1. The second primer sequence in
    the FASTA file is always optional. Example: AAACTYAAAKGAATTGRCGG and
    ACGGGCGGTGTGTRC for the 926F and 1392R primers that target the V6 to
    V9 region of the 16S rRNA gene.

**-un \<unidirectional\> | -unidirectional \<unidirectional\>**

:   Instead of producing reads bidirectionally, from the reference
    strand and its reverse complement, proceed unidirectionally, from
    one strand only (forward or reverse). Values: 0 (off, i.e.
    bidirectional), 1 (forward), -1 (reverse). Use \<unidirectional\> =
    1 for amplicon and strand-specific transcriptomic or proteomic
    datasets. Default: 0

**-lb \<length\_bias\> | -length\_bias \<length\_bias\>**

:   In shotgun libraries, sample reference sequences proportionally to
    their length. For example, in simulated microbial datasets, this
    means that at the same relative abundance, larger genomes contribute
    more reads than smaller genomes (and all genomes have the same fold
    coverage). 0 = no, 1 = yes. Default: 1

**-cb \<copy\_bias\> | -copy\_bias \<copy\_bias\>**

:   In amplicon libraries where full genomes are used as input, sample
    species proportionally to the number of copies of the target gene:
    at equal relative abundance, genomes that have multiple copies of
    the target gene contribute more amplicon reads than genomes that
    have a single copy. 0 = no, 1 = yes. Default: 1

**-md \<mutation\_dist\>... | -mutation\_dist \<mutation\_dist\>...**

:   Introduce sequencing errors in the reads, under the form of
    mutations (substitutions, insertions and deletions) at positions
    that follow a specified distribution (with replacement): model
    (uniform, linear, poly4), model parameters. For example, for a
    uniform 0.1% error rate, use: uniform 0.1. To simulate Sanger
    errors, use a linear model where the errror rate is 1% at the 5' end
    of reads and 2% at the 3' end: linear 1 2. To model Illumina errors
    using the 4th degree polynome 3e-3 + 3.3e-8 \* i\^4 (Korbel et al
    2009), use: poly4 3e-3 3.3e-8. Use the \<mutation\_ratio\> option to
    alter how many of these mutations are substitutions or indels.
    Default: uniform 0 0

**-mr \<mutation\_ratio\>... | -mutation\_ratio \<mutation\_ratio\>...**

:   Indicate the percentage of substitutions and the number of indels
    (insertions and deletions). For example, use '80 20' (4
    substitutions for each indel) for Sanger reads. Note that this
    parameter has no effect unless you specify the \<mutation\_dist\>
    option. Default: 80 20

**-hd \<homopolymer\_dist\> | -homopolymer\_dist \<homopolymer\_dist\>**

:   Introduce sequencing errors in the reads under the form of
    homopolymeric stretches (e.g. AAA, CCCCC) using a specified model
    where the homopolymer length follows a normal distribution N(mean,
    standard deviation) that is function of the homopolymer length n:

          Margulies: N(n, 0.15 * n)              ,  Margulies et al. 2005.
          Richter  : N(n, 0.15 * sqrt(n))        ,  Richter et al. 2008.
          Balzer   : N(n, 0.03494 + n * 0.06856) ,  Balzer et al. 2010.

    Default: 0

**-cp \<chimera\_perc\> | -chimera\_perc \<chimera\_perc\>**

:   Specify the percent of reads in amplicon libraries that should be
    chimeric sequences. The 'reference' field in the description of
    chimeric reads will contain the ID of all the reference sequences
    forming the chimeric template. A typical value is 10% for amplicons.
    This option can be used to generate chimeric shotgun reads as well.
    Default: 0 %

**-cd \<chimera\_dist\>... | -chimera\_dist \<chimera\_dist\>...**

:   Specify the distribution of chimeras: bimeras, trimeras, quadrameras
    and multimeras of higher order. The default is the average values
    from Quince et al. 2011: '314 38 1', which corresponds to 89% of
    bimeras, 11% of trimeras and 0.3% of quadrameras. Note that this
    option only takes effect when you request the generation of chimeras
    with the \<chimera\_perc\> option. Default: 314 38 1

**-ck \<chimera\_kmer\> | -chimera\_kmer \<chimera\_kmer\>**

:   Activate a method to form chimeras by picking breakpoints at places
    where k-mers are shared between sequences. \<chimera\_kmer\>
    represents k, the length of the k-mers (in bp). The longer the kmer,
    the more similar the sequences have to be to be eligible to form
    chimeras. The more frequent a k-mer is in the pool of reference
    sequences (taking into account their relative abundance), the more
    often this k-mer will be chosen. For example, CHSIM (Edgar et al.
    2011) uses this method with a k-mer length of 10 bp. If you do not
    want to use k-mer information to form chimeras, use 0, which will
    result in the reference sequences and breakpoints to be taken
    randomly on the "aligned" reference sequences. Note that this option
    only takes effect when you request the generation of chimeras with
    the \<chimera\_perc\> option. Also, this options is quite memory
    intensive, so you should probably limit yourself to a relatively
    small number of reference sequences if you want to use it. Default:
    10 bp

**-af \<abundance\_file\> | -abundance\_file \<abundance\_file\>**

:   Specify the relative abundance of the reference sequences manually
    in an input file. Each line of the file should contain a sequence
    name and its relative abundance (%), e.g. 'seqABC 82.1' or 'seqABC
    82.1 10.2' if you are specifying two different libraries.

**-am \<abundance\_model\>... | -abundance\_model \<abundance\_model\>...**

:   Relative abundance model for the input reference sequences: uniform,
    linear, powerlaw, logarithmic or exponential. The uniform and linear
    models do not require a parameter, but the other models take a
    parameter in the range [0, infinity). If this parameter is not
    specified, then it is randomly chosen. Examples:

          uniform distribution: uniform
          powerlaw distribution with parameter 0.1: powerlaw 0.1
          exponential distribution with automatically chosen parameter: exponential

    Default: uniform 1

**-nl \<num\_libraries\> | -num\_libraries \<num\_libraries\>**

:   Number of independent libraries to create. Specify how diverse and
    similar they should be with \<diversity\>, \<shared\_perc\> and
    \<permuted\_perc\>. Assign them different MID tags with
    \<multiplex\_mids\>. Default: 1

**-mi \<multiplex\_ids\> | -multiplex\_ids \<multiplex\_ids\>**

:   Specify an optional FASTA file that contains multiplex sequence
    identifiers (a.k.a MIDs or barcodes) to add to the sequences (one
    sequence per library). The MIDs are included in the length specified
    with the -read\_dist option and can be altered by sequencing errors.
    See the MIDesigner or BarCrawl programs to generate MID sequences.

**-di \<diversity\>... | -diversity \<diversity\>...**

:   This option specifies alpha diversity, specifically the richness,
    i.e. number of reference sequences to take randomly and include in
    each library. Use 0 for the maximum richness possible (based on the
    number of reference sequences available). Provide one value to make
    all libraries have the same diversity, or one richness value per
    library otherwise. Default: 0

**-sp \<shared\_perc\> | -shared\_perc \<shared\_perc\>**

:   This option controls an aspect of beta-diversity. When creating
    multiple libraries, specify the percent of reference sequences they
    should have in common (relative to the diversity of the least
    diverse library). Default: 0 %

**-pp \<permuted\_perc\> | -permuted\_perc \<permuted\_perc\>**

:   This option controls another aspect of beta-diversity. For multiple
    libraries, choose the percent of the most-abundant reference
    sequences to permute (randomly shuffle) the rank-abundance of.
    Default: 0 %

**-rs \<random\_seed\> | -random\_seed \<random\_seed\>**

:   Seed number to use for the pseudo-random number generator.

**-dt \<desc\_track\> | -desc\_track \<desc\_track\>**

:   Track read information (reference sequence, position, errors, ...)
    by writing it in the read description. Default: 1

**-ql \<qual\_levels\>... | -qual\_levels \<qual\_levels\>...**

:   Generate basic quality scores for the simulated reads. Good residues
    are given a specified good score (e.g. 30) and residues that are the
    result of an insertion or substitution are given a specified bad
    score (e.g. 10). Specify first the good score and then the bad score
    on the command-line, e.g.: 30 10. Default:

**-fq \<fastq\_output\> | -fastq\_output \<fastq\_output\>**

:   Whether to write the generated reads in FASTQ format (with
    Sanger-encoded quality scores) instead of FASTA and QUAL or not (1:
    yes, 0: no). \<qual\_levels\> need to be specified for this option
    to be effective. Default: 0

**-bn \<base\_name\> | -base\_name \<base\_name\>**

:   Prefix of the output files. Default: grinder

**-od \<output\_dir\> | -output\_dir \<output\_dir\>**

:   Directory where the results should be written. This folder will be
    created if needed. Default: .

**-pf \<profile\_file\> | -profile\_file \<profile\_file\>**

:   A file that contains Grinder arguments. This is useful if you use
    many options or often use the same options. Lines with comments (\#)
    are ignored. Consider the profile file, 'simple\_profile.txt':

          # A simple Grinder profile
          -read_dist 105 normal 12
          -total_reads 1000

    Running: grinder -reference\_file viral\_genomes.fa -profile\_file
    simple\_profile.txt

    Translates into: grinder -reference\_file
    viral\_genomes.fa -read\_dist 105 normal 12 -total\_reads 1000

    Note that the arguments specified in the profile should not be
    specified again on the command line.

------------------------------------------------------------------------

CLI OUTPUT
==========

For each shotgun or amplicon read library requested, the following files
are generated:

-   A rank-abundance file, tab-delimited, that shows the relative
    abundance of the different reference sequences

-   A file containing the read sequences in FASTA format. The read
    headers contain information necessary to track from which reference
    sequence each read was taken and what errors it contains. This file
    is not generated if \<fastq\_output\> option was provided.

-   If the \<qual\_levels\> option was specified, a file containing the
    quality scores of the reads (in QUAL format).

-   If the \<fastq\_output\> option was provided, a file containing the
    read sequences in FASTQ format.

------------------------------------------------------------------------

API EXAMPLES
============

The Grinder API allows to conveniently use Grinder within Perl scripts.
Here is a synopsis:

      use Grinder;

      # Set up a new factory (see the OPTIONS section for a complete list of parameters)
      my $factory = Grinder->new( -reference_file => 'genomes.fna' );

      # Process all shotgun libraries requested
      while ( my $struct = $factory->next_lib ) {

        # The ID and abundance of the 3rd most abundant genome in this community
        my $id = $struct->{ids}->[2];
        my $ab = $struct->{abs}->[2];

        # Create shotgun reads
        while ( my $read = $factory->next_read) {

          # The read is a Bioperl sequence object with these properties:
          my $read_id     = $read->id;     # read ID given by Grinder
          my $read_seq    = $read->seq;    # nucleotide sequence
          my $read_mid    = $read->mid;    # MID or tag attached to the read
          my $read_errors = $read->errors; # errors that the read contains
     
          # Where was the read taken from? The reference sequence refers to the
          # database sequence for shotgun libraries, amplicon obtained from the
          # database sequence, or could even be a chimeric sequence
          my $ref_id     = $read->reference->id; # ID of the reference sequence
          my $ref_start  = $read->start;         # start of the read on the reference
          my $ref_end    = $read->end;           # end of the read on the reference
          my $ref_strand = $read->strand;        # strand of the reference
          
        }
      }

      # Similarly, for shotgun mate pairs
      my $factory = Grinder->new( -reference_file => 'genomes.fna',
                                  -insert_dist    => 250            );
      while ( $factory->next_lib ) {
        while ( my $read = $factory->next_read ) {
          # The first read is the first mate of the mate pair
          # The second read is the second mate of the mate pair
          # The third read is the first mate of the next mate pair
          # ...
        }
      }

      # To generate an amplicon library
      my $factory = Grinder->new( -reference_file  => 'genomes.fna',
                                  -forward_reverse => '16Sgenes.fna',
                                  -length_bias     => 0,
                                  -unidirectional  => 1              );
      while ( $factory->next_lib ) {
        while ( my $read = $factory->next_read) {
          # ...
        }
      }

------------------------------------------------------------------------

API METHODS
===========

The rest of the documentation details the available Grinder API methods.

new
---

Title : new

Function: Create a new Grinder factory initialized with the passed
arguments. Available parameters described in the OPTIONS section.

Usage : my \$factory = Grinder-\>new( -reference\_file =\> 'genomes.fna'
);

Returns : a new Grinder object

next\_lib
---------

Title : next\_lib

Function: Go to the next shotgun library to process.

Usage : my \$struct = \$factory-\>next\_lib;

Returns : Community structure to be used for this library, where
\$struct-\>{ids} is an array reference containing the IDs of the genome
making up the community (sorted by decreasing relative abundance) and
\$struct-\>{abs} is an array reference of the genome abundances (in the
same order as the IDs).

next\_read
----------

Title : next\_read

Function: Create an amplicon or shotgun read for the current library.

Usage : my \$read = \$factory-\>next\_read; \# for single read my
\$mate1 = \$factory-\>next\_read; \# for mate pairs my \$mate2 =
\$factory-\>next\_read;

Returns : A sequence represented as a Bio::Seq::SimulatedRead object

get\_random\_seed
-----------------

Title : get\_random\_seed

Function: Return the number used to seed the pseudo-random number
generator

Usage : my \$seed = \$factory-\>get\_random\_seed;

Returns : seed number

------------------------------------------------------------------------

COPYRIGHT
=========

Copyright 2009-2012 Florent ANGLY \<<florent.angly@gmail.com>\>

Grinder is free software: you can redistribute it and/or modify it under
the terms of the GNU General Public License (GPL) as published by the
Free Software Foundation, either version 3 of the License, or (at your
option) any later version. Grinder is distributed in the hope that it
will be useful, but WITHOUT ANY WARRANTY; without even the implied
warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
GNU General Public License for more details. You should have received a
copy of the GNU General Public License along with Grinder. If not, see
\<http://www.gnu.org/licenses/\>.

------------------------------------------------------------------------

BUGS
====

All complex software has bugs lurking in it, and this program is no
exception. If you find a bug, please report it on the SourceForge
Tracker for Grinder:
[http://sourceforge.net/tracker/](http://sourceforge.net/tracker/?group_id=244196&atid=1124737)

Bug reports, suggestions and patches are welcome. Grinder's code is
developed on Sourceforge
([http://sourceforge.net/scm/](http://sourceforge.net/scm/?type=git&group_id=244196))
and is under Git revision control. To get started with a patch, do:

       git clone git://biogrinder.git.sourceforge.net/gitroot/biogrinder/biogrinder
