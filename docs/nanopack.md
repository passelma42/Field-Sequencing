# Nanopack  
Nanopack is a toolset of different long read processing and analysis tools. Below you'll find elaborate exercises explaining all the different faeatures of this toolset.  
The publischer also provided a *test-data* set which can be downloaded in preparation of this tutorial.

**Tool**: [https://github.com/wdecoster/nanopack](https://github.com/wdecoster/nanopack)  
**Download test data**: [https://github.com/wdecoster/nanotest](https://github.com/wdecoster/nanotest)  
**Publication**: [NanoPack: visualizing and processing long-read sequencing data](https://doi.org/10.1093/bioinformatics/bty149) 
>Wouter De Coster, Svenn D’Hert, Darrin T Schultz, Marc Cruts, Christine Van Broeckhoven, NanoPack: visualizing and processing long-read sequencing dta, Bioinformatics, Volume 34, Issue 15, August 2018, Pages 2666–2669, https://doi.org/10.1093/bioinformatics/bty149  

## Installation Using Pyenv
Please follow the installation instructions on the [nanopack github website](https://github.com/wdecoster/nanopack?tab=readme-ov-file) for installing all the individual tools and modules.  

NanoPack is a collection of tools for quality control and visualization of Oxford Nanopore sequencing data. Commonly used utilities include:

- `NanoPlot`
- `NanoComp`
- `NanoStat`
- `NanoFilt`

Using a dedicated Python environment is recommended to avoid dependency conflicts with your operating system or other bioinformatics software.

---

#### Install Pyenv

If Pyenv is not yet installed, follow the official installation instructions:

<https://github.com/pyenv/pyenv>

Verify the installation:

```bash
pyenv --version
```

---

#### Install a Python Version

List available Python versions:

```bash
pyenv install --list
```

Install Python 3.12:

```bash
pyenv install 3.12.3
```

Verify:

```bash
pyenv versions
```

---

## Nanopack pyenv  
### Create & Activate an Environment

Create a virtual environment:

```bash
pyenv virtualenv 3.12.3 nanopack-env
```

Activate the environment:

```bash
pyenv activate nanopack-env
```

!!! note "Your terminal prompt should now look similar to"
    (nanopack-env) user@workstation:~$

!!! warning
    Make sure that the envrionment is active before to continue with the following commands.  
    This to make sure you install nanopack in the environment and not in you OS python environment.


---

### Install NanoPack

Upgrade pip first:

```bash
pip install --upgrade pip
```

Install NanoPack:

```bash
pip install NanoPack
```

Verify the installation:

```bash
NanoPlot --version
```

or:

```bash
NanoStat --help
```

---

### Deactivate the Environment

After finishing your analysis:

```bash
pyenv deactivate
```

Your shell will return to the default Python environment.

---