# Setup PhysiCell on macOS &#x1F34F; &#x1F34E;


## Minimum installation

### Brew

+ Follow the instruction to download and install brew. Basically, copy the installation command, paste it into the Terminal (found at Applications / Utilities), and execute it.

https://brew.sh/

+ In the Terminal, after you have brew install, run the following commands:

```bash
brew install gcc imagemagick ffmpeg
```

### Python environment dedicated for PhysiCell

In this manual we will install everithing PhysiCell realted place in the ~/src folder.
If you prefere an oder foldername please adjust the commands accordingly.

We will generate a python3 environment with the default python installation, where we will install all PhysiCell modelling related python libraries.
We will name this python3 environment physienv, and we install it in the src folder where just before have installed PhysiCell.
Here we demonstrate, how to generate the environment with the regular python. If you run mamba or conda, please adjust the commands accordingly.

+ Generate a python environment named physienv and an alias for this environment for activation:

```bash
cd ~
mkdir -p ~/src
python3 -m venv src/physienv
echo "alias physienv=\"source /Users/$USER/src/physienv/bin/activate\"" >> ~/.zshenv
source ~/.zshenv
```

+ Activate the physienv python environment using the alias generated before:

```bash
physienv
```

### PhysiCell Studio

+ Download PhysiCell-Studio and install it in the ~/src folder.

```bash
cd ~/src
git clone https://github.com/PhysiCell-Tools/PhysiCell-Studio.git
pip3 install -r PhysiCell-Studio/requirements.txt
cd ~/src/physienv/bin
echo "python3 /Users/$USER/src/PhysiCell-Studio/bin/studio.py $*" > pcstudio
chmod 775 pcstudio
cd ~
```

+ Please check out the official PhysiCell Studio manual:

https://github.com/PhysiCell-Tools/Studio-Guide/tree/main


## Advanced installation

### PhysiCell

bue 20250221: can maybe be refinded, because it assumes brew is installed on default location.

```bash
cd ~/src
echo export PHYSICELL_CPP=$(ls /opt/homebrew/bin/ | grep '^g++-[0-9][0-9]') >> ~/.zshenv
git clone https://github.com/MathCancer/PhysiCell.git
```

### physicell data loader

Physicell data loader is a python library for downstream data analysis from your physicell runs from within python or from the command line.

To learn more check put the official pcdl manual:

+ https://github.com/PhysiCell-Tools/python-loader

```bash
pip3 install pcdl
```


## IDE VSCode integration

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

5. Link physienv (the python environment we generated above):

```
View | Command Palette… | Python: Select Interpreter |
Enter interpreter path… | Find… | src/physienv
```

