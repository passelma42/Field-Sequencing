# Nanopore Tools  
All nanopore proprietary software can be found in [the software & Download section](https://community.nanoporetech.com/downloads?from=support) on the website of nanopore.   

## Sequencing: Minkow  
The basic setup for sequencing in the software package called `Minknow`. Minknow can both operate the sequencing device as well as running real time basecalling.  
The installation guide provided in the [the software & Download section](https://community.nanoporetech.com/downloads?from=support) gives you an overview of all necessary commands to issue for installing the software.  
### Default installation directories  

1. For the MinKNOW software:  
`/opt/ont/minknow`  
2. For the MinKNOW user interface:  
`/opt/ont/minknow-ui`  
3. Location of the reads folder:  
`/var/lib/minknow/data`  
4. Location of the log files:  
The MinKNOW logs are located in `/var/log/minknow`  
The Dorado basecaller logs are located in `/var/log/dorado`  
### Run Minknow as root  
When running minknow you'll notice that when issuing another path for your reads folder, minkow will issue an error. Redirecting output from the standard `/var/lib/minknow/data`  
might be necessary when you run out of diskspace or simply when you prefer to output your data elsewhere.  
To enable this, you'll need to give the `minknow.service` writing privileges i.e. make this root.  
```shell title="Make minknow.service root"
		$ sudo service minknow stop
		$ sudo perl -i -pe 's/(User|Group)=minknow/$1=root/'/lib/systemd/system/minknow.service
		$ sudo systemctl daemon-reload
		$ sudo service minknow start
```

## Basecalling: Dorado  
Dorado is a high-performance, easy-to-use, open source basecaller for Oxford Nanopore reads. The tool can be installed as a stand alone tool which provides more functionalities than basecalling in Minknow.  
You can find the [Dorado github repo here](https://github.com/nanoporetech/dorado).  
### Dorado features  

1. **Download model** modules are not packed in the software  
```shell
dordado download --list
```   
2. Basecaller  
```shell
dorado basecaller dna_r10.4.1_e82_400bps_fast@v4.1.0 ./input_pod5/ > output.bam
dorado basecaller dna_r10.4.1_e82_400bps_fast@v4.1.0 ./input_pod5/ --emit-fastq > output.fastq
dorado basecaller dna_r10.4.1_e82_400bps_fast@v4.1.0 ./input_pod5/ --emit-sam > output.sam
```
3. Aligner  
```shell
dorado basecaller -x cuda:0 --reference .your-reference.fasta dna_r10.4.1_e8_400bps_fast@v4.1.0 ./pod5 > aligned.bam
```    
4. Sequencing summary generation  
```shell
dorado summary ./reads.bam > sequencing_summary.txt
```  
