# hot_METS
Group Repo for CompBio METS project

# Introduction:
  This is the METS project, a pipeline for microbiome analysis that can be run in the command line. The data we have been given is a set 
  of 101 FASTQ files containing 16S rRNA sequencing reads taken from the fecal samples of study participants in the METS Ghana site and 
  the US site. There are 50 files each for US and Ghana populations and one H2O control. For the purpose of cross team hacking, we have 
  created a folder for you containing just four of these FASTQ files to speed up running of the script. You will also see a silva file in 
  this directory; that is the database to which you will be alligning your reads. You can find the directory at: 
/homes/sgreenwood1/crossteam on the comp bio server. 

*Due to the writing permissions on the compbio server, you must copy this crossteam directory to your own in order for this pipeline to work*
          
# Software Requirements:
This pipeline can be run either as an Rscript through command line or in RStudio. The required package: dada2

# To Import from GitHub:
In addition to the crossteam directory (see Introduction) you will need the crossteam.R file to run the pipeline. 
Import this file from GitHub using:

	git clone https://github.com/greenwood-suzanne/crossteam.git

	git fork https://github.com/greenwood-suzanne/crossteam.git

When you run the pipeline, you will find plots outputted as files in your home directory (see Results).
  
# Implementation:
   The pipeline will take in this directory for input and runs through the following major steps:
      -Filter and trim the raw data: Remove PCR/Sequencing primers, exclude all N bases, exclude all reads with low PHRED scores
			-Evaluate errors: this is a step neessary for getting taxonomy information later on. outputs plots of
			error x quality. (May take a few minutes)
      -Pull out just the unique sequences: Remove duplicates from our filtered data set
      -Merge the paired reads: Here we combine the forward and reverse reads so that we can ascertain taxonomy information 
      in subsequent steps
      -Remove chimeras: We want to have only properly matched forward and reverse reads 
          (Note: it is at this point that we include a tracking function to ensure that we have a sizable number of reads left at this
          point in the pipeline. This is important for the plotting functions that are still to come. If there are too few reads for any 
          files, the taxonomy table will not contain a true taxonomic assignment for that read. It will instead show NA which cannot be 
          plotted later.)
      -plotting with phyloseq: This portion is still being developed. It will output bar graphs of the taxonomic groups present in our
      two sample groups: Ghana and US for comparison.

In order to run the pipeline through the command line, use the crossteam folder as your working directory. The recommended 
command to run the script is: 
			
			nohup Rscript crossteam.R &
			
The nuhup .. & is not *necessary* but it is helpful, because it will append the text output into one nohup.out file. Through that file
you will be able to see all the text output from each individual step in the pipeline. This nohup.out file should be written to your home 
directory. If you would prefer, the code can also be run using RStudio. There, you will be able to view all tables by selecting them from
the environment section on the right. The images will appear under files in your home directory. 
																																																																
# Results:																						
The first few lines of the tables that are produced throughout the running of the pipeline can be viewed in the nohup.out file that
will appear after running is complete.

	Tables:
		-out: out is a table that summarizes the number of reads in each file before and after filtering by PHRED score, 
		removing Ns and removing primers.
		-seqtab: probably not useful to look at for this activity; lists occurances of reads in each file
		-seqtab.nochim: same as seqtab but with chimeras removed. 
		-track: track is a table that shows you how many reads are in each file after each step of the pipeline. 
		Recall that each of the steps that make up the columns of the table are removing "bad" reads whether they be
		full of N bases, duplicates, or contain chimeras.
		-taxa: preliminary taxonomy table. Will likely take several minutes to run.
		-taxa.print: taxonomy table adjusted for display. This contains all the taxonomy information we are able to obtain
		from checking against the silva version 132 database.
			
	PNG Files:
		*note: If there is trouble viewing the image files, the crossteam directory can be copied to your own home directory and 
		viewed using RStudio under the files tab on the right. There is also the option of SCP-ing the images to your own computer
		to view.
		-filtFqual.png: qualilty plot of forward reads after filtering
		-filtRqual.png: qualilty plot of reverse reads after filtering
		-fqual.png: qualilty plot of forward reads before filtering
		-Rqual.png: qualilty plot of reverse reads before filtering
			
You will also notice a new subdirectory "filtered" within your crossteam directory. This contains the filtered files. 
All of your raw data files are unchanged; all filtering/denoising/dereplicating, etc. was done within the filtered subdirectory.
