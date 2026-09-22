# Introduction

Field sequencing outside a traditional laboratory environment has become possible through portable sequencing technologies developed by  [Nanoporetech](https://nanoporetech.com/). Using a compact nanopore sequencer such as the **MinION** in combination with a modern laptop equipped with sufficient CPU and GPU resources, researchers can perform DNA sequencing and bioinformatics analyses directly in the field.

Portable sequencing has opened new opportunities for biodiversity monitoring, environmental DNA (eDNA) studies, pathogen surveillance, conservation research, and rapid species identification. The ability to generate and analyse sequencing data in real time enables faster decision-making and reduces the need to transport samples to centralized laboratories.

The aim of this website is to provide a practical guide for establishing a portable field sequencing workflow.  
Topics covered include:

- Building and configuring a GPU-enabled analysis workstation
- Installing and configuring Oxford Nanopore sequencing software
- Processing and analysing nanopore sequencing data
- Using `BLAST` and reference databases for taxonomic identification
- Quality control and data visualization
- Field deployment considerations and best practices

This website provides a step-by-step guide that will help researchers, students, and practitioners develop the skills required to perform both laboratory and bioinformatics workflows for field-based nanopore sequencing.

!!! info "What has changed since early nanopore workflows?"

    Modern nanopore sequencing workflows increasingly use:

    - Dorado for basecalling
    - POD5 as the preferred raw data format
    - GPU-accelerated data processing
    - Adaptive sampling for targeted sequencing
    - Reproducible workflows using containers and workflow managers

    Throughout this guide, current best practices are highlighted where appropriate.