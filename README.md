# howtodocument

Here I will document the tools I use to  work on the AndroidAPS documentation.
 
## operating system
I use **Manjaro Linux 21.2.5**. It's not important but there the tools are available und work very well.

## Github
I use **Github Desktop** to handle the communication with Github.

I used the AppImage to set it up and had to install gnome-keyring manually to get it working.

Afterwords it runs very well and I have a good tool to handle the Github changes.

It's important to alwas start a branch for the work and then bring it later with a PR into the main branch.
The temporary branch can then be deleted. Maybe there is better solution later with merge a branch into another but I don't know it at the moment for sure. This way it works. ;-)

![Github Desktop](githup-desktop.png)


## md/rst-Editor
For editing md/rst-files I use the simple tool **ReText** which can handle both formats.

You can switch to a double view with preview on the right by using the key combinatation CTRL-L.

![ReText](retext.png)


## Screenshot-Tool
I use **ksnip** for makeing screenshots and comment them directly in the tool.

The comment functionaltity is very good.

![KSnip](ksnip.png)

## Resizing of Images
I use **Gwenview** for resizing the images.

![Gwenview](gwenview.png)

# setting up a local sphinx environment

export WORKDIR=$HOME/work/tryrun
rm -rf $WORKDIR
mkdir $WORKDIR
cd $WORKDIR
git clone https://github.com/peterleimbach/AndroidAPSdocs.git
python -m venv .venv 
source .venv/bin/activate 
cd .venv

# besser wäre, aber die Versionen sind sehr alt pip install -r requirements.txt
pip install --upgrade pip
pip install sphinx
pip install myst_parser
pip install recommonmark
pip install sphinx_rtd_theme

mkdir docs
cp -r ../AndroidAPSdocs/docs/EN docs/source

cp -r ../AndroidAPSdocs/docs/_static/ docs
cp -r ../AndroidAPSdocs/docs/_templates docs

cp ../AndroidAPSdocs/docs/drawing.png docs
cp ../AndroidAPSdocs/docs/favicon.ico docs

cp $HOME/work/preparemigration/.venv/docs/Makefile $WORKDIR/.venv/docs
cp $HOME/work/preparemigration/.venv/docs/shared.conf.py $WORKDIR/.venv/docs

cp $HOME/work/preparemigration/.venv/docs/source/conf.py $WORKDIR/.venv/docs/source

cd docs
make html

cd build/html
python3 -m http.server
