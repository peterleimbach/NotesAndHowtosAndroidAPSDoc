# how to setup a local sphinx test environment for the documentation?

## prerequiteries

The documentation was written on a Linux pc with python 3 installed.

It should run on Windows and Mac very simliar but the author uses Linux.

To keep things a simple as possible 
+ I use a virtual python environment which is seperate from the python environment of the pc.
=> Otherwise I risk to crash the global python environment of the machine which is not necessary at all.
+ I generate the html in a seperate environment and not the GitHub source.
=> Otherwise I risk to generate the HTML and temporary files in the GitHub tree and git would ask me if this should synched back to the cloned git which does not make sense as I only want to pull reqeust the markdown source to Github.

## Step 1 : clone the documentation to your pc

```sh
git clone https://github.com/openaps/AndroidAPSdocs.git
```

This results on my pc at $HOME/GitHub/AndroidAPSDocs

## Step 2: create a virtual test python environment

```sh
$ python -m venv $HOME/dev/AndroidAPSDocs/.venv

$ source $HOME/dev/AndroidAPSDocs/.venv/bin/activate 
```

Important: Now my virutal python environment is active and I can use it as long as I don't close this terminal.

If I need to restart the machine e.g. next day I will need to execute the source command again to set the virutal environment again!

## install the needed python packages for sphinx

In general we have a small file with the required packages which we use in the last command here. As we run on ReadTheDocs I use two command before as ReadTheDocs  uses it to install some dependencies!

```sh
$ python -m pip install --upgrade --no-cache-dir pip "setuptools<58.3.0"

$ python -m pip install --upgrade --no-cache-dir pillow "mock==1.0.1" "alabaster>=0.7,<0.8,!=0.7.5" "commonmark==0.9.1" "recommonmark==0.5.0" "sphinx<2" "sphinx-rtd-theme<0.5" "readthedocs-sphinx-ext<2.3" "jinja2<3.1.0"

$ python -m pip install --exists-action=w --no-cache-dir -r $HOME/Dokumente/GitHub/AndroidAPSdocs/requirements.txt
```

## generate the html files from the markdown files

```sh
$ python -m sphinx -T -E -b html -d $HOME/dev/AndroidAPSDocs/_build/doctrees -D language=en $HOME/Dokumente/GitHub/AndroidAPSdocs/docs/EN $HOME/dev/AndroidAPSDocs/html
```

## start a local http server to serve your generated html files

The easiest way to do this is to use the build in python http server.

It's best to start this in a seperate terminal environment because it does not need to be restarted after a new generation as it automatically will pick the new version from file system.

Therefore just cd in the html directory with the generated html files and start the http server.
```sh
$ cd $HOME/dev/AndroidAPSDocs/html

$ python3 -m http.server 
```


# Additional notes for windoz users... 
I got it to work fairly well by using git bash so we can use the same commands on windows/linux/mac... I am also enabling autobuild which allow changes saved to get reflected almost immediately in your browser...  

## pre-requisites (20-45 minutes?)
tested on windows 11
+ install git bash https://gitforwindows.org/ 
+ install tortoise git https://tortoisegit.org/download/
+ install python3 &  pip
+ install Visual Studio Code from https://code.visualstudio.com/ 
+ there are gazillions of plugins for visual studio code, you want to install the followings:
    + python Name: Python https://marketplace.visualstudio.com/items?itemName=ms-python.python
    + Name: MyST-Markdown  https://marketplace.visualstudio.com/items?itemName=ExecutableBookProject.myst-highlight
    + Name: Markdown All in One https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one

## First Time SETUP  (15 minutes):
1. Select your folders in windows

+ Select 1 folder to store the local git repository copy where the source files will be edited we will call it **aaps_local_sources** (I used C:\AAPSDOC2\local_sources) 
+  1 folder to store the actual html web site for testing we will call it **aaps_local_web**  (I used C:\AAPSDOC2\local_web)...  

<br>


2. start a git bash window and set your folders values

    note that with git bash, 'C:' becomes '/c'  and your windows '\\' become '/'
    ```sh
    cd ~
    echo aaps_local_sources="/c/AAPSDOC2/local_sources" >> .bash_profile
    echo aaps_local_web="/c/AAPSDOC2/local_web/AndroidAPSDocs" >> .bash_profile
    
    ```
3. (in the same git bash window) create the folders, git local repo, python venv,  and install sphinx/myst/autobuild
    ``` sh
    . .bash_profile
    echo alias aapscode="'source  $aaps_local_web/.venv/Scripts/activate  && code $aaps_local_sources/AndroidAPSdocs/'" >> .bash_profile
    echo alias aapsbuild="'source  $aaps_local_web/.venv/Scripts/activate  && sphinx-autobuild --open-browser  --port 8004 $aaps_local_sources/AndroidAPSdocs/docs/EN/ $aaps_local_web/'"  >> .bash_profile

    . .bash_profile
    echo "creating the source and target webserver folders"
    mkdir -p $aaps_local_sources  $aaps_local_web

    echo "coling the git repo"
    cd $aaps_local_sources
    git clone https://github.com/openaps/AndroidAPSdocs.git
    
    echo "creating the python virtual env and installing stuffs"
    python -m venv $aaps_local_web/.venv    
    source  $aaps_local_web/.venv/Scripts/activate  ||    source $aaps_local_web/.venv/bin/activate
    cd $aaps_local_web
    python -m pip install --upgrade --no-cache-dir pip "setuptools<58.3.0"
    python -m pip install --upgrade --no-cache-dir pillow "mock==1.0.1" "alabaster>=0.7,<0.8,!=0.7.5" "commonmark==0.9.1" "recommonmark==0.5.0" "sphinx<2" "sphinx-rtd-theme<0.5" "readthedocs-sphinx-ext<2.3" "jinja2<3.1.0"
    python -m pip install --exists-action=w --no-cache-dir -r $aaps_local_sources/AndroidAPSdocs/requirements.txt
    python -m pip install sphinx-autobuild

    echo "building the first time with sphinx"
    python -m sphinx -T -E -b html -d $aaps_local_web/_build/doctrees -D language=en $aaps_local_sources/AndroidAPSdocs/docs/EN $aaps_local_web/html


    ```

## to work on MD documents and see the results locally *almost* right away... 
4. open a git bash window and run:
    ```sh
        aapscode  
        # this will open VisualStudioCode on the top folder of your source, then you can open files under AndroidAPS/docs/EN/

        appsbuild 
        # this will start a mini web server with a local copy of the built files. they get refreshed automatically everytime you save a file in visual studio code (it takes a few seconds though...)

    ```




