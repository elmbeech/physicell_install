# Setup PhysiCell on macOS
<!-- &#x1F34F; -->

We will install everithing PhysiCell realted under the src folder that we will place in your home directory.
If you prefere another foldername please adjust the commands accordingly.


## &#x1F34E; Operating system dependencies.

+ Install Brew

Follow the instruction to download and install brew.
Basically, copy the installation command, paste it into the Terminal (found at Applications / Utilities), and execute it.

https://brew.sh/

+ Install GCC, ImageMagick, and FFmpeg (required by PhysiCell):

```bash
brew install gcc imagemagick ffmpeg
```


## &#x1F34E; Basic PhyiCell installation

```bash
mkdir -p ~/src
cd ~/src
echo export PHYSICELL_CPP=$(compgen -c | grep -m 1 -e '^g++-[0-9]\+') >> ~/.zshenv
git clone https://github.com/MathCancer/PhysiCell.git
```

+ Test the installation with the template sample project.

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


## &#x1F34E; Essential installation

We will generate a python3 environment with the default python installation, where we will install all PhysiCell modelling related python libraries.
We will name this python3 environment pcpyenv (PhysiCell Python environment).

For generate the environment with assume a regular python installation.
If you run mamba or conda, please adjust the commands accordingly.

+ Install PhysiCell-Studio.

Open a Terminal (found at Applications / Utilities).

```bash
cd ~
mkdir -p ~/src
python3 -m venv src/pcpyenv
echo "alias pcpyenv=\"source /Users/$USER/src/pcpyenv/bin/activate\"" >> ~/.zshenv
source ~/.zshenv
pcpyenv
cd ~/src
git clone https://github.com/PhysiCell-Tools/PhysiCell-Studio.git
pip3 install -r PhysiCell-Studio/requirements.txt
cd ~/src/pcpyenv/bin
echo "python3 /Users/$USER/src/PhysiCell-Studio/bin/studio.py \$*" > pcstudio
chmod 775 pcstudio
cd ~
```

+ Test the installation.

```bash
cd ~/src/PhysiCell
pcpyenv
pcstudio
```
+ To learn more, please check out the official PhysiCell Studio manual:

https://github.com/PhysiCell-Tools/Studio-Guide/tree/main


## &#x1F34E; Advanced installation

+ Install PhysiCell data loader and iPython

```bash
pcpyenv
pip3 install pcdl ipython
```
+ Test installation

```bash
ipython
```
```python
import pcdl
```
```python
exit()
```

To learn more, please check put the official pcdl manual:

+ https://github.com/PhysiCell-Tools/python-loader


## &#x1F34E; IDE VSCode integration (optional)

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
