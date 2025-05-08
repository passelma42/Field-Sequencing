# Essential Linux Commands

## File and Directory Operations

### Listing and Navigating
- **`ls`**: List the contents of a directory.
  ```shell
  ls /path/to/directory
  ```
- **`cd`**: Change the current directory.
  ```shell
  cd /path/to/directory
  ```
- **`pwd`**: Display the current working directory.
  ```shell
  pwd
  ```

### Creating and Removing
- **`mkdir`**: Create a new directory.
  ```shell
  mkdir new_directory
  ```
- **`rmdir`**: Remove an empty directory.
  ```shell
  rmdir empty_directory
  ```
- **`rm`**: Remove files or directories.
  ```shell
  rm file_name   # for files
  rm -r directory_name   # for directories
  ```

### Copying and Moving
- **`cp`**: Copy files or directories.
  ```shell
  cp /path/to/source/file /path/to/destination/
  ```
  Example:
  ```shell
  cp /home/user/file.txt /home/user/Documents/
  ```
  - Copy a directory and its contents:
  ```shell
  cp -r /path/to/source/directory /path/to/destination/
  ```
  Example:
  ```shell
  cp -r /home/user/folder /home/user/Documents/
  ```
- **`mv`**: Move or rename files or directories.
  ```shell
  mv /path/to/source/file /path/to/destination/
  ```
  Example:
  ```shell
  mv /home/user/file.txt /home/user/Documents/
  ```

### File Manipulation
- **`touch`**: Create an empty file or update the timestamp of an existing file.
  ```shell
  touch file_name
  ```
- **`cat`**: Concatenate and display the content of files.
  ```shell
  cat file_name
  ```

### Advanced Copying with `rsync`
- **`rsync`**: Synchronize directories and copy large amounts of data efficiently.
  ```shell
  rsync -av /path/to/source/ /path/to/destination/
  ```
  - `-a`: Archive mode, preserves permissions, times, symbolic links, etc.
  - `-v`: Verbose, shows the progress of the transfer.

## File Permissions and Ownership
- **`chmod`**: Change the permissions of a file or directory.
  ```shell
  chmod 755 file_name
  ```
- **`chown`**: Change the ownership of a file or directory.
  ```shell
  chown user:group file_name
  ```

## File Viewing and Editing
- **`nano`/`vim`**: Text editors for creating and editing files directly in the terminal.
  ```shell
  nano file_name
  vim file_name
  ```
- **`less`**: Display the content of a file one screen at a time.
  ```shell
  less file_name
  ```
- **`head`**: Show the first few lines of a file.
  ```shell
  head -n 10 file_name   # shows the first 10 lines
  ```
- **`tail`**: Show the last few lines of a file.
  ```shell
  tail -n 10 file_name   # shows the last 10 lines
  ```
- **`grep`**: Search for a specific pattern within files.
  ```shell
  grep "pattern" file_name
  ```

## System Information
- **`uname`**: Display basic information about the system.
  ```shell
  uname -a   # shows all system information
  ```
- **`df`**: Show disk space usage.
  ```shell
  df -h   # human-readable format
  ```
- **`du`**: Display the disk usage of files and directories.
  ```shell
  du -sh directory_name
  ```
- **`free`**: Show the amount of free and used memory in the system.
  ```shell
  free -h
  ```
- **`top`**: Display real-time system processes and resource usage.
  ```shell
  top
  ```
- **`ps`**: Display the currently running processes.
  ```shell
  ps aux
  ```

## Network Operations
- **`ping`**: Test connectivity to a network host.
  ```shell
  ping google.com
  ```
- **`ifconfig`**: Display or configure network interfaces.
  ```shell
  ifconfig   # use `ip addr` in newer systems
  ```
- **`netstat`**: Show network connections, routing tables, and more.
  ```shell
  netstat -tuln   # shows listening ports
  ```
- **`ssh`**: Connect to another machine securely over the network.
  ```shell
  ssh user@hostname
  ```

## Process Management
- **`kill`**: Send a signal to terminate a process.
  ```shell
  kill PID   # where PID is the process ID
  ```
- **`killall`**: Terminate all processes by name.
  ```shell
  killall process_name
  ```
- **`bg`**: Resume a suspended job in the background.
  ```shell
  bg job_number
  ```
- **`fg`**: Bring a job to the foreground.
  ```shell
  fg job_number
  ```

## User and Group Management
- **`adduser`/`useradd`**: Add a new user to the system.
  ```shell
  adduser username
  useradd username
  ```
- **`passwd`**: Change a user’s password.
  ```shell
  passwd username
  ```
- **`su`**: Switch to another user account.
  ```shell
  su - username
  ```
