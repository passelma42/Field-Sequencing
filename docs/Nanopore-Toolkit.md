# Nanopore Toolkit with Pixi  

Oxford Nanopore sequencing workflows typically require many different bioinformatics tools for basecalling, quality control, assembly, taxonomic profiling, and downstream analysis. Managing these tools individually can be challenging because of dependency conflicts and version differences between systems.

[Pixi](https://pixi.sh) provides a modern and reproducible way to manage software environments. A Pixi workspace stores all dependencies in a single project and creates a lock file (`pixi.lock`) that guarantees the same software versions can be installed again on any supported system. Pixi workspaces are based on a `pixi.toml` manifest that defines channels, platforms, dependencies, tasks, and project metadata.  

This tutorial demonstrates how to:

1. Install Pixi
2. Create your first Pixi workspace
3. Build a reusable Nanopore Toolkit environment
4. Install commonly used Nanopore bioinformatics software
5. Run tools through Pixi

---

## Chapter 1: Install Pixi

Pixi can be installed on Linux, macOS, and Windows.

### 1.1 Linux and macOS

```bash
curl -fsSL https://pixi.sh/install.sh | sh
```

After installation, restart your terminal or reload your shell profile:

```bash
source ~/.bashrc
```

Verify the installation:

```bash
pixi --version
```

---

### 1.2 Windows (PowerShell)

```powershell
powershell -ExecutionPolicy ByPass -c "irm -useb https://pixi.sh/install.ps1 | iex"
```

Verify installation:

```powershell
pixi --version
```

You should see the installed Pixi version.

Check the ```.toml``` file:  
```shell
MyComputer:~/nanopore-toolkit % cat pixi.toml 
[workspace]
authors = ["MyID <Mymaol@mail.be>"]
channels = ["conda-forge"]
name = "nanopore-toolkit"
platforms = ["osx-arm64"]
version = "0.1.0"

[tasks]

[dependencies]
```
This tells me that I'm on a mac os platform, that looking for tools only searches in conda-forge channel. The latter might be a problem if your tool is not in the ```conda-forge``` channel but in ```bioconda``` like a lot of bioinformatic tools are. Let's add this channel too.

```shell
pixi project channel add bioconda
```
Output should be: ✔ Added bioconda (https://conda.anaconda.org/bioconda/) 
The ```pixi.toml``` file looks now like this:  
Check the ```.toml``` file:  
```shell
MyComputer:~/nanopore-toolkit % cat pixi.toml 
[workspace]
authors = ["MyID <Mymaol@mail.be>"]
channels = ["conda-forge", "bioconda"]
name = "nanopore-toolkit"
platforms = ["osx-arm64"]
version = "0.1.0"

[tasks]

[dependencies]
```
---

## Chapter 2: Create Your First Pixi Workspace

A Pixi workspace is simply a directory containing a `pixi.toml` manifest file. The manifest describes all software, environments, and tasks associated with the project.  

Create a new workspace:

```bash
pixi init nanopore-toolkit
```

Pixi creates a project structure similar to:

```text
nanopore-toolkit/
├── .gitattributes
├── .gitignore
└── pixi.toml
```

The `pixi.toml` file is the central configuration file for the workspace. 

Move into the workspace:

```bash
cd nanopack-toolkit
```

---

## Chapter 3: Understanding the Pixi Workflow

Pixi automatically:

- Stores dependencies in `pixi.toml`
- Resolves compatible package versions
- Creates a reproducible `pixi.lock` file
- Builds and installs the environment locally

This ensures collaborators can reproduce the exact same software environment. 

Useful commands:  

```bash
pixi add
pixi remove
pixi update
pixi install
pixi list
pixi shell
```  

---

## Chapter 4: Building the Nanopore Toolkit

The Nanopore Toolkit contains software for:

| Purpose | Tool |
|----------|----------|
| Read QC | NanoPack |
| Polishing tool | Medaka |
| Polishing tool | Racon |
| FASTQ manipulation | SeqKit |
| FASTQ manipulation | SeqTK |
| Read filtering | Fastp |
| Assembly | Flye |
| Read mapping | Minimap2 |
| BAM handling | Samtools |
| Sequence similarity search | BLAST+ |
| Assembly quality assessment | QUAST |
| Compression | Pigz |
| Flow management system | Snakemake |
| Flow management system | Nextflow |
| Parallel execution | GNU Parallel |

---

## Chapter 5: Install All Dependencies

Add all required tools to the workspace:

```bash
pixi add \
    python=3.12 \
    nanopack \
    seqkit \
    seqtk \
    fastp \
    flye \
    minimap2 \
    samtools \
    blast \
    quast \
    pigz \
    nextflow \
    snakemake \
    parallel
```

Pixi will:

1. Update `pixi.toml`
2. Resolve dependencies
3. Create `pixi.lock`
4. Install the environment

This may take several minutes the first time.

---

## Chapter 6: Inspect Installed Packages

Show all installed software:

```bash
pixi list
```  

---

## Chapter 7: Activate the Environment

Start a shell within the Pixi environment:

```bash
pixi shell
```

Once activated, all installed software is available directly from the command line. Pixi automatically configures the required environment variables and paths. 

Check a few tools:

```bash
flye --version

minimap2 --version

samtools --version
```

Exit the environment:

```bash
exit
```

---

## Chapter 8:  Running Commands Without Activating the Environment

Instead of activating a shell, commands can be executed directly with `pixi run`.

Examples:

```bash
pixi run seqkit version
```

```bash
pixi run flye --version
```

```bash
pixi run samtools --version
```

---

## Chapter 9: Typical Nanopore Workflow

### 9.1 Basecalling

```bash
pixi run dorado model_directory pod5s/ > reads.fastq
```

---

### 9.2 Read Statistics

```bash
pixi run seqkit stats reads.fastq
```

---

### 9.3 Read Quality Control

Generate NanoPlot reports:

```bash
pixi run NanoPlot \
    --fastq reads.fastq \
    --outdir qc
```

---

### 9.4 FASTQ Subsampling

```bash
pixi run seqtk sample reads.fastq 10000 > subset.fastq
```

---

### 9.5 Read Filtering

```bash
pixi run fastp \
    -i reads.fastq \
    -o filtered.fastq
```  

---

### 9.6 Genome Assembly

```bash
pixi run flye \
    --nano-hq reads.fastq \
    --genome-size 5m \
    --threads 16 \
    --out-dir assembly
```  

---

### 9.7 Read Mapping  

```bash
pixi run minimap2 \
    -ax map-ont \
    assembly/assembly.fasta \
    reads.fastq > alignment.sam
```

Convert to BAM:  

```bash
pixi run samtools view \
    -bS alignment.sam > alignment.bam
```

Sort and index:  

```bash
pixi run samtools sort \
    alignment.bam \
    -o alignment.sorted.bam

pixi run samtools index alignment.sorted.bam
```  

---

### 9.8 BLAST Search

```bash
pixi run blastn \
    -query assembly/assembly.fasta \
    -db nt \
    -out blast_results.txt
```

---

### 9.9 Assembly Polishing

```bash
pixi run medaka_consensus \
    -i reads.fastq \
    -d assembly/assembly.fasta \
    -o medaka_results
```

---

### 9.10 Assembly Quality Assessment

```bash
pixi run quast assembly/assembly.fasta
```

---

## Chapter 10: Updating the Toolkit

Update all packages:

```bash
pixi update
```

Upgrade packages to newer major versions if available:

```bash
pixi upgrade
```
Be carefull with this not to corrupt dependencies relying on specific versions.
---

## Chapter 11: Sharing the Toolkit  

Typical structure:  
```text
nanopore-toolkit/
├── pixi.toml
├── workflows-snakemake/
│   ├── species_id.smk
│   ├── assembly.smk
│   ├── polishing.smk
├── workflows-snextflow/
│   ├── main.nf # Discribes the workflow
│   ├── modules/
│       ├── flye.nf # How to execute the command in the workflow
│       └── medaka.nf
├── scripts/
│   ├── prepare_reads.py
│   ├── collect_metrics.py
│   └── make_consensus_report.py
├── config/
│   └── config.yaml
├── test_data/
└── results/
```  

The key files that should be shared with collaborators are:

```text
pixi.toml
pixi.lock
```

Add the files to a github repo:  
```shell
git add pixi.toml pixi.lock
git commit -m "Add Pixi environment"
``` 

A colleague can recreate the exact environment with:

```bash
git clone <repository>
cd nanopore-toolkit
pixi install
```

Because the dependency versions are stored in `pixi.lock`, the resulting environment will be reproducible across systems. 

---
## Chapter 12: Download the nanopack-toolkit  

Clone the Repository and Install the Pixi Environment

Using SSH:

```bash
git clone git@github.com:passelma42/nanopore-toolkit.git
cd nanopore-toolkit
```

Alternatively, using HTTPS:

```bash
git clone https://github.com/passelma42/nanopore-toolkit.git
cd nanopore-toolkit
```
Go to the folder ./nanopack-tookit and issue the install command.  Pixi resolves all dependencies defined in `pixi.toml` and creates the project environment.

```bash
cd /path/to/nanopack-toolkit
pixi install
```

Activate the Environment (still working from within the project directory).  
Start a Pixi shell:

```bash
pixi shell
```

You are now working inside the project environment with all dependencies available.


## Chapter 13: Useful Daily Commands  

```bash
pixi shell # Activate the environment
pixi list # List installed packages
pixi update #Update dependencies
pixi install # Recreate the environment from the lock file
pixi remove <package> # Remove a package
pixi info # Display workspace information
```  

---

## Chapter 14: Co-Installation of NGSpeciesID with Pixi (macOS)  
[NGSpeciesID](https://github.com/ksahlin/NGSpeciesID) is a tool for clustering and consensus forming of long-read amplicon sequencing data (has been used with both PacBio and Oxford Nanopore data). The repository is a modified version of isONclust, where consensus, primer-removal, and polishing feautures have been added.
This tool can be a nice addition to your toolpack if you are focused on amplicon sequencing for instance and building consensus sequences.  

NGSpeciesID now recommends installing its dependencies through Conda/Bioconda and then installing NGSpeciesID with `pip --no-deps`. This workflow can be reproduced with Pixi because Pixi uses the Conda ecosystem. 

***Recommended minimal Pixi Setup***

```bash
pixi init

pixi add -c conda-forge -c bioconda \
    python=3.12 \
    medaka \
    spoa \
    racon \
    minimap2 \
    samtools \
    pip

pixi shell

pip install --no-deps NGSpeciesID
```
The key part is that ```---no-deps``` avoids pip trying to build/reinstall parasail and edlib, a common source of installation problems on Apple Silicon Macs.  

!!! Note
    If you already built the nanopore toolkit you only need to add the SPOA to the project environment before running the pip install command to install NGSpeciesID with no dependencies.

---

## Conclusion

You now have a reproducible Nanopore bioinformatics toolkit managed with Pixi. The workspace contains tools for:

- Basecalling (Dorado)
- Read QC (NanoPack)
- FASTQ processing (SeqKit, SeqTK, Fastp)
- Assembly (Flye)
- Read mapping (Minimap2, Samtools)
- Similarity searching (BLAST+)
- Polishing (Medaka, Racon)
- Assembly QC (QUAST)
- Clustering and consensus tool (NGspeciesID)

The entire [toolkit can be installed](https://github.com/passelma42/nanopore-toolkit.git), updated, shared, and reproduced using only `pixi.toml` and `pixi.lock`.  
Adding tools to your liking is also straightforward by using the command ```pixi add <toolname>```. This enabels you to tailor your project environment to your needs.   