# how to setup a local sphinx test environment for the documentation?

## prerequiteries

Th documentation was written on a Linux pc with python 3 installed.

It should run on Windows and Mac very simliar but the author uses Linux.

To keep things a simple as possible 
+ I use a virtual python environment which is seperate from the python environment of the pc.
=> Otherwise I risk to crash the global python environment of the machine which is not necessary at all.
+ I generate the html in a seperate environment and not the GitHub source.
=> Otherwise I risk to generate the HTML and temporary files in the GitHub tree and git would ask me if this should synched back to the cloned git which does not make sense as I only want to pull reqeust the markdown source to Github.

## Step 1 : clone the documentation to your pc
git clone https://github.com/openaps/AndroidAPSdocs.git

This results on my pc at $HOME/GitHub/AndroidAPSDocs

## Step 2: create a virtual test python environment

$ python -m venv $HOME/dev/AndroidAPSDocs/.venv
$ source $HOME/dev/AndroidAPSDocs/.venv/bin/activate 

Important: Now my virutal python environment is active and I can use it as long as I don't close this terminal. If I need to restart the machine e.g. next day. I need to execute the source command again to set the virutal environment again!

## install the needed python packages for sphinx

In general we have a small file with the required packages which we use in the last command here. As we run on ReadTheDocs I use two command before as ReadTheDocs  uses it to install some dependencies!

$ python -m pip install --upgrade --no-cache-dir pip "setuptools<58.3.0"

$ python -m pip install --upgrade --no-cache-dir pillow "mock==1.0.1" "alabaster>=0.7,<0.8,!=0.7.5" "commonmark==0.9.1" "recommonmark==0.5.0" "sphinx<2" "sphinx-rtd-theme<0.5" "readthedocs-sphinx-ext<2.3" "jinja2<3.1.0"

$ python -m pip install --exists-action=w --no-cache-dir -r $HOME/Dokumente/GitHub/AndroidAPSdocs/requirements.txt

## generate the html files from the markdown files

$ python -m sphinx -T -E -b html -d $HOME/dev/AndroidAPSDocs/_build/doctrees -D language=en $HOME/Dokumente/GitHub/AndroidAPSdocs/docs/EN $HOME/dev/AndroidAPSDocs/html

## start a local http server to serve your generate html files

The easiest way to do this is to use the build in python http server.

It's best to start this in a seperate terminal environment because it does not need to be restarted after a new generation as it automatically will pick the new version from file system.

Therefore just cd in the html directory with the generated html files and start the http server.

$ cd $HOME/dev/AndroidAPSDocs/html
$ python3 -m http.server 
