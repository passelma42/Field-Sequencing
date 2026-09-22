# Software installation-2026

This guide describes which additional software needs to be installed to run nanopore sequencing in the field. Including EPI2ME pipeline from Nanopore.

## 1. JAVA  

Linux systems can have multiple Java installations, including different versions of the Java Development Kit (JDK) and Java Runtime Environment (JRE). The JDK is used for developing Java applications, while the JRE is sufficient for running them.  

Full details see: [https://linuxvox.com/blog/how-to-install-java-on-linux/](https://linuxvox.com/blog/how-to-install-java-on-linux)

### Quick Guide for Java install on linux. 

Update the package list:  
```shell
sudo apt update
```  

Install the default OpenJDK (Java 25 on Ubuntu 26.04):   
```shell 
sudo apt install default-jdk
```  

To install a specific version, such as Java 21:
```shell
sudo apt install openjdk-21-jdk
```

If you only need the JRE:
```shell
sudo apt install openjdk-25-jre
``` 

---  
## 2. GPU  
### Install NVIDIA Drivers

For full details see: [https://www.nvidia.com/en-us/drivers/](https://www.nvidia.com/en-us/drivers/)
!!! info "Linux NVIDIA drivers"
        Many Linux distributions provide their own packages of the NVIDIA Linux Graphics Driver in the  
        distribution's native package management format. This may interact better with the rest of your  
        distribution's framework, and you may want to use this rather than NVIDIA's official package.
       
**Manual Linux installation**  

```bash
sudo apt update
```  
Install driver management utilities:
```bash
sudo apt install ubuntu-drivers-common
```
Check available hardware:
```bash
ubuntu-drivers devices
```
Install the recommended driver automatically:

```bash
sudo ubuntu-drivers autoinstall
```

Reboot:

```bash
sudo reboot
```

### Verify Driver Installation

Check driver status:

```bash
nvidia-smi
```

Expected output:

```text
+-----------------------------------------------------------+
| NVIDIA-SMI                                                |
| Driver Version: xxx.xx                                    |
| CUDA Version: xx.x                                        |
+-----------------------------------------------------------+
```

Confirm that the GPU is detected and CUDA is available.

### Verify CUDA Toolkit

Check CUDA version:

```bash
nvcc --version
```

Example:

```text
Cuda compilation tools, release 12.x
```

!!! warning "Secure Boot Warning"

    On some systems NVIDIA drivers fail to load when Secure Boot is enabled.

    If `nvidia-smi` reports errors after installation, verify whether Secure Boot is enabled in the BIOS/UEFI settings.

---
## 3. Minknow  

*Quick installation commands for Ubuntu 24**  
Add Oxford Nanopore apt repository:  
```shell
sudo apt update
sudo apt install wget
wget -O- https://cdn.oxfordnanoportal.com/apt/ont-repo.pub | sudo tee /etc/apt/keyrings/nanoporetech.asc > /dev/null
echo -e "Types: deb\nURIs: http://cdn.oxfordnanoportal.com/apt\nSuites: noble-stable\nComponents: non-free\nSigned-By: /etc/apt/keyrings/nanoporetech.asc" | sudo tee /etc/apt/sources.list.d/nanoporetech.sources
```  
Install MinKNOW using the command:  
```shell
sudo apt update
sudo apt install ont-standalone-minknow-release
```  

For full details see: [https://nanoporetech.com/document/experiment-companion-minknow](https://nanoporetech.com/document/experiment-companion-minknow)  

!!! warning
    One some sytems MinKNOW doesn't work out of the box.  
    For troubleshooting we refer you to the Section: System Documentation > Minknow-troubleshooting

## 4. Dorado  
Dorado is a high-performance, easy-to-use, open source analysis engine for Oxford Nanopore reads.  
It consists of a standalone binary which can be downloaded [here](https://github.com/nanoporetech/dorado/).  

Once the relevant ```.tar.gz``` or ```.zip``` archive is downloaded, extract the archive to your desired location.

You can then call Dorado using the full path, for example:

```shell
/path/to/dorado-x.y.z-linux-x64/bin/dorado basecaller hac pod5s/ > calls.bam
```


Or you can add the bin path to your $PATH environment variable, and run with the dorado command instead, for example:

```shell
dorado basecaller hac pod5s/ > calls.bam
```

For full details see:  

- [https://github.com/nanoporetech/dorado/](https://github.com/nanoporetech/dorado/)
- [https://software-docs.nanoporetech.com/dorado/latest/](https://software-docs.nanoporetech.com/dorado/latest/)

---  

## 5. Nanopack  
Nanopack is a toolset of different long read processing and analysis tools. Below you'll find elaborate exercises explaining all the different faeatures of this toolset.  
The publischer also provided a *test-data* set which can be downloaded in preparation of this tutorial.

**Tool**: [https://github.com/wdecoster/nanopack](https://github.com/wdecoster/nanopack)  
**Download test data**: [https://github.com/wdecoster/nanotest](https://github.com/wdecoster/nanotest)  
**Publication**: [NanoPack: visualizing and processing long-read sequencing data](https://doi.org/10.1093/bioinformatics/bty149) 
>Wouter De Coster, Svenn D’Hert, Darrin T Schultz, Marc Cruts, Christine Van Broeckhoven, NanoPack: visualizing and processing long-read sequencing dta, Bioinformatics, Volume 34, Issue 15, August 2018, Pages 2666–2669, https://doi.org/10.1093/bioinformatics/bty149  

Please follow the installation instructions on the [nanopack github website](https://github.com/wdecoster/nanopack?tab=readme-ov-file) for installing all the individual tools and modules.  

See the Full installation procedure on the [Nanopack](nanopack.md) page for more detailed information.  
Co-installing interesting bioinformatic tools to manipulate sequencereads using pixi, have a look here [Nanopore-Toolkit](Nanopore-Toolkit.md).  


---
## 6. Nextflow  

Nextflow enables scalable and reproducible scientific workflows using containers.

Install:

```bash
curl -s https://get.nextflow.io | bash
```
Make Nextflow executable:  
```shell
chmod +x nextflow
```
Move Nextflow into an executable path. For example:  
```shell
sudo mv nextflow /usr/local/bin/
```
Verify:

```bash
nextflow -version
```
### Nextflow Cache Configuration  

For large projects it can be useful to define dedicated storage locations for Nextflow data. Container images can become very large. This is optional but can simplify storage management for large sequencing projects.  

Nextflow downloads:  

- workflow code
- container images
- reference resources
- dependencies

Make a nextflow-cache directory:  
```bash
mkdir -p $HOME/nextflow-cache
```

Set environmtental variables for:  
```bash
export NXF_HOME=$HOME/.nextflow
export NXF_SINGULARITY_CACHEDIR=$HOME/nextflow-cache
```  

For full details see: 
  
[https://www.nextflow.io/](ttps://www.nextflow.io/)  

!!! warning
    Nextflow depends on a compatible version of Java pre installed.


## 7. EPI2ME 

EPI2ME Labs maintains a collection of actively maintained bioinformatics workflows for Oxford Nanopore sequencing data. Each workflow has it's onw installation instructions

Useful workflows include:

- [wf-amplicon](https://github.com/epi2me-labs/wf-amplicon)
- [wf-metagenomics](https://github.com/epi2me-labs/wf-metagenomics)
- [wf-bacterial-genomes](https://github.com/epi2me-labs/wf-bacterial-genomes)
- [wf-basecalling](https://github.com/epi2me-labs/wf-basecalling)
- [...](https://github.com/orgs/epi2me-labs/repositories?type=all)

For full details see:  

- [https://epi2me.nanoporetech.com/wfindex/](https://epi2me.nanoporetech.com/wfindex/)
- [https://nanoporetech.com/products/analyse/epi2me](https://nanoporetech.com/products/analyse/epi2me)

---  

## 8. Containers  

Docker on Ubuntu: <https://docs.docker.com/engine/install/ubuntu/>  
Apptainer on Ubuntu: <https://apptainer.org/docs/admin/main/installation.html>  
---

## 9. Package managers  

Conda on Linux: <https://docs.conda.io/projects/conda/en/latest/user-guide/install/>  
Pixi on Linux: <https://pixi.prefix.dev/latest/#installation>

---  

## 10. Useful Links

- [EPI2ME Labs Installation](https://labs.epi2me.io/installation/)
- [EPI2ME Workflow Index](https://labs.epi2me.io/wfindex/)
- [Nextflow Installation Guide](https://www.nextflow.io/docs/latest/install.html)
- [Dorado GitHub Repository](https://github.com/nanoporetech/dorado)
- [NVIDIA Drivers](https://www.nvidia.com/en-us/drivers/)
- [Java Linux x64 Installation](https://www.java.com/en/download/help/linux_x64_install.html)
- [MinKNOW Experiment Companion](https://nanoporetech.com/document/experiment-companion-minknow)
- [Dorado Documentation](https://software-docs.nanoporetech.com/dorado/latest/)
- [Docker](https://docs.conda.io)
- [Apptainer](https://apptainer.org/docs/)
- [Conda](https://docs.conda.io)
- [Pixi](ttps://pixi.prefix.dev/)