- **`sudo`**: Execute a command with superuser privileges.
  ```shell
  sudo command
  ```

## Package Management (Example: Debian-based systems)
- **`apt-get update`**: Update the package lists.
  ```shell
  sudo apt-get update
  ```
- **`apt-get install`**: Install a new package.
  ```shell
  sudo apt-get install package_name
  ```
- **`apt-get upgrade`**: Upgrade all installed packages.
  ```shell
  sudo apt-get upgrade
  ```

## Text Manipulation
- **`echo`**: Display a line of text.
  ```shell
  echo "Hello, World!"
  ```
- **`grep`**: Search for text patterns in files.
  ```shell
  grep "text" file_name
  ```
- **`sed`**: Stream editor for filtering and transforming text.
  ```shell
  sed 's/old/new/' file_name
  ```
- **`awk`**: A powerful text processing language.
  ```shell
  awk '{print $1}' file_name
  ```

## Expanded Section on Text Manipulation for FASTA, FASTQ, and BAM Files  
When working with bioinformatics data formats like FASTA, FASTQ, and BAM files, text manipulation tools like `echo`, `grep`, `sed`, and `awk` become essential. Below are examples and explanations of how these tools can be used effectively.

### FASTA Files

**FASTA** files store nucleotide or protein sequences with a header line starting with `>` followed by sequence data. 

**Examples:**  

- **`grep`**: Extract sequence identifiers (headers) from a FASTA file.
  ```shell
  grep "^>" sequences.fasta
  ```
  This command searches for lines starting with `>` and outputs them, giving you all the headers in the file.

- **`awk`**: Count the number of sequences in a FASTA file.
  ```shell
  awk '/^>/ {count++} END {print count}' sequences.fasta
  ```
  This `awk` script increments a counter for each header line and prints the total number of sequences at the end.

- **`sed`**: Remove description from headers in a FASTA file.
  ```shell
  sed 's/\s.*$//' sequences.fasta > cleaned_sequences.fasta
  ```
  This command keeps only the identifier in each header line by deleting everything after the first space.

### FASTQ Files

**FASTQ** files contain sequence data with quality scores, commonly used in next-generation sequencing. 

**Examples:**  

- **`grep`**: Extract all sequence headers (lines starting with `@`).
  ```shell
  grep "^@" sequences.fastq
  ```
  This command will output all sequence headers in a FASTQ file.

- **`awk`**: Calculate the total number of reads in a FASTQ file.
  ```shell
  awk '{s++} END {print s/4}' sequences.fastq
  ```
  Since each read spans four lines (header, sequence, plus, quality), dividing the total line count by four gives the number of reads.

- **`sed`**: Convert sequence headers to uppercase (useful if headers have mixed cases).
  ```shell
  sed -e '1~4s/.*/\U&/' sequences.fastq > uppercased_headers.fastq
  ```
  This command converts the headers (every 4th line, starting from line 1) to uppercase.

### BAM Files

**BAM** files are binary versions of SAM files and store aligned sequence data. While binary, text manipulation often applies to the SAM format (BAM converted to SAM via `samtools view`).

**Examples:**  

- **`samtools` & `awk`**: Extract read names and count the total number of unique reads.
  ```shell
  samtools view -h file.bam | awk '{if($1 !~ /^@/) print $1}' | sort | uniq | wc -l
  ```
  This command extracts the read names from a BAM file, sorts them, filters out duplicates, and counts the unique entries.

- **`grep`**: Filter reads mapped to a specific chromosome.
  ```shell
  samtools view file.bam | grep "^chr1" > chr1_reads.sam
  ```
  This filters out reads that are mapped to chromosome 1.

- **`awk`**: Extract and count reads with a specific mapping quality.
  ```shell
  samtools view file.bam | awk '$5 >= 30' | wc -l
  ```
  This command counts the number of reads with a mapping quality of 30 or higher.

### Combining Tools for Advanced Manipulation

Combining `grep`, `awk`, and `sed` can provide powerful data extraction and manipulation. Here’s an example:

- **Extracting and summarizing GC content from FASTA sequences**:
  ```shell
  awk '/^>/ {if (seqlen){print gc/seqlen*100}; print; gc=0; seqlen=0; next} {gc+=gsub(/[GgCc]/,""); seqlen+=length($0)} END {print gc/seqlen*100}' sequences.fasta
  ```
  This script calculates the GC content for each sequence and prints it after each sequence header.

By mastering these commands, you can efficiently manipulate and analyze bioinformatics data files. Each tool offers specific advantages depending on the task, allowing for streamlined data processing in a command-line environment.

## Archiving and Compression

