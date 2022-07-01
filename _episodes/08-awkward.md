---
title: "Using awkward arrays to analyze HEP data"
teaching: ???
exercises: ???
questions:
- "What are awkward arrays?"
- "How do I work with awkward arrays?"
objectives:
- "Learn to use awkward arrays in a simple, naive way."
- "Learn to use awkward arrays in a much faster way, making use of the built-in functionality of the library"
keypoints:
- "Awkward arrays can help speed up your code, but it requires a different way of thinking and more awareness of what functions are coded up in the awkward library."
---

# Download a ROOT file for use with this exercise

Let's grab a ROOT file to use for this lesson. If you are doing this lesson on a Mac or Linux computer, 
you have the option to simply execute the following command in the terminal. 

~~~
curl http://opendata.cern.ch/record/12361/files/SMHiggsToZZTo4L.root --output SMHiggsToZZTo4L.root
~~~
{: .language-bash}

Alternatively, you can follow [this link](http://opendata.cern.ch/record/12361) to the data record
on the CERN Open Data Portal. If you scroll down to the bottom of the page and click 
the **Download** button. 

For the remainder of this tutorial you will want the file to be in the same directory/folder
as your python code, whether you are using a Jupyter notebook or a simple python script. So make
sure you move this file to that location after you have downloaded it. 


# Open the file

Let's open this ROOT file! 
If you're writing a python script, let's call it `open_root_file.py` and if you're using
a Jupyter notebook, let's call it `open_root_file.ipynb`. 

First we will import the `uproot` library, as well as some other standard
libraries. These can be the first lines of your python script or the first cell of your Jupyter notebook.

*If this is a script, you may want to run `python open_root_file.py` every few lines or so to see the output.
If this is a Jupyter notebook, you will want to put each snippet of code in its own cell and execute
them as you go to see the output.*


~~~
import numpy as np
import matplotlib.pylab as plt

import uproot
import awkward as ak
~~~
{: .language-python}

Let's open the file!

~~~
infile_name = 'SMHiggsToZZTo4L.root'

infile = uproot.open(infile_name)

events = infile['Events']

~~~
{: .language-python}


{% include links.md %}

