---
title: "Installing python and the analysis tools locally"
teaching: 5
exercises: 20
questions:
- "How do I set up my local python environment?"
- "How do I install uproot and awkward?"
objectives:
- "Prepare your laptop or deskop computer to use python 3.x"
- "Install some standard libraries, including uproot and awkward"
keypoints:
- "It takes a finite amount of work to be able to read and and analyze ROOT files on your laptop."
- "You can do this *without* ever using the official ROOT libraries."
---

Python has become the primary analysis language for the majority of HEP experimentalists. It has a
rich ecosystem that is constantly evolving. This is a good thing because it means that improvements
and new tools are always being developed, but it can sometimes be a challenge to keep up with the 
latest and greatest projects! :)

In this section, we will set up an environment in which you can explore some more modern
python tools. We'll show you two approaches: 

* Make use of a Docker container (recommended)
* Install a *local* python environment on your laptop

In either case, these tools will allow you to can easily open
and analyze ROOT files. This is useful for when you make use of the CMS open data tools to skim 
some subset of the open data and then copy it to your local laptop, desktop, or perhaps an 
HPC cluster at your home institution. 

# Option 1: Making use of Docker

If you would rather not install python from Anaconda, or keep your existing python tools separate from the CMS open data work, you can use the python Docker container.
If you completed the [Docker pre-exercises](https://cms-opendata-workshop.github.io/workshop2022-lesson-docker/) 
you should already have worked through 
[this episode](https://cms-opendata-workshop.github.io/workshop2022-lesson-docker/03-docker-for-cms-opendata/index.html), under **Download the docker images for ROOT and python tools and start container**, and you will have

- a working directory `cms_open_data_python` on your local computer
- a docker container with name `my_python` created with the working directory `cms_open_data_python` mounted into the `/code` directory of the container.

Start your python container with

~~~
docker start -i my_python
~~~
{: .language-bash}

In the container, you will be in the `/code` directory and it shares the files with your local `cms_open_data_python` directory.

If you want to test out the installation, from within Docker you can launch and 
interactive python session by typing `python` (in Docker) and then trying

~~~
import uproot
import awkward as ak
~~~
{: .language-python}

If you don't get any errors then congratulations! You have a working environment and you are ready to
perform some HEP analysis with your new python environment!

# Option 2: Installing python with Anaconda

We recommend working with a newer python release, preferably python 3.7 or higher. Even if you have 
a standard python installation (as comes with many Mac computers), we recommend installing
the popular (and free) [Anaconda python distribution](https://www.anaconda.com/). This will give you an easily extendible
python ecosystem. To install it for Windows, Mac, or Linux, head to 

[https://www.anaconda.com/products/distribution](https://www.anaconda.com/products/distribution)

Follow the instructions to install the Anaconda version of python and make it your default version. You'll
need between to 600 MB to 1 GB free disk space and depending on the speed of your connection, it can
take up to 15 minutes to download. 

## Install the extra packages

We now want to install some additional python libraries.

* [numpy](https://numpy.org/), a very useful library to do fast, efficient manipulations of arrays of numbers.
* [matplotlib](https://matplotlib.org/), for most people this is the standard plotting library.
* [uproot](https://uproot.readthedocs.io/en/latest/index.html), a more recent library to efficiently open ROOT files, effectively freeing you from needing to install the entire ROOT package.
* [awkward](https://awkward-array.readthedocs.io/en/latest/), also a more recent library that lets us perform fast, efficient calculations on the *jagged* arrays that we so often encounter in HEP datasets. More on this later...
* [xrootd](https://xrootd.slac.stanford.edu/), a library that allows ROOT and `uproot` to access files over the web, provided
they are refereced in a particular way. 

`numpy` and `matplotlib` are most likely already installed when you installed your Anaconda distribution. But lets test this out!

### Test on your local bash terminal

Open a bash terminal (native Linux shell, Terminal on Mac, or WSL2 Linux bash shell) and type

~~~
python
~~~
{: .language-bash}

If the Anaconda installation was installed properly, you should get output that looks something like

~~~
Python 3.7.9 (default, Aug 31 2020, 12:42:55)
[GCC 7.3.0] :: Anaconda, Inc. on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
~~~
{: .output}

You will most likely have a different version of python (perhaps 3.8 or 3.9), but it should definitely 
say *Anaconda* somewhere in that output. 

In this environment, lets see if we can `import` some of these libraries. At the prompt (`>>>`), type
the following lines, hitting *Enter* (or *Return*) after you type them. 

~~~
import numpy as np
import matplotlib.pylab as plt
~~~
{: .language-python}

If it all worked, you should see no output. If you get errors, reach out to the organizers
through Mattermost.

Type `quit()` to exit and return to the shell. 

### Creating a python virtual environment

We will now use the Anaconda tools to install a 
[*virtual environment*](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html).
This creates an environment with a specific version of python with very specific libraries installed.
By creating this environment all at once, you can often avoid some of the conflicts that cause `pip` to take 
a long time or fail. 

If you have installed Anaconda, you should be able to open a terminal or on Windows, the Anaconda command line
tool. We'll be making use of the `conda` tool, provided by Anaconda. You can make sure it's there by typing

~~~
conda --version
~~~
{: .language-bash}

~~~
conda 4.12.0
~~~
{: .output}

You might not get that exact version, but that's OK, so long as you get some sort of version. 

You can now use `conda` to create a virtual environment with the modern python tools we'll be using
for this pre-exercise and later lessons. We'll call this environment `pyhep`. 

~~~
conda create --name pyhep root matplotlib xrootd awkward uproot numpy jupyter-notebook
~~~
{: .language-bash}

It might take a while (10-20 minutes). Once it's created, you can then type

~~~
conda activate pyhep
~~~
{: .language-bash}

Now go into the python interpreter by typing `python` and then try typing the following in that interpreter.

~~~
import uproot
import awkward as ak
~~~
{: .language-python}

If you don't get any errors then congratulations! You have installed the necessary libraries and are ready to
perform some HEP analysis with your new python environment!

and when you use `python` or `jupyter` you'll be able to use all these libraries. Just make sure
you *first* activate this virtual environment with 

~~~
conda activate pyhep
~~~
{: .language-bash}


{% include links.md %}
