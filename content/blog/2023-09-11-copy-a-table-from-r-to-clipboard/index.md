---
title: Copy a table from R to clipboard
author: Ji Huang
date: '2023-09-11'
categories:
  - dry lab
tags:
  - bioinformatics
  - R
meta_img: images/image.png
description: Description for the page
---


I learned using `clipr::write_clip()` can easily copy a table to clipboard and then copy to Excel for better formatting.

For a long time, I have problems copy a simple table from R/Rstudio to clipboard, until today. I saw a post from [here](https://community.rstudio.com/t/how-to-copy-and-paste-my-table-in-r-to-excel/82540/5).

```r
tt1 <- iris %>% head()
clipr::write_clip(tt1)

# Or
iris %>% head() %>% clipr::write_clip()
```
Then you can copy it to Excel. The format looks good.

For copying table from Excel to R, I use [datapasta](https://github.com/MilesMcBain/datapasta).