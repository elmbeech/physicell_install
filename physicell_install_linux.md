# Setup PhysiCell on Linux

This document describes the PhysiCell installation on a Debian Linux distribution.
If you run on another flavor, please adjust accordingly.

We will install everithing PhysiCell realted under the ~/src folder.
If you prefere another foldername please adjust the commands accordingly.


## &#x1F427; Operating system library dependencies

### &#x2728; Update the package manager:

```bash
sudo apt update
sudo apt upgrade
```

### &#x2728; Install GCC (required by PhysiCell):

```bash
sudo apt install build-essential
```

### &#x2728; Install Image Magick (required by PhysiCell):

ImageMagick is used for making jpeg and gif images from PhysiCell svg image output.

The PhysiCell `Makefile` is written for **ImageMagick >=  version 7**, which requires a magick command in front of each ImageMagick command (e.g. magick convert instead of convert).
Many Linux distrubution ship with **ImageMagick version 6**.
This is why we might have to tewak a bit the installation.

```bash
sudo apt install imagemagick
```

Now try:
```bash
magick --version
```

1. If ok: you have >= version 7 installed. You are all set!
2. If you receive: Command 'magick' not found, try:
```bash
convert --version
```
3. If ok: you have version 6 or less installed.

**If and only if you have <= version 6 installed**, you can follow the instruction below to generate a magick command that simply passes everything to the next command. This will make the PhysiCell Makefile work for you too.

`cd` to the folder where you have your manual installed binaries. e.g. `~/.local/bin/`

```bash
echo '$*' > magick
chmod 775 magick
which magick
```

### &#x2728; Install FFmpeg, Tar, Gzip, and Unzip (required by PhysiCell):

```bash
sudo apt install ffmpeg tar gzip unzip
```

### &#x2728; Install the Qt 5 library (required by PhysiCell-Studio):
```
sudo apt install qtbase5-dev
```

### &#x2728; Install Python:

Python is most probbaly already installed, but pip might be missing (required by PhysiCell-Studio and pcdl).

```bash
sudo apt install python3-pip
```


## &#x1F427; &#x1F427; Basic PhyiCell installation

### &#x2728; Install  PhysiCell:

```bash
mkdir -p ~/src
cd ~/src
curl -L https://github.com/MathCancer/PhysiCell/archive/refs/tags/$(curl https://raw.githubusercontent.com/MathCancer/PhysiCell/master/VERSION.txt).zip > download.zip
unzip download.zip
rm download.zip
rm -fr PhysiCell
mv PhysiCell-$(curl https://raw.githubusercontent.com/MathCancer/PhysiCell/master/VERSION.txt) PhysiCell
```

### &#x2728; Test the PhyiCell installation with the template sample project:

```bash
cd ~/src/PhysiCell
```
```bash
make template
```
```bash
make -j8
```
```bash
./project
```
```bash
make jpeg
```
```bash
make gif
```
```bash
make movie
```


## &#x1F427; &#x1F427; &#x1F427; Essential installation

We will generate a python3 environment with the default python installation, where we will install all PhysiCell modelling related python libraries.
We will name this python3 environment pcpyenv (PhysiCell Python environment).

For generate the environment with assume a regular python installation.
If you run mamba or conda, please adjust the commands accordingly.

### &#x2728; Install PhysiCell-Studio:

```bash
cd ~/src
python3 -m venv pcpyenv
if ! grep -Fq 'alias pcpyenv=' ~/.bash_aliases
then
    echo "alias pcpyenv=\"source /home/$USER/src/pcpyenv/bin/activate\"" >> ~/.bash_aliases
fi
source ~/.bash_aliases
pcpyenv
curl -L https://github.com/PhysiCell-Tools/PhysiCell-Studio/archive/refs/tags/v$(curl https://raw.githubusercontent.com/PhysiCell-Tools/PhysiCell-Studio/refs/heads/main/VERSION.txt).zip > download.zip
unzip download.zip
rm download.zip
rm -fr PhysiCell-Studio
mv PhysiCell-Studio-$(curl https://raw.githubusercontent.com/PhysiCell-Tools/PhysiCell-Studio/refs/heads/main/VERSION.txt) PhysiCell-Studio
pip3 install -r PhysiCell-Studio/requirements.txt
cd ~/src/pcpyenv/bin/
echo "python3 /home/$USER/src/PhysiCell-Studio/bin/studio.py \$*" > pcstudio
chmod 775 pcstudio
cd ~/src
```

### &#x2728; Test the PhysiCell-Studio installation:

```bash
cd ~/src/PhysiCell
pcpyenv
pcstudio
```

### &#x2728; Official PhysiCell Studio manual:

https://github.com/PhysiCell-Tools/Studio-Guide/tree/main


## &#x1F427; &#x1F427; &#x1F427; &#x1F427; Advanced installation

### &#x2728; Install PhysiCell data loader (pcdl) and iPython:

```bash
pcpyenv
pip3 install pcdl ipython
```
### &#x2728; Test the pcdl installation:

```bash
ipython
```
```python
import pcdl
```
```python
exit()
```

### &#x2728; Official pcdl manual:

+ https://github.com/PhysiCell-Tools/python-loader



## &#x1F427; &#x1F427; &#x1F427; &#x1F427; IDE VSCode integration (optional)

1. Install vs code, either from your operating system’s app store or from https://code.visualstudio.com/ .

2. Generate a vs code profile for physicell:

```
File | New Window with Profile
Name: physicell
Icon: choose something cool.
Create
Add Folder: Home/src
click the profile icon (default is a gearwheel) on the left side bottom corner.
Profile > physicell
```

3. Open the Folder:

```
File | Open Folder… | src | Open
Yes, I trust the authors
```

4. Install the official python and C++ extensions into the profile:

```
click the profile icon (default is a gearwheel) on the left side bottom corner.
Profile > physicell
Extension: Python Install
Extension: C/C++ Install
```

5. Link pcpyenv (the python environment we generated above):

```
View | Command Palette… | Python: Select Interpreter |
Enter interpreter path… | Find… | src/pcpyenv
```
