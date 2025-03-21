---
title: View code from jupyter notebook in the terminal
author: Ji Huang
date: '2025-03-20'
slug: []
categories:
  - dry lab
tags:
  - python
meta_img: images/image.png
description: Description for the page
---

Jupyter notebook is convenient for interactive coding, but sometimes I just need to view the code quickly in the terminal. I used to use [nbconvert](https://nbconvert.readthedocs.io/en/latest/index.html) which is OK. Sometimes, it's a little bit slow.

Now I use [jupytext](https://jupytext.readthedocs.io/en/latest/) which feels faster in the terminal. Most of time, I don't need the python file. 

So I ask ChatGPT to write a bash function to view the jupyter notebook in the terminal without saving the intermediate python script. Below is the function. I added it into the `.bashrc`.

To use, `jpyview your_notebook.ipynb`.


```bash
# Function to convert a Jupyter Notebook to a Python script and open it in Vim
# with Python syntax highlighting
jpyview() {
    if [ "$#" -ne 1 ]; then
            echo "Usage: jpyview <notebook.ipynb>"
                return 1
                fi

                jupytext --to py "$1" -o - | vim -c "set filetype=python" -
            }

```


