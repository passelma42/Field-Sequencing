# Linux Command Reference Guide  

A practical overview of essential Linux commands for file management, system administration, text processing, bioinformatics workflows, and data compression.

---

## Chapter1: File and Directory Operations  

### Listing and Navigating Directories

#### `ls`
Lists the contents of a directory.

```bash
ls /path/to/directory
```

#### `cd`
Changes the current working directory.

```bash
cd /path/to/directory
```

#### `pwd`
Displays the current working directory.

```bash
pwd
```

---

### Creating and Removing Files and Directories

#### `mkdir`
Creates a new directory.

```bash
mkdir new_directory
```

#### `rmdir`
Removes an empty directory.

```bash
rmdir empty_directory
```

#### `rm`
Removes files or directories.

Remove a file:

```bash
rm file_name
```

Remove a directory recursively:

```bash
rm -r directory_name
```

> ⚠️ **Warning**
>
> The `rm` command permanently deletes files. Use it carefully, especially with the `-r` option.

---

### Copying and Moving Files

#### `cp`
Copies files or directories.

Copy a file:

```bash
cp /path/to/source/file /path/to/destination/
```

Example:

```bash
cp /home/user/file.txt /home/user/Documents/
```

Copy a directory and all its contents:

```bash
cp -r /path/to/source/directory /path/to/destination/
```

Example:

```bash
cp -r /home/user/folder /home/user/Documents/
```

#### `mv`
Moves or renames files and directories.

```bash
mv /path/to/source/file /path/to/destination/
```

Example:

```bash
mv /home/user/file.txt /home/user/Documents/
```

---

### File Manipulation

#### `touch`
Creates an empty file or updates the timestamp of an existing file.

```bash
touch file_name
```

#### `cat`
Displays the contents of a file.

```bash
cat file_name
```

---

### Advanced File Synchronization with `rsync`

#### `rsync`
Efficiently synchronizes directories and transfers large amounts of data.

```bash
rsync -av /path/to/source/ /path/to/destination/
```

#### Common Options

| Option | Description |
|----------|-------------|
| `-a` | Archive mode (preserves permissions, timestamps, symlinks, etc.) |
| `-v` | Verbose mode (shows transfer progress) |

> 💡 **Tip**
>
> `rsync` is often preferred over `cp` for backups and large data transfers.

---

## Chapter 2: File Permissions and Ownership

### `chmod`
Changes file or directory permissions.

```bash
chmod 755 file_name
```

### `chown`
Changes file or directory ownership.

```bash
chown user:group file_name
```

---

## Chapter 3: File Viewing and Editing

### Text Editors

#### `nano`

```bash
nano file_name
```

### `vim`

```bash
vim file_name
```

---

### Viewing File Contents

#### `less`

View a file one screen at a time.

```bash
less file_name
```

#### `head`

Show the first 10 lines.

```bash
head -n 10 file_name
```

#### `tail`

Show the last 10 lines.

```bash
tail -n 10 file_name
```

#### `grep`

Search for text patterns.

```bash
grep "pattern" file_name
```

---

## Chapter 4: System Information

### `uname`

Display system information.

```bash
uname -a
```

### `df`

Display disk space usage.

```bash
df -h
```

### `du`

Display directory disk usage.

```bash
du -sh directory_name
```

### `free`

Display memory usage.

```bash
free -h
```

### `top`

Monitor running processes and system resources.

```bash
top
```

### `ps`

Display running processes.

```bash
ps aux
```

---

## Chapter 5: Network Operations

### `ping`

Test network connectivity.

```bash
ping google.com
```

### `ifconfig`

Display or configure network interfaces.

```bash
ifconfig
```

> 💡 Modern Linux distributions often use:

```bash
ip addr
```

### `netstat`

Display network connections and listening ports.

```bash
netstat -tuln
```

### `ssh`

Connect securely to a remote system.

```bash
ssh user@hostname
```

---

## Chapter 6: Process Management

### `kill`

Terminate a process using its PID.

```bash
kill PID
```

### `killall`

Terminate all processes with a given name.

```bash
killall process_name
```

### `bg`

Resume a stopped job in the background.

```bash
bg job_number
```

### `fg`

Bring a background process to the foreground.

```bash
fg job_number
```

---

## Chapter 7: User and Group Management

### Add Users

#### `adduser`

```bash
adduser username
```

#### `useradd`

```bash
useradd username
```

### Change Password

#### `passwd`

```bash
passwd username
```

### Switch Users

#### `su`

```bash
su - username
```

### Execute as Administrator

#### `sudo`

```bash
sudo command
```

---

## Chapter 8: Package Management (Debian-Based Systems)

### Update Package Lists

```bash
sudo apt-get update
```

### Install Packages

```bash
sudo apt-get install package_name
```

### Upgrade Installed Packages

```bash
sudo apt-get upgrade
```

---

## Chapter 9: Text Manipulation

Linux provides several powerful tools for processing and manipulating text directly from the command line.

### `echo`

Display text.

```bash
echo "Hello, World!"
```

### `grep`

Search for patterns.

```bash
grep "text" file_name
```

### `sed`

Stream editor for text transformations.

```bash
sed 's/old/new/' file_name
```

### `awk`

Advanced text processing.

```bash
awk '{print $1}' file_name
```

---

## Chapter 10: Bioinformatics Applications: FASTA, FASTQ, and BAM Files

Bioinformatics datasets are often manipulated using standard Linux text-processing tools.

---

### FASTA Files

FASTA files contain nucleotide or protein sequences. Header lines begin with `>`.

