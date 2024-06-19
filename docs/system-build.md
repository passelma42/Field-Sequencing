# System setup   

## Operating system  

**Release**: Ubuntu 22.04.4 LTS (Jammy Jellyfish)  
Follow this link to learn [all you need to know on how to install ubuntu](https://ubuntu.com/tutorials/install-ubuntu-desktop#1-overview).

## GPU & CUDA   

GPU computational power increased ONT basecalling speed and enabled real-time data analysis. Upon writing this documentation dorado 0.7 is the current basecaller  
which is also incorperated in the sequencing operating software Minknow. Before we get to the installation of dedicated sequencing software and other data analys packages we  
will focus on installing a GPU and the CUDA toolkit.  
### Installation on Ubuntu  

- Add graphics drivers ppa repo  
```shell
sudo add-apt-repository ppa:graphics-drivers/ppa
```
- Install Ubuntu drivers app  
```shell
sudo apt install ubuntu-drivers-common
```
- Check available GPUs  
```shell
ubuntu-drivers devices
```
```shell title='Example output'
== /sys/devices/pci0000:00/0000:00:01.0/0000:01:00.0 ==
modalias : pci:v000010DEd000027E0sv00001025sd0000166Cbc03sc00i00
vendor   : NVIDIA Corporation
driver   : nvidia-driver-545-open - distro non-free
driver   : nvidia-driver-555-open - third-party non-free
driver   : nvidia-driver-550 - third-party non-free
driver   : nvidia-driver-550-open - third-party non-free
driver   : nvidia-driver-545 - distro non-free
driver   : nvidia-driver-535 - third-party non-free
driver   : nvidia-driver-555 - third-party non-free recommended
driver   : nvidia-driver-535-server - distro non-free
driver   : nvidia-driver-535-open - distro non-free
driver   : nvidia-driver-535-server-open - distro non-free
driver   : nvidia-driver-525 - third-party non-free
driver   : xserver-xorg-video-nouveau - distro free builtin
```
- Install latest Nvidia driver (change `555` with latest version available in your case)  
```shell
sudo apt install nvidia-driver-525
```
- Reboot your computer  
- Check if Nvidia driver and CUDA is available after reboot  
```shell
nvidia-smi
```
```shell title='Example output'
+---------------------------------------------------------------------------------------+
| NVIDIA-SMI 545.29.06              Driver Version: 545.29.06    CUDA Version: 12.3     |
|-----------------------------------------+----------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |         Memory-Usage | GPU-Util  Compute M. |
|                                         |                      |               MIG M. |
|=========================================+======================+======================|
|   0  NVIDIA GeForce RTX 4080 ...    Off | 00000000:01:00.0 Off |                  N/A |
| N/A   45C    P8               1W /  90W |   1032MiB / 12282MiB |      0%      Default |
|                                         |                      |                  N/A |
+-----------------------------------------+----------------------+----------------------+
                                                                                         
+---------------------------------------------------------------------------------------+
| Processes:                                                                            |
|  GPU   GI   CI        PID   Type   Process name                            GPU Memory |
|        ID   ID                                                             Usage      |
|=======================================================================================|
+---------------------------------------------------------------------------------------+
```

This output tells us we have a NVIDIA GeForce RTX 4080 running (12GB virtual RAM) and CUDA Version 12.3 has been co-installed.  

### Purge Nvidia  

On some occasions you'll need to perform a fresh installation of nvidia drivers. It is important to purge all currently installed NVIDIA/CUDA before installing anew.  
Here is a step by step guide on how to do this:  

- List all packages  
The dpkg -l command is used in Debian-based Linux distributions to list all the installed packages.  
This command provides detailed information about each package, including its status, name, version, and a brief description.
```shell 
dpkg -l
```
The output of dpkg -l, the columns represent:  

> 1. Package status (e.g., "ii" for installed, "un" for uninstalled, etc.)  
> 2. Package name  
> 3. Version  
> 4. Architecture  
> 5. Description  

- List and purge all NVIDIA/CUDA  

```shell
sudo apt-get purge $(dpkg -l | grep '^ii.*nvidia'| awk '{print $2}')
```
``` title='Description'
dpkg -l | grep '^ii.*nvidia'	# This command lists all installed packages (output of dpkg -l) that have "nvidia" in their names and are in the "installed" state (lines starting with "ii").  
	
awk '{print $2}'				# This command uses awk to print the second column (package names) from the output of the previous command.  
	
sudo apt-get purge $(...)		# This command uses command substitution to execute the inner command and pass its output (list of package names) as arguments to apt-get purge, which removes the specified packages.  
```


.
## System Software  

On Ubuntu system applications can be installed using the graphical user interface (GUI).  
More information on how to install software on Ubuntu [click here](https://help.ubuntu.com/stable/ubuntu-help/addremove-install.html.en).  

### Bioinformatic software  

Usually bioinformatic software are opensource packages with installation instructions described on Github repositories.  
First thing you can do is get yourself a github account and [get familiarized with github](https://github.com/git-guides).  
Executable scripts, often found in a `/bin`folder, can be installed in `/usr/local/bin` or in a dedicated bioinfo folder `/usr/local/bioinf/`.  
The next example is to demonstrate how to install dorado, the ont basecaller, in the `/bioinf`folder and put the script on `PATH`.  

- Goto [DORADO GITHUB](https://github.com/nanoporetech/dorado)  
- Download the relevant installer for your platform (like described in the instructions)  
- Extract the archive to the desired location  
```shell title='Extract and put on PATH'
1. mkdir /usr/local/bioinf/
2. cd /usr/local/bioinf/
3. wget https://cdn.oxfordnanoportal.com/software/analysis/dorado-0.7.2-linux-x64.tar.gz
4. tar -xzvf dorado-0.7.2-linux-x64.tar.gz
	# after unzipping, the main folder will contain a /bin and /lib folder.
5. cd bin  
6. ls
	# /usr/local/bioinf/dorado/bin/dorado => this is the Executable file 
7. sudo ln -s /usr/local/bioinf/dorado/bin/dorado /usr/local/bin/dorado
	# this will symlink the dorado script to /usr/local/bin/dorado
8. echo 'export PATH=$PATH:/usr/local/bin' | sudo tee -a /etc/profile && export PATH=$PATH:/usr/local/bin
	# put in path if not by default, you can check by `echo $PATH`
```  
Following these instructions you should have downloaded the dorado script and it's libraries in the designated `/usr/local/bioinf` folder. To make the executable file accessible for all users  
you need to put it in PATH.

### Python environments  

When you need to install packages using the command `pip` it is adviced to do so in a separate python environment to avoid issues with the already existing python installation supporting your operating system.  
Detailed information on python venv can be found [here](https://docs.python.org/3/library/venv.html).  

### Conda  

Yet another packaging and environmanagement tool is Conda. A lot of bioinformatic software has been made available in dedicated conda environments.
Read all there is to know on [how to use conda here](https://conda.io/projects/conda/en/latest/user-guide/getting-started.html). 

