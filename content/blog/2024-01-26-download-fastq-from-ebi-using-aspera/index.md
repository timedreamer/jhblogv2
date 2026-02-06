---
title: "Download fastq from EBI using Aspera"
author: "Ji Huang"
date: "2024-01-26"
categories: dry lab
tags:
- bioinformatics
- linux
meta_img: images/image.png
description: Description for the page
---


This post shows you how to download the fastq files from EBI, if they are available. The Aspera speeds up the downloading process by a LOT!


I use the Aspera on NYU HPC, but it should work on your own device too.


1. Get the openssh key path. I found it at `/share/apps/aspera/4.1.3.93/etc/asperaweb_id_dsa.openssh`

```shell
module show aspera/4.1.3.93
cd /share/apps/aspera/4.1.3.93/etc
```

2. Get the downloading link. Go to **https://sra-explorer.info/#**, paste the library info. Modify the **ascp openssh key path**.

3. Copy the generated script and save it on HPC. Remember to add `module load aspera/4.1.3.93` to the script.

4. Run the script from the *[Data Transfer Node](https://sites.google.com/nyu.edu/nyu-hpc/hpc-systems/hpc-storage/data-management/data-transfers)*. `ssh gdtn` 