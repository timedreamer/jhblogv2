---
title: Query GEO for number of uploaded datasets each year
author: Ji Huang
date: '2024-05-27'
slug: []
categories:
  - dry lab
tags:
  - bioinformatics
  - misc
  - visualization
meta_img: images/image.png
description: Description for the page
---


I wrote a R script to query the NCBI-GEO datasets to see whether microarrays are still been used for several species. I also query the sequencing datasets.

This is not a perfect search as I only queried the GEO. Many sequencing libraries are only at SRA. Also, some early day microarray data is probably not on GEO. Besides, I only counted the number of *datasets*, not the samples within the datasets.

With that being said, I'm likely underestimate the sequencing data. 

A while back, I also wrote some Python code to query the number of DNA and RNA sequencing libraries on SRA. See `SRA_search.ipynb`. I wrote a R version before, but I can't find the script.

[Link to Github](https://github.com/timedreamer/NCBI_GEO_search/)

I got all the possible query terms for `Dataset Type` from [NCBI-GEO-Advanced Search](https://www.ncbi.nlm.nih.gov/gds/advanced).

![](images/p0.png)

Here are the results for the **Human**. The sequencing is totally taken up the GEO, only a small portion of array assays.

![](images/p1.png)

![](images/p2.png)

![](images/p3.png)

Here are all the possible **Dataset Type**

| Number 	| DataSet Type                                                       	|
|--------	|--------------------------------------------------------------------	|
| 1      	| expression profiling by array                                      	|
| 2      	| expression profiling by genome tiling array                        	|
| 3      	| expression profiling by snp array                                  	|
| 4      	| genome binding/occupancy profiling by array                        	|
| 5      	| genome binding/occupancy profiling by genome tiling array          	|
| 6      	| genome binding/occupancy profiling by snp array                    	|
| 7      	| genome variation profiling by array                                	|
| 8      	| genome variation profiling by genome tiling array                  	|
| 9      	| genome variation profiling by snp array                            	|
| 10     	| methylation profiling by array                                     	|
| 11     	| methylation profiling by genome tiling array                       	|
| 12     	| methylation profiling by snp array                                 	|
| 13     	| non coding rna profiling by array                                  	|
| 14     	| non coding rna profiling by genome tiling array                    	|
| 15     	| protein profiling by protein array                                 	|
| 16     	| snp genotyping by snp array                                        	|
| 20     	| expression profiling by high throughput sequencing                 	|
| 21     	| genome binding/occupancy profiling by high throughput   sequencing 	|
| 22     	| genome variation profiling by high throughput sequencing           	|
| 23     	| methylation profiling by high throughput sequencing                	|
| 24     	| non coding rna profiling by high throughput sequencing             	|
| 26     	| protein profiling by mass spec                                     	|
| 27     	| third party reanalysis                                             	|
| 17     	| expression profiling by mpss                                       	|
| 18     	| expression profiling by rt pcr                                     	|
| 19     	| expression profiling by sage                                       	|
| 25     	| other                                                              	|