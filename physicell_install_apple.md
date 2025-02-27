# Setup PhysiCell on macOS

We will install everything PhysiCell related under the src folder that we will place in your home directory.
If you prefer another folder name, please adjust the commands accordingly.


## &#x1F34E; Operating system dependencies.

### &#x2728; Install Brew:

Follow the instruction to download and install brew.
Basically, copy the installation command, paste it into the Terminal (found at Applications / Utilities), and execute it.

+ https://brew.sh/

Don't forget to put brew under your $PATH.

```bash
echo "export PATH=/opt/homebrew/bin:$PATH" >> ~/.bash_profile
source ~/.bash_profile
```

### &#x2728; Install GCC, ImageMagick, and FFmpeg (required by PhysiCell):

```bash
brew install gcc imagemagick ffmpeg
```


## &#x1F34E; Basic PhyiCell installation

### &#x2728; Install PhysiCell:

```bash
install='Y'
uart='Y'
if [ -d ~/src/PhysiCell ]
then
    echo "WARNING : /Users/$USER/src/PhysiCell already exists! do you wanna re-install? data will be lost! [Y,N]"
    read uart
if [ $install == $uart ]
then
    mkdir -p ~/src
    cd ~/src
    if ! grep -Fq 'export PHYSICELL_CPP=' ~/.zshrc
    then
        echo export PHYSICELL_CPP=$(bash -c "compgen -c" | grep -m 1 -e '^g++-[0-9]\+') >> ~/.zshrc
    fi
    if ! grep -Fq 'export PHYSICELL_CPP=' ~/.bash_profile
    then
        echo export PHYSICELL_CPP=$(bash -c "compgen -c" | grep -m 1 -e '^g++-[0-9]\+') >> ~/.bash_profile
    fi
    if ps -p $$ | grep zsh
    then
        source ~/.zshrc
    else
        source ~/.bash_profile
    fi
    curl -L https://github.com/MathCancer/PhysiCell/archive/refs/tags/$(curl https://raw.githubusercontent.com/MathCancer/PhysiCell/master/VERSION.txt).zip > download.zip
    unzip download.zip
    rm download.zip
    rm -fr PhysiCell
    mv PhysiCell-$(curl https://raw.githubusercontent.com/MathCancer/PhysiCell/master/VERSION.txt) PhysiCell
else
    echo 'installation terminated.'
fi
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


## &#x1F34E; Essential installation

We will generate a python3 environment with the default python installation, where we will install all PhysiCell modelling related python libraries.
We will name this python3 environment pcvenv (PhysiCell virtual environment).

### &#x2728; Install PhysiCell-Studio:

Open a Terminal (found at Applications / Utilities).

```bash
install='Y'
uart='Y'
if [ -d ~/src/PhysiCell-Studio ]
then
    echo "WARNING : /Users/$USER/src/PhysiCell-Studio already exists! do you wanna re-install? data will be lost! [Y,N]"
    read uart
if [ $install == $uart ]
then
    cd ~/src
    python3 -m venv pcvenv
    if ! grep -Fq 'alias pcvenv=' ~/.zshrc
    then
        echo "alias pcvenv=\"source /Users/$USER/src/pcvenv/bin/activate\"" >> ~/.zshrc
    fi
    if ! grep -Fq 'alias pcvenv=' ~/.bash_profile
    then
        echo "alias pcvenv=\"source /Users/$USER/src/pcvenv/bin/activate\"" >> ~/.bash_profile
    fi
    if ps -p $$ | grep zsh
    then
        source ~/.zshrc
    else
        source ~/.bash_profile
    fi
    pcvenv
    curl -L https://github.com/PhysiCell-Tools/PhysiCell-Studio/archive/refs/tags/v$(curl https://raw.githubusercontent.com/PhysiCell-Tools/PhysiCell-Studio/refs/heads/main/VERSION.txt).zip > download.zip
    unzip download.zip
    rm download.zip
    rm -fr PhysiCell-Studio
    mv PhysiCell-Studio-$(curl https://raw.githubusercontent.com/PhysiCell-Tools/PhysiCell-Studio/refs/heads/main/VERSION.txt) PhysiCell-Studio
    pip3 install -r PhysiCell-Studio/requirements.txt
    cd ~/src/pcvenv/bin/
    echo "python3 /Users/$USER/src/PhysiCell-Studio/bin/studio.py \$*" > pcstudio
    chmod 775 pcstudio
    cd ~/src
else
    echo 'installation terminated.'
fi
```

### &#x2728; Test the PhysiCell-Studio installation:

```bash
cd ~/src/PhysiCell
pcvenv
pcstudio
```

### &#x2728; Official PhysiCell Studio manual:

+ https://github.com/PhysiCell-Tools/Studio-Guide/tree/main


## &#x1F34E; Advanced installation

### &#x2728; Install PhysiCell Data Loader (pcdl) and iPython:

```bash
pcvenv
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


## &#x1F34F; IDE VSCode integration (optional)

1. Install vs code, either from your operating system’s app store or from https://code.visualstudio.com/ .

2. Generate a vs code profile for physicell:

```
File | New Window with Profile
Name: physicell
Icon: choose something cool.
Create
Add Folder: Users/<username>/src
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

5. Link pcvenv (the python environment we generated above):

```
View | Command Palette… | Python: Select Interpreter |
Enter interpreter path… | Find… | src/pcvenv
```
