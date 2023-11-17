## Tutorial 1

**Objective:** Successfully log on to Google Cloud Platform (GCP) Console and become familiar with working in a command-line (Linux) environment.

<br>

## Opening a terminal window

We are working on remote servers or virtual machines (VMs) hosted by Google.
To open a terminal window where we will work, we need to log on to the GCP console with our Wadsworth credentials,
start the VM associated with our Project and `ssh` to those servers.
`ssh` is a secure shell protocol for safely connecting to remote services.
To `ssh` you need an account on the server with a username and password as well as the IP address of the server.

- Navigate to the [GCP console](https://console.cloud.google.com) and log on with your Wadsworth credentials
- Once connected to the console, click the Compute Engine link and start your VM by expanding the three dot icon to the right
- Click the SSH button once the VM has started. This will open a terminal window on your VM.

<br>

To perform any operations in a Linux environment, we need to tell the computer what to do by typing specific commands into our terminal window.
In general, the syntax will be: `Command File`,
where `Command` is the function or operation you want to perform on the` File` or `Directory`
that you specify. Sometimes specifying a command is all we need.

<br>

Examine the contents of your directory with:
 
	ls

 > **`ls`** = list

<br>


## Directory structure and navigation

Directories are folders with specific locations on the server.
To navigate from one directory to another, we must specify the location of the directory or its path.
Directories have a forward slash `/` after their names.  Thus to get to a subdirectory within a directory
you would specify the path by stringing together the directory names, separated by `/`. We'll practice below

<br>

First, determine the name and location of your home directory.

	echo $HOME

> **`echo`** = repeat, **`$HOME`** = variable name for your home directory and its location on the server. <br>
> Try the **`echo`** command with other variable names, such as **`$SHELL`** or **`$PATH`**.

<br>

You can also print your working directory (where you are).
 
	pwd

> **`pwd`** = print working directory

<br>

Make a directory within your home directory called `genomes`.

	mkdir genomes

> **`mkdir`** = make directory

<br>

Now try making two directories at the same time, one named `fastq` and the other named `reference`.

<br>

Change (move) to a different directory.

	cd genomes

> **`cd`** = change directory. Use **`../`** or **`..`** to move up one directory (back to your home directory). What does **`cd`** alone do? <br>
> You can think of changing directories as physically moving from on directory to another, which means your point of reference has changed. This will become more evident in the upcoming examples. <br>
> For now, try listing the contents of the directory above yours from your home directory. Then **`cd`** to the directory above yours and list the contents of your home directory.

<br>


## Permissions


Directories and files have specific permissions associated with them or things that you are allowed to do to them.
There are three permissions: 1) the ability to read (r) 2) the ability to write (w) and 3) the ability to execute (a script or program; x).
There three sets of permissions representing 1) the User (you), 2) the group (multiple users),
and 3) Others (Everyone else in the world).

<br>

Examine the permissions of your 'genomes' directory.

	ls -lh genomes

> **`ls -l`** = long list, providing you information on when the **`genomes`** directory was created and its associated permissions. <br>
> the **`-h`** specifies that the output be in human-readable format.

<br>

Change the permissions associated with your `genomes` directory with the 'chmod' command (change mode).

	chmod 775 genomes

