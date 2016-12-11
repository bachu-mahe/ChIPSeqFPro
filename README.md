# ChIPSeqFPro

ChiPSeqFPro (short from ChIP-Seq Full Processing) is a pipeline that will perform full processing of ChIPSeq data starting from the fastq.gz files. It performs fastqc quality control, mapping to the human genome hg19 or mouse mm10 using bwa, sam to bam conversion, peak calling with MACS, and finally creates bigwig files from bam files using bam2BigWig tool.

#Dependencies

**Place fastqc.gz in a working folder**

<pre>
mkdir work.folder
cp path-to-files/*fastq.gz work.folder
</pre>

**FastQC**

Instalation (Linux), place FastQC folder in working directory:

<pre>
cd work.folder
wget http://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.5.zip
unzip fastqc_v0.11.5.zip
chmod 755 ./FastQC/fastqc
</pre>

**Reference genome**

Download the reference genome, in this example it is human hg19:

<pre>
mkdir ~/reference_genomes
cd ~/reference_genomes
mkdir hg19
cd hg19
wget --timestamping 
        'ftp://hgdownload.cse.ucsc.edu/goldenPath/hg19/bigZips/hg19.2bit ' 
        -O hg19.2bit 
wget http://hgdownload.cse.ucsc.edu/admin/exe/linux.x86_64/twoBitToFa
chmod 755 twoBitToFa
./twoBitToFa hg19.2bit hg19.fa
</pre>

For the mouse genome:

<pre>
mkdir mm10
cd mm10
wget --timestamping 
        ' http://hgdownload.cse.ucsc.edu/goldenPath/mm10/bigZips/mm10.2bit' 
        -O mm10.2bit 	
./twoBitToFa mm10.2bit mm10.fa
</pre>

**Install BWA***

<pre>
#Download to your ~ folder the latest version from http://sourceforge.net/projects/bio-bwa/files/
bunzip2 bwa-0.7.15.tar.bz2 
tar xvf bwa-0.7.15.tar
cd bwa-0.7.15
make

#edit ~/.bashrc to add bwa to your PATH 
nano ~/.bashrc
export PATH=$PATH:~/bwa-0.7.15
source ~/.bashrc
#test the installation
bwa
</pre>

**Indexing the reference genome with BWA**

Use BWA to index the reference genome, use number of core on your machine, e.g. 64.
<pre>
cd ~/reference_genomes
#for human genome
bwa index -a bwtsw hg19.fa
#for mouse genome
bwa index -a bwtsw mm10.fa
</pre>

**Running**

After placing fastq.gz files in the working folder run the script that is suitable for your experiment, e.g: 

<pre>
chmod 755 ChIPSeqFPro.PE.hg19.sh
./rnaSeqFPro.PE.hg19.sh 64
</pre>

The number indicates number of cores you want to allocate for the analysis.

**Don't forget to place the FastQC folder into the working folder! These are the only requirment neccesary to be in the working directory, in addition to the fastqc.gz files**

**Dont forget that reference genome needs to be in your ~/reference_genomes folder, in case you switch to another user account script may not work because it searches for ~/reference_genomes folder.**