### **`tar`**  
Archive files into a tarball, which can be compressed using various algorithms.
  - **Create a tarball**: 
    ```shell
    tar -cvf archive_name.tar directory_name
    ```
  - **Create a compressed tarball with gzip**: 
    ```shell
    tar -czvf archive_name.tar.gz directory_name
    ```
  - **Create a compressed tarball with bzip2**: 
    ```shell
    tar -cjvf archive_name.tar.bz2 directory_name
    ```
  - **Create a compressed tarball with xz**: 
    ```shell
    tar -cJvf archive_name.tar.xz directory_name
    ```
  - **Extract a tarball**: 
    ```shell
    tar -xvf archive_name.tar
    ```
  - **Extract a gzip-compressed tarball**: 
    ```shell
    tar -xzvf archive_name.tar.gz
    ```
  - **Extract a bzip2-compressed tarball**: 
    ```shell
    tar -xjvf archive_name.tar.bz2
    ```
  - **Extract an xz-compressed tarball**: 
    ```shell
    tar -xJvf archive_name.tar.xz
    ```

### **`gzip`**  
Compress files using the gzip algorithm.
  - **Compress a file**: 
    ```shell
    gzip file_name
    ```
  - **Decompress a file**: 
    ```shell
    gunzip file_name.gz
    ```
  - **Compress a file while keeping the original**: 
    ```shell
    gzip -c file_name > file_name.gz
    ```
  - **List the contents of a gzip-compressed file**: 
    ```shell
    zcat file_name.gz
    ```

### **`bzip2`**  
Compress files using the bzip2 algorithm, which typically achieves better compression than gzip.
  - **Compress a file**: 
    ```shell
    bzip2 file_name
    ```
  - **Decompress a file**: 
    ```shell
    bunzip2 file_name.bz2
    ```
  - **Compress a file while keeping the original**: 
    ```shell
    bzip2 -c file_name > file_name.bz2
    ```
  - **List the contents of a bzip2-compressed file**: 
    ```shell
    bzcat file_name.bz2
    ```
		
### **`zip`**  
Create a ZIP archive, which can include multiple files and directories.
  - **Create a ZIP archive**: 
    ```shell
    zip archive_name.zip file1 file2 directory_name
    ```
  - **Add files to an existing ZIP archive**: 
    ```shell
    zip archive_name.zip newfile
    ```
  - **Extract a ZIP archive**: 
    ```shell
    unzip archive_name.zip
    ```
  - **List the contents of a ZIP archive**: 
    ```shell
    unzip -l archive_name.zip
    ```

### **`7z`**  
Use the 7-Zip compression tool for high compression ratios and support for many formats.
  - **Create a 7z archive**: 
    ```shell
    7z a archive_name.7z file1 file2 directory_name
    ```
  - **Extract a 7z archive**: 
    ```shell
    7z x archive_name.7z
    ```
  - **List the contents of a 7z archive**: 
    ```shell
    7z l archive_name.7z
    ```

These tools and commands cover a broad range of archiving and compression needs, from simple file compression to handling more complex multi-file archives.

## Finding Files
- **`find`**: Search for files in a directory hierarchy.
  ```shell
  find /path -name "file_name"
  ```
- **`locate`**: Quickly find files by name using an indexed database.
  ```shell
  locate file_name
  ```

## System Monitoring and Performance
- **`htop`**: Interactive process viewer (more user-friendly than `top`).
  ```shell
  htop
  ```
- **`vmstat`**: Report virtual memory statistics.
  ```shell
  vmstat 1   # refreshes every second
  ```
- **`iostat`**: Report CPU and I/O statistics.
  ```shell
  iostat
  ```
## Navigation folders
Here's a beginner-friendly explanation of the `.` and `..` notations in Linux:

---

**Understanding `.` and `..` in Linux**

When you're navigating files and folders (directories) in Linux, you’ll often see `.` and `..`. These are **special directory notations** that help you move around more easily.

**`.` — The Current Directory**

* The single dot `.` refers to the **current directory**.
* It’s like saying “right here.”

**Example:**

```bash
ls .
```

This lists the contents of the current directory — the same as just running `ls`.

**`..` — The Parent Directory**

* The double dot `..` refers to the **parent directory** — the folder **one level up**.
* It’s like saying “go up one folder.”

**Example:**

```bash
cd ..
```

This moves you **up one level** in the folder structure.

---

**Visual Example:**

Imagine your folder structure looks like this:

```
/home/pieter/documents/reports/
```

You're currently in:

```
/home/pieter/documents/reports/
```

* `.` refers to: `/home/pieter/documents/reports/`
* `..` refers to: `/home/pieter/documents/`

So:

```bash
cd ..
```

will move you to:

```
/home/pieter/documents/
```

And:

```bash
cd .
```

keeps you in the same folder.

---

