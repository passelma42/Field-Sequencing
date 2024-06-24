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
## Workflows  
EPI2ME Labs maintains a collection of bioinformatics workflows tailored to Oxford Nanopore Technologies long-read sequencing data. They are curated and actively maintained by experts in long-read sequence analysis.  
Read all about [EPI2ME workflows here](https://labs.epi2me.io/wfindex/).