#### Extract Sequence Headers

```bash
grep "^>" sequences.fasta
```

#### Count Sequences

```bash
awk '/^>/ {count++} END {print count}' sequences.fasta
```

### Remove Header Descriptions

```bash
sed 's/\s.*$//' sequences.fasta >*cleaned_sequences.fasta
```

*--

### FASTQ Files

FASTQ*files store*sequencing reads*and quality scores.

#### Extract H*aders

```bash
grep "^@" sequences*fastq
```

#### Count Reads

```bash*awk '{s*+} END {print s/4}' sequences.fastq
```

Each FASTQ record consists of four lines:

1. Header
2. Sequence
3. Separator (`@`)
4. Quality string

#### Convert Headers to Uppercase

```bash
sed -e '1~4s/.*/\U&/' sequences.fastq > uppercased_headers.fastq
```

*--

### BAM Files

BAM files are bi*ary versions of SAM files and are *ommonly processed with `samtools`.  

#### Count Unique Reads

```bash
samtools view -h file.bam | awk '{if($1 !~ /^@/) print $1}' | sort | uniq | wc -l
```

#### Extract Chromosome-Specific Reads

```bash
samtools view file.bam | grep "^chr1" > chr1_reads.sam
```

#### Count Rea*s with Mapping Quality ≥ 30

```bash
samtools view file.bam | awk '$5 > 30' | wc -l
```

---

### Example: Calculate GC Content in FASTA Files
```shell
awk '/^>/ {if (seqlen){print gc/seqlen*100}; print; gc=0; seqlen=0; next} {gc+=gsub(/[GgCc]/,""); seqlen+=length($0)} END {print gc/seqlen*100}' sequences.fasta
```  

---

## Chapter 11: Archiving and Compression*
### TAR Archives

### Create a Tarball

```bash
tar -cvf archive_name.tar directory_name
```

### Create*a Gzip-Compressed Archive

```bash
tar -czvf archive_name.tar.gz directory_name
```

### Create a Bzip2-Compressed Archive
```bash
tar -cjvf archive_name.tar.bz2 directory_name
```

#### Create*an XZ-Compressed Archive

```bash
tar -cJvf archive_name.tar.xz directory_name
```

#### Extract Archive

```bash
tar -xvf archive_name.ta*
tar -xzvf archive_name.tar.gz
tar -xjvf archive_name.tar.bz2
tar -xJvf archive_name.tar.xz
```

---

### GZIP

Compress:

```bash
zip file_name
```

Decompress:

```bash
gunzip file_name.gz
```

Keep original file:

```bash
gzip -c file_name > file_name.gz
```

View contents:

```bash
zcat file_name.gz
```

---

### BZIP2

Compress:

```bash
bzip2 file_name
```

Decompress:

```bash
bunzip2 file_name.bz2
```

Keep original:

```bash
bzip2 -c file_name > file_name.bz2
```

View contents:

```bash
bzct file_name.bz2
```

---

### ZIP

Create archive:

```bash
zip archive_name.zip file1 file2 directory_name
```

Add files:

```bash
zip archive_name.zip newfile
```

Extract:

```bash
unzip archive_name.zip
```

List contents:

```bash
unzip -l archive_name.zip
```

---

### 7-Zip (`7z`)

Create archive:

```bash
7z a archive_name.7z file1 file2 directory_name
```

Extract archive:

```bash
7z x archive_name.7z
```

List contents:

```bash
7z l archive_name.7z
```

---

### Finding Files

#### `find`

Search for *files* within a directory hierarchy.

```bash
find /path -name "file_name"
```

#### `locate`

Search using an indexed database.

```bash
locate file_name
```

💡 *`locate` is usually much faster than `find`, but relies on an updated index.*

---

## Chapter 12: System Monitoring and Performance

### `htop`

Interactive process viewer.

```bash
htop
```

### `vmstat`

View virtual memory statistics.

```bash
vmstat 1
```

### `iostat`

Display CPU and disk I/O statistics.

```bash
iostat
```

---

#@ Navigation Shortcuts: `.` and `..`

Understanding these two symbols makes Linux navigation much easier.

### `.` (Current Directory)

Represents the directory you are currently in.

Example:

```bash
ls .
```

Equivalent to:

```bash
ls
```

---

### `..` (Parent Directory)

Represents the directory one level above the current location.

Example:

```bash
cd ..
```

---

### Visual Example

Assume the current directory is:

```text
/home/pieter/documents/reports/
```

| Symbol | Location |
|----------|----------|
| `.` | `/home/pieter/documents/reports/` |
| `..` | `/home/pieter/documents/` |

Move up one directory:

```bash
cd ..
```

Result:

```text
/home/pieter/documents/
```

Stay in the current directory:

```bash
cd .
```

Result:

```text
/home/pieter/documents/reports/
```

---

## Chatper 13: Quick Reference Cheat Sheet

| Task | Command |
|--------|--------|
| Current directory | `pwd` |
| List files | `ls` |
| Change directory | `cd` |
| Create directory | `mkdir` |
| Remove file | `rm file` |
| Copy file | `cp source destination` |
| Move file | `mv source destination` |
| Search text | `grep` |
| Search files | `find` |
| View processes | `top` |
| SSH connection | `ssh user@host` |
| Update packages | `sudo apt-get update` |
| Create tar archive | `tar -cvf archive.tar folder` |
| Unzip archive | `unzip archive.zip` |

> ✅ *Mastering these commands provides a strong foundation for Linux system administration, scripting, data analysis, and bioinformatics workflows.*