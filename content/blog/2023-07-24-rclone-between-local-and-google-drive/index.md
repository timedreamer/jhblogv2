---
title: rclone between local and Google Drive
author: Ji Huang
date: '2023-07-24'
slug: []
categories:
  - dry lab
tags:
  - linux
  - google
  - tools
meta_img: images/image.png
description: Description for the page
---


It's very easy to use `rclone` to transfer files between linux local and cloud services, e.g. Google Drive. NYU has a help page on setting `rclone` on Greene, [Transferring Cloud Storage Data with rclone](https://sites.google.com/nyu.edu/nyu-hpc/hpc-systems/hpc-storage/data-management/data-transfers/transferring-cloud-storage-data-with-rclone).

This post is a quick go-to guide on transfering files, assuming you alrady set up to correct link between local and Google Drive.

I set up the Google Drive as `jhGDrive`. You can check the name using `rclone config`.

1. `tar -cf test.tar test_files/`. This is to tar multiple files or directories into one `.tar` file for quicker transfer. The `-c` option tells `tar` to create a new archive and the `-f` option specifies the name of the archive file. You can also add `-z` if you want to use the gzip to compress.

2. `ssh gdtn`. Connect to data transfer node from Greene. [detail](https://sites.google.com/nyu.edu/nyu-hpc/hpc-systems/hpc-storage/data-management/data-transfers)

3. `module load rclone/1.60.1`. 

4. `rclone copy test.tar jhGDrive:hpc_uploads`. The `jhGDrive` is the Google drive link name that you set up. The `hpc_uploads` is the folder name. 

5. `rclone copy jhGDrive:hpc_uploads/test.tar .`. This is the transfer cloud file to local.

---

Extra:

To find out what folders will be purged from `$SCRATCH`, use the following command. This will returns the folders up to **depth 3**. Just change the `-f1-5` to get to the 4th depth.

`cut -d'/' -f1-4 /scratch/cleanup/60days-files/20240229/jh6577 | sort | uniq`

