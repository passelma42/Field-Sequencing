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
```shell
curl -s https://get.nextflow.io | bash # install nextflow
``` 
### GO  
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
If folder is not set you get this warning:  
	WARN: Singularity cache directory has not been defined -- Remote image will be stored in the path: /home/sequencing/Analisys/wf-amplicon-demo/work/singularity -- Use the environment variable NXF_SINGULARITY_CACHEDIR to specify a different location

To avoid this create cache folder to store downloaded images when running epi2me labs. For each new analysis you run with epi2me make sure to put the image in this folder.  
```shell
mkdir /path/to/singularity-cache-dir
```
**Create nextflow config file**  
This file will be checked running each wf and redirects singularity to find the images stored locally rather than download them each time.  
[More info on nextflow configuration file](https://www.nextflow.io/docs/latest/config.html).  
Process:  
```shell
mkdir /home/sequencing/nextflow-config
touch /home/sequencing/nextflow-config/nextflow.config
```
```shell title="nextflow.config"
singularity { 
	enabled = true
	cacheDir = /path/to/singularity-cache-dir
} 
```  

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
    -c /home/sequencing/nextflow-config/nextflow.config \
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