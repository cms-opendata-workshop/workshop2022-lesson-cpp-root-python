---
title: "Using uproot to open ROOT files"
teaching: 10 min
exercises: 10 min
questions:
- "How do I open a ROOT file with uproot?"
- "How do I explore what is in the file?"
objectives:
- "Use a different library than ROOT to open ROOT files"
- "Get comfortable with a different way of examining ROOT files"
keypoints:
- "You can use uproot to interface with ROOT files which is often easier than installing the full ROOT ecosystem."
---

# Other resources

Before we go any further, we point out that this episode and the next are only the
*most basic* introductions to `uproot` and `awkward`. There is a plethora of material
that go much deeper and we list just a few here. 

* [Official uproot documentation](https://uproot.readthedocs.io/en/latest/basic.html)
* [HSF uproot and awkward tutorial](https://hsf-training.github.io/hsf-training-uproot-webpage/aio/index.html)
* [Uproot, awkward, and columnar analysis](https://github.com/jpivarski-talks/2020-06-08-uproot-awkward-columnar-hats) 
from Jim Pivarski.

# How to type these commands?

Now that you've installed the necessary python modules you can choose to write and execute the code however 
you like. There are a number of options, but we will point out two here. 

* [Jupyter notebook](https://jupyter.org/). This provides an editor and an evironment in which to run
your python code. Often you will run the code one *cell* at a time, but you could always put all your 
code in one cell if you prefer. There are many, many tutorials out there on using Jupyter notebooks
and if you chose to use Jupyter as your editing/executing environment that you have developed some
familiarity with it. 

  * In the `my_python` container, you can start `jupyter-lab` with
  ~~~
  jupyter-lab --ip=0.0.0.0 --no-browser
  ~~~
  {: .language-bash} 
  
  and open the link given in the message on your browser. Choose the icon under "Notebook". 

* Python scripts. In this approach, you edit the equivalent of a text file and then pass that text
file into a python interpreter. For example, if you edited a file called `hello_world.py` such that
it contained

~~~
print("Hello world!")
~~~
{: .language-python}

You could save the file and then (perhaps in another Terminal window), execute

~~~
python hello_world.py
~~~
{: .language-bash}

This would interpret your text file as python commands and produce the output

~~~
Hello world!
~~~
{: .output}

We leave it to you to decide which approach you prefer.


# Open a file

Let's open a ROOT file! 
If you're writing a python script, let's call it `open_root_file.py` and if you're using
a Jupyter notebook, let's call it `open_root_file.ipynb`. If you are working in the container, you will open and *edit* the python script on your local computer and *run* it in the container, or you will open a notebook on your jupyter-lab window in the browser.

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

Let's open the file! We'll make use of `uproot`s use of [XRootD](https://xrootd.slac.stanford.edu/) to 
read in the file *over the network*. This will save us from having to download the file.

~~~
infile_name = 'root://eospublic.cern.ch//eos/opendata/cms/derived-data/AOD2NanoAODOutreachTool/ForHiggsTo4Leptons/SMHiggsToZZTo4L.root'

infile = uproot.open(infile_name)
~~~
{: .language-python}

> ## Download the file?
> If too many people are trying to open the same file, it may be easier to download the file 
> to your laptop.
> You can execute the following command in the bash terminal. 
>
> ~~~
> curl http://opendata.cern.ch/record/12361/files/SMHiggsToZZTo4L.root --output SMHiggsToZZTo4L.root
> ~~~
> {: .language-bash}
> Alternatively, you can follow [this link](http://opendata.cern.ch/record/12361) to the data record
> on the CERN Open Data Portal. If you scroll down to the bottom of the page and click 
> the **Download** button. 
> 
> For the remainder of this tutorial you will want the file to be in the same directory/folder
> as your python code, whether you are using a Jupyter notebook or a simple python script. So make
> sure you move this file to that location after you have downloaded it. 
> 
> To read in the file, you'll change one line to define the input file to be
> ~~~
> infile_name = 'SMHiggsToZZTo4L.root'
> ~~~
> {: .language-python}
{: .callout}

So you've opened the file with `uproot`. What is this `infile` object? Let's add the following code

~~~
print(type(infile))
~~~
{: .language-python}

and we get

~~~
<class 'uproot.reading.ReadOnlyDirectory'>
~~~
{: .output}

We can interface with this object similar to how we would interface
with a [python dictionary](https://www.w3schools.com/python/python_dictionaries.asp).


~~~
keys = infile.keys()

print(keys)
~~~
{: .language-python}

~~~
['Events;1']
~~~
{: .output}

But what is this? 
~~~
events = infile['Events']

print(type(events))
~~~
{: .language-python}

~~~
<class 'uproot.models.TTree.Model_TTree_v20'>
~~~
{: .output}

Ah, this is the `TTree` object that we learned a bit about in the previous episodes! Let's see what's in it!


~~~
branches = infile['Events'].keys()

for branch in branches:
    print(f"{branch:20s} {infile['Events'][branch]}")
~~~
{: .language-python}

~~~
run                  <TBranch 'run' at 0x7faa76d2cdd8>
luminosityBlock      <TBranch 'luminosityBlock' at 0x7faa76d2cda0>
event                <TBranch 'event' at 0x7faa76d13748>
PV_npvs              <TBranch 'PV_npvs' at 0x7faa76d13e10>
PV_x                 <TBranch 'PV_x' at 0x7faa76d194e0>
PV_y                 <TBranch 'PV_y' at 0x7faa76d19ba8>
PV_z                 <TBranch 'PV_z' at 0x7faa76d212b0>
nMuon                <TBranch 'nMuon' at 0x7faa76d21978>
Muon_pt              <TBranch 'Muon_pt' at 0x7faa76d5c080>
Muon_eta             <TBranch 'Muon_eta' at 0x7faa76d5c6d8>
Muon_phi             <TBranch 'Muon_phi' at 0x7faa76d5ccc0>
Muon_mass            <TBranch 'Muon_mass' at 0x7faa76d582e8>
Muon_charge          <TBranch 'Muon_charge' at 0x7faa76d588d0>
Muon_pfRelIso03_all  <TBranch 'Muon_pfRelIso03_all' at 0x7faa76d58eb8>
Muon_pfRelIso04_all  <TBranch 'Muon_pfRelIso04_all' at 0x7faa76d4e4e0>
Muon_dxy             <TBranch 'Muon_dxy' at 0x7faa76d4eac8>
Muon_dxyErr          <TBranch 'Muon_dxyErr' at 0x7faa7443a0f0>
Muon_dz              <TBranch 'Muon_dz' at 0x7faa7443a6d8>
Muon_dzErr           <TBranch 'Muon_dzErr' at 0x7faa7443ad30>
nElectron            <TBranch 'nElectron' at 0x7faa74442358>
Electron_pt          <TBranch 'Electron_pt' at 0x7faa74442940>
Electron_eta         <TBranch 'Electron_eta' at 0x7faa74442f28>
Electron_phi         <TBranch 'Electron_phi' at 0x7faa7444a550>
Electron_mass        <TBranch 'Electron_mass' at 0x7faa7444ab38>
Electron_charge      <TBranch 'Electron_charge' at 0x7faa74451160>
Electron_pfRelIso03_all <TBranch 'Electron_pfRelIso03_all' at 0x7faa74451748>
Electron_dxy         <TBranch 'Electron_dxy' at 0x7faa74451d30>
Electron_dxyErr      <TBranch 'Electron_dxyErr' at 0x7faa74459358>
Electron_dz          <TBranch 'Electron_dz' at 0x7faa74459940>
Electron_dzErr       <TBranch 'Electron_dzErr' at 0x7faa74459f28>
MET_pt               <TBranch 'MET_pt' at 0x7faa7445f550>
MET_phi              <TBranch 'MET_phi' at 0x7faa7445fbe0>
~~~
{: .output}

There are multiple syntax you can access each of these branches.


~~~
pt = infile['Events']['Muon_pt']

# or 

pt = infile['Events/Muon_pt']

# or

pt = events.Muon_pt

# or

pt = events['Muon_pt']
~~~
{: .language-python}

We'll use that last one for this lesson just to save some typing. :) 

In the next episode we'll use the `awkward` array object when we extract these data
and see how we can use `awkward` in a standard-but-slow way or in a clever-and-fast way!

{% include links.md %}

