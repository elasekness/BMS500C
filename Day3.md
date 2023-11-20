## Tutorial 3

**Ojective:** to generate a phylogeny from a multi-sequence alignment that includes our consensus genome and other West Nile Virus genomes.

A phylogeny depicts the evolutionary relationship among taxa.  We can use them to understand the evolution of traits, patterns of disperal/migration and diversification, rates of evolution, and much more.  Phylogenies can be built with morphological or molecular data or a combination of both.  Phylogenies rely on comparing homologous characters (in our case genomes), or those that share a common ancestor. Our genomes must be aligned so that homologous sites in each genome are matched to each other.  Typically, we are trying to optimize the similarity among sequences will minimizing the number of gaps that must be inserted to achieve the alignment. Both alignment and phylogenetic software can incorporate models as to how we think character states evolve and include parameters, such as the probabilities of changing between character states (ex. the probability of a character changing from an A to a T). Thus, our models explain how are data have been created. One method of inferring phylogenies that relies on models of evolution is the Maximum Likelihood (ML) method. Theoretically, an ML program would test every single possible tree given our data and find the one that maximizes the likelihood that our evolutionary model generated the data.  The resulting phylogeny is an acyclic graph, consisting of nodes connected by edges with only one path from one node to another. Our phylogeny is bifurcating, in that one parental node gives rise to two daughter nodes, each connected to the parental node through an edge or a branch.  The length of the branch can represent time or the amount of change that has occurred from parental to daughter node.  We root our trees to give them directionality (where we assign one node to be older than all others, it is the common anceestor to all other nodes in our tree), such that moving from the tips of our tree to deeper and deeper internal nodes represents moving further and further back in time. For an excellent review of all things phylogeny, see Casey Dunn's online book [Phylogenetic Biology](http://dunnlab.org/phylogenetic_biology/).

Add your consensus genome to the contextual sequences and align your genomes with [mafft](https://mafft.cbrc.jp/alignment/software/).

	cat wnv[A-G].fasta wnv_contextual.fasta > wnv_genomes.fasta
	mafft wnv_genomes.fasta > wnv_genomes.aln.fasta

> We concatendated our consensus genome with our contextual sequenes and saved them to a new file. <br>
> We could have also appended our consensus genome to the contextual sequences file or visa versa:<br>
	cat wnv[A-G].fasta >> wnv_contextual.fasta
> The **`>>`** indicates appending information to a file instead of overwriting it.

Generate an ML phylogeny with [IQ-Tree](http://www.iqtree.org/).

	iqtree -s wnv_genomes.aln.fasta -bb 1000

> This command will test hundreds of nucleotide substitution models to determine the one that is the best-fit. It will then use this model to infer the relationships among our genomes. The support for these relationships are evaluated by performing 1000 ultra-fast bootstrap replicates. <br>
> Bootstrapping is a way of resampling our data to evluate how many times the relationships in our ML tree are returned. <br>
> Your ML tree is in a file called `wnv_genomes.aln.fasta.treefile` written in [Newick format](https://en.wikipedia.org/wiki/Newick_format).
> Since the treefile is simply a textfile, you can view it with any text editor or a command such as `more` or we can open it in a program such as [FigTree](https://github.com/rambaut/figtree/releases), which will translate the information in our file into a graphical depiction.