> Permissions are represented by three-digit (for user, group, and other) octal numbers.
> Here we are allowing the user and group universal permissions (7 = read, write, and execute) and all others
> the ability to read and write only (5).
> For more information on permissions, see: [Linux permissions](https://www.guru99.com/file-permissions.html#linux_file_ownership)

<br>


## Manipulating files (making, (re)moving, editing, and viewing)

There are many ways to make and view files in a Linux OS (operating system).
We can redirect output from a command that prints its output to your screen (STDOUT) to a file instead,
we can generate files on the fly by opening them in a text editor, or we can copy an existing file.
Similarly, we can view and edit files in a text editor or we can print their contents to the screen with various command-line options.
As a general rule, it's always good to examine some of the contents of your file to ensure you've generated the results you want in the
format you want. Or that you are using the correct file and format for downstream applications.

<br>

Redirect STDOUT to a file.

	ls /usr/bin > programs.txt

> The path **`/usr/bin`** specifies the location where various Bash commands are found. When you type a command, **`/usr/bin`** is one of the locations your computer searches to find and execute the command. <br>
> Was **`/usr/bin`** part of your **`$PATH`**? <br>
> Here we are redirecting the STDOUT from the **`ls`** command to a file named **`programs.txt`**. The **`>`** sign is responsible for the redirection.

<br>

Scroll through the contents of your file.

	more programs.txt

> Scroll through line-by-line with the enter key.  Scroll through page-by-page with the space key. <br>
> Do you notice that the file contains some of the commands you have just used? <br>
> Exit with **`control-c`**.

<br>

Display the first ten lines of your file.

	head programs.txt

> **`head`** displays the first ten lines by default but you can specify the number of lines with a flag. <br>
> For example, **`head -200 programs.txt`** will display the first two hundred lines of your file. In general, most
> commands have additional arguments (or flags) associated with them. <br>
> You can see the different usage statements by typing the command with a **`-help`** or **`--help`** option.

<br>

Display the last ten lines of your file.

	tail programs.txt

<br>

Print the entire contents of your file to your screen.

	cat programs.txt

> **`cat`** = concatenate.  The **`cat`** command can also join the contents of multiple files together.

<br>

Now try copying files from one directory to another.  Here we will copy files required for our future West Nile Virus (WNV) genome analysis.
We will copy a fasta file containing contextual WNV genomes to our `genomes` directory and a fasta file conaining a reference genome 
(which will be used to generate a reference assembly) to our `reference` directory.
The files are located in different subdirectories within the directory entitled `BMS500-2023`.  Like all of your home directories, `BMS500-2023` is itself 
located in the `/home` directory. We will have to specify the path to these files.  We can specify an absolute path, which is the location
of these files with respect to the root directory (i.e. going through the entire filesystem to get to your file) or a relative path, which is
where these files are located with respect to your current working directory (i.e. where you are when you enter `pwd`).

<br>

Let's try using absolute and relative paths.

	cp /home/BMS500-2023/genomes/wnv_contextual.fasta genomes

> **`cp`** = copy. <br>
> Here we used the absolute path to copy the fasta file `wnv_contextual.fasta` to our `genomes` directory.

<br>

Now `cd` into your `reference` directory and use a relative path to copy the reference fasta file (HQ596519.fasta) to where you are.

	cp ../../BMS500-2023/reference/HQ596519.fasta .

> Notice that we had to move up two directories to get to `BMS500-2023`. <br>
> Also notice that we must always specify an end location for our copied files but in this case, we are copying the file to our current location, which is specified with a dot `.`.

<br>

View and edit the contents of a file with a text editor. Let's open our reference fasta file and change the sequence name to 'reference.'

	nano HQ596519.fasta

> Nano, emacs, vim, and vi are all text editors. <br>
> You can make an empty file on the fly by typing **`nano`** or **`nano filename`**.  This will open a blank text editor screen. <br>
> Save your changes with **`control-o`**. <br>
> To exit the text editor, use **`control-x`**. <br>
> More information on Nano commands can be found here: 
> [Nano](https://www.howtogeek.com/howto/42980/the-beginners-guide-to-nano-the-linux-command-line-text-editor/)

<br>

Rename a file. Let's rename our reference file to reflect the changes we made in nano to the sequence name.

	mv HQ596519.fasta reference.fasta

> **`mv`** = move. Renaming files with `mv` will overwrite any existing file.  You can also mv a file to a different directory. <br>
> Try it: **`mv reference.fasta genomes`** <br>
> Can you move the file back to your home directory?

<br>

Remove a file. Let's remove files we don't need.

	rm programs.txt

> **`rm`** = remove.  Remember, a removed file cannot be restored. <br>
> Can you remove a file from a different directory without having to change directories? <br>
> How would you remove a directory?

<br>


## Notes on working in a Linux environment


* Avoid creating file names and directories with spaces. Use underscores, dashes, or periods instead to separate multi-part names.
* Spaces need to be escaped in Linux (more on that later).  For example, if you tried to make a directory called “my directory”,
* mkdir would make two directories, `my` and `directory`.
* Use autocomplete for speedier typing and to avoid typos.  Autocomplete will fill in the unique part of a command or file.
For example, if I had only one file in my directory that began with a “b,” I could type `b` and then press the tab key to autocomplete the name of the directory.
* Everything in Linux is case sensitive
* Can’t find a command?
Try `which` to see if the command is in your path – whether the command is in a location that the computer searches for executing commands.
* Hit the up-arrow key to recall a command you entered previously.

<br>

## Databases and obtaining sequences


There are several sequence databases – NCBI, JGI, EMBL - that you might encounter.
You might want to explore each of these to familiarize yourself with the resources they offer.
We will focus on NCBI.  Our goal is to download the same reference genome that you copied to your `reference` directory. We also want to download the accompanying annotation file (gff file), which provides a description of the genes, the function of the coding sequences, and the start and stop positions of the genes in the genome.


Navigate to NCBI’s homepage: [https://www.ncbi.nlm.nih.gov/](https://www.ncbi.nlm.nih.gov/)


> Notice that there are options to submit sequences, download sequences, and even analyze data. <br>
> BLAST is an alignment tool to look for sequences that are similar (a proxy for homology) to your queries.


Choose 'Nucleotide' under the top left pull-down menu (set to 'All Databases' by default), type West Nile Virus into the search area, and hit enter. This brings us to a page containing over 50,000 nucleotide entries for WNV.  Many of these are only partical genomes or partial coding sequences, which we don't want. We could use the options in the left-side panel to filter the list further but this will be of limited use here. Clearly we need a better strategy.  If we click on the `NCBI Virus` botton located in the taxonomy panel, we are brought to a far more useful page that lists all WNV genomes and allows us to filter the data by several criteria, such as collection date, accession number, genome completeness, and host among others.  Since we know the accession number of our genome, we can type this into the Accession box and our genome appears.  If we click on the genome accession, we obtain more information about the sequence.  If we scroll up to the `Download` button, we can download the nucleotide sequence in fasta format.
We could download this sequence to our computers and then upload it to our VMs but this is a two-step process.
An easier way would be to use one of NCBI's tools for interacting with their databases.

<br>

Return to your VM terminal and type:

	efetch --help

 > This brings up a long menu of options for the efetch tool, which can be used to download a variety of data in different formats from NCBI. <br>
> The relevant arguments for us will be the database `-db` we want to search, the format `-format` of the data, and the `-id` of our query, which is the accession of our WNV reference gnome.

<br>

Let's download our sequence and redirect STDOUT to a file.

	efetch -db nuccore -format fasta -id HQ596519 > HQ596519.fasta

 > Remember that STDOUT is output from a command that gets printed to your screen while the `>` symbol redirects this output to a file.

<br>

Although we aren't working with eukaryotic or prokaryotic genomes, it's worth mentioning that there is a command-line
tool to download these as well: the **`datasets`** command-line tool. A help menu will appear if you type **`datasets`** without any arguments.  Typing **`datasets download`** will give you additional information on how to use this command, which shows the option to download a genome by its accession.  


As you can see, there are usually multiple ways to solve a problem in bioinformatics.

<br>


## Obtaining reads from the SRA


Now let's download the raw reads for a WNV library from the SRA (sequence read archive) database.  We'll map the reads from WNV sequencing libraries on to our reference genome. Typically, any published NGS data must also be submitted to the SRA. Each sample/specimen sequenced will have a BioSample accession number. Biosample information provides associated metadata. The SRA and Biosample for each submission are further housed under a BioProject, which can contain multiple submissions from the same study or experiment. Because the SRA entries often contain information about the origin of the sample and are linked to additional metadata, I'm not going to have you download the libraries you'll be using to generate refence-based assemblies.  You'll copy these renamed files from the `BMS500-2023` directory. 

Return to the [NCBI homepage](https://www.ncbi.nlm.nih.gov/), select 'SRA' from the dropdown menu and search for 'West Nile Virus'. This results in over 5,000 entries.  Let's filter further by selecting options in the menu to the left. For 'File type' select 'fastq', for 'Strategy' select 'genome', and for 'Plastform' select 'Illumina.' We want paired-end (PE) data as well but notice that all 'Illumina' libraries are PE.  Click on the link for the first library entitled [Other Sequencing of West Nile Virus](https://www.ncbi.nlm.nih.gov/sra/SRX13440230[accn]).

This brings you to a page with additional information on the sequencing run as well as links to the [Bioproject](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA231221) and [Biosample](https://www.ncbi.nlm.nih.gov/biosample/SAMN24061100).

By clicking on the link to the run [SRR17262079](https://trace.ncbi.nlm.nih.gov/Traces/?view=run_browser&acc=SRR17262079&display=metadata), we could download the two fastq files associated with this run and then upload them to our VM or we could use faster tools provided by NCBI.

<br>

Use **`prefetch`** and **`fasterq-dump`** tools from the SRA toolkit to download the fastq files for SRR17262079.

	prefetch SRR17262079
	fasterq-dump SRR17262079

> **`prefetch`** will download the SRA data in binary format and **`fasterq-dump`** will perform the fastq conversion. <br>
> Notice that the conversion tool automatically saves forward and reverse reads to separate files. <br>
> Although we could use fasterq by itself, NCBI claims prefetch in combination with fasterq is faster. <br>
> More information on the SRA-toolkit and other frequently used tools can be found here: [SRA toolkit](https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc)

If you haven't done so already, try moving your fastq files to your `fastq` directory from your current location.

<br>


## Manipulating data: Parsing files, modifying content, and piping


Often, we want to extract information from and/or alter the contents of a file. We might also want to determine some basic features of the file. For example, we might want to know how many sequences are in our fasta file without having to open and scroll through a file. You will probably use the following commands most frequently to parse and manipulate files – grep, sed, cut, paste, sort and uniq. These commands can perform simple routines such as search and replace but combined with regular expressions, these tools are incredibly powerful and efficient.

Piping is specified by **`|`** and simply pipes the STDOUT from one command to another so that you can string multiple operations together on one line. Sometimes the most challenging bioinformatics operations are wrangling your data into the proper format.

<br>


`cd` to your `genomes` directory and count how many sequences are in the `wnv_contextual.fasta` file.

	grep -c ">" wnv_contextual.fasta

> **`grep`** = global regular expression print.  Grep searches a file line-by-line for patterns that you specify and returns every line containing that pattern. <br>
> The **`-c`** option counts the number of lines that contain the search pattern instead of returning the lines. <br>
> Try **`grep`** without the **`-c`** argument to see the difference. <br>
> Combined with metacharacters, **`grep`** is a powerful way to search your document for complicated patterns.

<br>

Grab the first 5 header lines from your fasta file with `grep` and a pipe.

	grep  ">" wnv_contextual.fasta | head -5

> Here we are using a pipe, designated by **`|`** to capture the output of grep and pass it to another command (**`head`**).
> Piping is a really useful skill to learn for parsing and processing data more efficiently. <br>
> Note that you can string many pipes together, if necessary. As is the case for most operations conducted in Linux, there are multiple ways to do things. <br>
> Use the manual page for grep to find an alternative way to obtain the first five header lines (**`man grep`**).

<br>

Count the number of sequences in the fasta file using a pipe to `wc` instead.

	grep ">" wnv_contextual.fasta | wc -l

 > Here we are passing the output of **`grep`** to the word count command, **`wc`**.  The **`-l`** argument specifies that we want to count lines. <br>
> Again, there is more than one way to achieve the same outcome in Linux.

<br>

Use **`sed`** to rename the definition lines of your fasta file so that only the accessions remain.


	sed "s/|.*//" wnv_contextual.fasta

> **`sed`** = stream editor.  **`sed`** is essentially a search and replace function. <br>
> Like **`grep`**, we can search for complicated patterns when we use this command with regular expressions. Unlike `grep`, we can replace these complicated patterns with another. <br>
> The syntax for the search and replace command is **`'s/search pattern/replacement pattern/'`** where the 's' stands for substitute. <br>
> In our example, the fasta file contains definition lines that begin with genome accessions, proceeded by metadata separated from the accession with a space and a `|`. The format of the metadata varies among the definition lines. <br>
> We can use a regular expression to replace all of the patterns displayed in the sequence names without having to search for each pattern individually. Regular expressions use characters with special meaning (metacharacters). <br>
> Like **`grep`**, **`sed`** will search for your pattern line by line. It will make a replacement once (unless you specify otherwise, see the manual page). <br>
> Here we search for a **`|`** and everything that proceeds it, specified by two metacharacters: **`.`** and **`*`**. The `.` represents any character one time and the `*` is a greedy character that represents any character any number of times. <br>
> If we wanted to specify an actual period and not a special character, we would need to escape the metacharacter with a preceeding slash: `\.` <br>
> Brackets also have special meaning, specifying ranges of numbers, letters, or both. Additional metacharacters and their meanings are listed below.


As you can see (but not very well) **`sed`** will print your entire document to STDOUT with the replacements made. However, it prints the entire file to STDOUT, making it hard to find the changes, which are only in the definition lines.

* How can you view just the definition lines you have changed by piping the output of `sed` to another command?
* Do you get the same results if you don't include the `.` in your search pattern?
* How would you remove the 'set:accession' pattern at the end of some of the sequence names?


<br>


## More piping


Let's try some more complicated parsing of our data using various Bash commands and pipes. First, copy the annotation file (HQ596519.gff3) for our reference genome from the `BMS50-2023` directory to your `reference` directory and take a quick view of the gff file (using `head`, `more` or some other option).  A gff file is a tab delimited file that provides a description of the genes, the function of the coding sequences, and the start and stop positions of the genes (and other features) in the genome. One genome feature is listed per line. The lines don't fit on our screen, making it difficult to visualize.

<br>

Extract all coding sequences (CDS) and view their start and stop positions with a combination of 'grep' and 'cut.'

	grep "CDS" HQ596519.gff3 | cut -f 3,4,5

> We have searched for every line that contains **`CDS`** and cut those lines on the third, fourth, and fifth delimiters (delimiters can be anything but the default is a tab).
> This also highlights that **`grep`** is greedy - returning **`CDS`** even if it is part of a larger phrase or word. <br>
> Because WNV RNA is translated as a single polyprotein and then post-translationally cleaved, the description of the coding sequence features in this gff file are labeled as 'CDS' for the polyprotein and 'mature_protein_region_of_CDS' for the 10 cleaved subunits. <br>
> There are a few ways to be more specific about the features we extract.

<br>

Extract only exact matches to **`CDS`** in the third field of the gff file and print the 3rd-5th fields to STDOUT.


	cut -f 3,4,5 HQ596519.gff3 | grep "CDS" | grep -v "mature"

* What does the **`-v`** argument specify in the **`grep`** command?
* What is another approach to getting the same results?

<br>

Find and count only mature protein coding regions.

	grep -wc "mature_protein_region_of_CDS" HQ596519.gff3

 > We specify an exact word match with the **`-w`** argument.

<br>

Count how many mature protein coding regions are on the positive strand (information on strandedness is in the 7th column)

	grep -w "mature_protein_region_of_CDS" HQ596519.gff3 | cut -f 7 | sort | uniq -c

> **`sort`** sorts lines alpha-numerically (by default, this can be changed) and **`uniq -c`** counts the number of times each unique pattern occurs. <br>
> Note that the lines must be sorted in order for **`uniq -c`** to work properly.

<br>


## Bash for loops


Bash for loops are basically little shell scripts that can be excecuted from the command line (Bash is the command-line language we are using). Like all loops, they allow you to automate iterative processes. For example, instead of opening 200 hundred fasta files and manually changing the definition lines in each, I can run a for loop that will open each fasta file and make the changes that I specify.

<br>

The basic syntax is:

	for FILE in *common_file_ending; do command $FILE; done

> The interpretation of this code is: <br>
> For every file that ends in some common ending (such as .txt or .gz), perform (do) some command on that file until there are no more files on which to operate, whereby “done” will exit us from the loop. <br>
> The $ in front of FILE indicates that $FILE is a variable, a placeholder which is referring to each file that enters the loop, just as x is a variable that represents 2 in the equation x = 2. <br>
> The `for`, `in`, `do`, and `done` are required parts of the for-loop syntax.

<br>

Use a 'for loop' to count the lines in your reference fasta and gff file:

	for filn in HQ*; do wc -l $filn; done

> Here we use the greedy metacharacter, **`*`** to specify that we want to count the lines of all files begining with 'HQ'. <br>
> Note that you don't actually need a loop to count the lines in both files.

	wc -l HQ*

<br>


## Regular expressions (regex) and special characters (metacharacters)

Regular expressions are search terms that incorporate special characters to make searches more powerful (both broader and more specific).
Metacharacters have a special meaning and include:

- `*` 	Star is a greedy metacharacter meaning match anything any number of times
- `[]` Brackets are often used to specify a range of numbers or letters to include in a search pattern
- `.` 	A period represents any character once
- `?` 	Match one character
- `$`		End of Line
- `^`		Beginning of line

Usually, we escape special characters with a backslash to interpret them literally.

<br>

## Assignment 1 ##

Now that we're all expert pipers and regex users, can you modify the sequence names in the wnv_contextual.fasta file so that only the accession number proceeded by the geographic location (Country or state) and collection date are displayed?
