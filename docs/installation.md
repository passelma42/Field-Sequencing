# Installation  

EPI2ME Labs maintains a collection of bioinformatics workflows tailored to Oxford Nanopore Technologies long-read sequencing data. They are curated and actively maintained by experts in long-read sequence analysis.  
Read all about [EPI2ME workflows here](https://labs.epi2me.io/wfindex/).  
## Installation guide  
#### JAVA  
```shell
sudo apt-get update
sudo apt-get install default-jre #install java on ubuntu
```
### NEXTFLOW  
Nextflow enables scalable and reproducible scientific workflows using software containers. It allows the adaptation of pipelines written in the most common scripting languages.  
> More information can be found on the [NEXTFLOW WEBSITE](https://www.nextflow.io/).
```shell title="Download and install nextflow"
curl -s https://get.nextflow.io | bash # install nextflow
``` 
### GO  
Singularity 3.0 is written primarily in Go, and you will need Go installed to compile it from source.  
> More information can be found on the [Singularity installation guide website](https://docs.sylabs.io/guides/3.0/user-guide/quick_start.html#quick-installation-steps).  

```shell
# Install
	export VERSION=1.11 OS=linux ARCH=amd64 && \
	wget https://dl.google.com/go/go$VERSION.$OS-$ARCH.tar.gz && \
	sudo tar -C /usr/local -xzvf go$VERSION.$OS-$ARCH.tar.gz && \
	rm go$VERSION.$OS-$ARCH.tar.gz

# setup GO environment
	echo 'export GOPATH=${HOME}/go' >> ~/.bashrc && \
	echo 'export PATH=/usr/local/go/bin:${PATH}:${GOPATH}/bin' >> ~/.bashrc && \
	source ~/.bashrc
    
# install dep for dependency resolution
	go get -u github.com/golang/dep/cmd/dep
```
### Singularity  
Singularity is a container platform. It allows you to create and run containers that package up pieces of software in a way that is portable and reproducible. You can build a container using Singularity on your laptop, and then run it on many of the largest HPC clusters in the world, local university or company clusters, a single server, in the cloud, or on a workstation down the hall.  
Your container is a single file, and you don’t have to worry about how to install all the software you need on each different operating system and system.  
> More information can be found on the [Singularity introduction page](https://docs.sylabs.io/guides/3.5/user-guide/introduction.html).  

**Installation**
```shell
export VERSION=3.0.3 && # adjust this as necessary \
    mkdir -p $GOPATH/src/github.com/sylabs && \
    cd $GOPATH/src/github.com/sylabs && \
    wget https://github.com/sylabs/singularity/releases/download/v${VERSION}/singularity-${VERSION}.tar.gz && \
    tar -xzf singularity-${VERSION}.tar.gz && \
    cd ./singularity && \
    ./mconfig
```
**Compile**
```shell
./mconfig && \
    make -C ./builddir && \
    sudo make -C ./builddir install
```  
**Singularity cache folder**  

Save storage space on your computer.  
When you run the singularity container (wf-amplicon for instance), a *work* directory will be created in your project folder. The wf-amplicon uses different containers without you noticing. All container *images* will be downloaded each time you run this wf. And this takes up space on your HD. 

To avoid this, you can create a cache folder where singularity can store these *.img* files and use these instead of downloding them each time you run the wf. The cache foler can then be specified in a nextflow.config file.  OR you can do as the error message expalains is to set the NXF_SINGULARITY_CACHEDI variable in you .bashrc file.  
Both options will be explained below.  

OPTION 1: create nextflow.config file
Follow these steps to do just this:  

1. **Run the workflow for the 1st time**  
	Run the workflow for the firs time and don't bother about this warning:

	!!! WARNING  
		If the cache folder is not set you get this warning:  
		*WARN: Singularity cache directory has not been defined -- Remote image will be stored in the path: /home/path-to/work/singularity -- Use the environment variable NXF_SINGULARITY_CACHEDIR to specify a different location*  

2. **Locate the .img files**  
	The wf-amplicon will create a `./work/singularity` folder in your projcet folder. 
	```shell
	pieter@UGENT-PC:~/Analyses/wf-amplicon/work
	$ ls
	0f  14  24  2d  3f  46  49  54  5b  63  6d  77  7b  80  85  93  9b  9f  a1  a4  af  b5  bc  cc  d6  e5  e8  singularity
	11  1e  2b  3c  40  47  4c  56  5d  6b  71  78  7f  82  89  96  9c  a0  a3  ac  b0  b6  c8  d0  db  e6  ee
	(base)
	pieter@UGENT-PC~/Analyses/wf-amplicon/work
	$ ls singularity/
	ontresearch-medaka-sha61a9438d745a78030738352e084445a2db3daa2a.img       ontresearch-wf-common-sha91452ece4f647f62b32dac3a614635a6f0d7f8b5.img
	ontresearch-wf-amplicon-sha3b6a08c2ed47efd85b17b98762951fffdeee5ba9.img
	```
	In this folder you'll find the `.img` files you need later.  

3. **Copy the `.img` files to a local folder**  
	To avoid this create cache folder to store downloaded images when running epi2me labs. For each new analysis you run with epi2me make sure to put the image in this folder.  
	```shell
	mkdir /path/to/singularity-img
	cp ~/Analyses/wf-amplicon/work/singularity/*.img /path/to/singularity-img # this will copy all img files to your folder created in previou step, don't forget to personalize the paths  
	```
**Create nextflow config file**  
Now it is time to configure nextflow which is used to *stitch* all the singularity containers from wf-amplicon together.  
We will create a file that will be checked running each wf and redirects singularity to find the images stored locally rather than download them each time.  
[More info on nextflow configuration file](https://www.nextflow.io/docs/latest/config.html).  
Process:  
```shell
mkdir /home/sequencing/nextflow-config # /home/sequencing is in this case an example of your user's home directory. User is called *sequencer* in this example.  
touch /home/sequencing/nextflow-config/nextflow.config
```
```shell title="nextflow.config"
singularity { 
	enabled = true
	cacheDir = /path/to/singularity-img
} 
```  

OPTION2: Set the environmental variable  

The warning message indicates that the Singularity cache directory has not been set correctly, even though you specified it in your custom Nextflow configuration. To resolve this, you can explicitly set the Singularity cache directory using the `NXF_SINGULARITY_CACHEDIR` environment variable.

**Steps to Set the Singularity Cache Directory**  

1. **Set the Environment Variable**:
   - You can set the environment variable in your shell before running Nextflow. For example:
     ```bash
     export NXF_SINGULARITY_CACHEDIR=/home/pieter/singularity-img
     ```
2. **Set the Environment Variable Permanently** (Optional):
   - To make this change permanent, you can add the `export` command to your shell’s configuration file (e.g., `~/.bashrc` or `~/.bash_profile` for bash):
     ```bash
     echo "export NXF_SINGULARITY_CACHEDIR=/home/pieter/singularity-img" >> ~/.bashrc
     ```
   - Reload your shell configuration:
     ```bash
     source ~/.bashrc
     ```

3. **Verify the Configuration**:
   - Run the pipeline again to ensure the warning disappears:
     ```bash
     nextflow run epi2me-labs/wf-amplicon --fastq ./fastq --sample_sheet ./sample-sheet.csv --porechop --discard_middle --out_dir output-wf-amlicon-toydata-bis
     ```

This should direct Nextflow to use the specified cache directory for Singularity images, avoiding the warning and ensuring images are stored in your preferred location.

## wf-amplicon  
This workflow performs the analysis of reads generated from PCR amplicons. After some pre-processing, reads are either aligned to a reference (containing the expected sequence for each amplicon) for variant calling or the amplicon’s consensus sequence is generated *de novo*.  

Installation guides for wf-amplicon can be found here: [wf-amplicon Installation guide](https://labs.epi2me.io/workflows/wf-amplicon/).  

### Run the workflow  
```shell title="data layout"
(i)                     (ii)                 (iii)
input_reads.fastq   ─── input_directory  ─── input_directory
                        ├── reads0.fastq     ├── barcode01
                        └── reads1.fastq     │   ├── reads0.fastq
                                             │   └── reads1.fastq
                                             ├── barcode02
                                             │   ├── reads0.fastq
                                             │   ├── reads1.fastq
                                             │   └── reads2.fastq
                                             └── barcode03
                                              └── reads0.fastq
```
```shell title="Command to run"
nextflow run epi2me-labs/wf-amplicon \
    --fastq ./fastq \
    --reference ./reference.fa \
    --sample_sheet ./sample-sheet.csv
    -c /home/sequencing/nextflow-config/nextflow.config \ #this can be omitted if you set the variable in your .bashrc
    -profile singulairy
```
```shell title="Example: sample-sheet.csv"
barcode,alias,type,ref
barcode41,BC41,test_sample,ref1
barcode42,BC42,test_sample
barcode43,BC43,test_sample,ref2
```
```shell title="Example: reference.fa"
>ref1
ATGC
>ref2
CGTA
```
!!! NOTES
	When not running variant mode a reference file is not necessary. Also make sure to delete the ref column in that case.

## Links   
- [Installation EPI2ME](https://labs.epi2me.io/installation/#installation-on-linux)
- [Installation Nextflow](https://www.nextflow.io/docs/latest/install.html)  
- [Installation Singularity](https://docs.sylabs.io/guides/3.0/user-guide/installation.html)  
- [Installation Docker](https://docs.docker.com/engine/install/ubuntu/)
- [Installation GO](https://go.dev/doc/install)