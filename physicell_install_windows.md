# Setup PhysiCell on Windows

We will install everithing PhysiCell realted under the src folder that we will place in your Windows Home directory.
If you prefere another foldername please adjust the commands accordingly.

## &#x1FA9F; Operating system dependencies

### &#x2728; Download and install MSYS2 x86_64:

+ https://www.msys2.org/

### &#x2728; Install GCC, ImageMagick, Unzip, Zip, and ca-certificates:

Open the MSYS2 MINGW64 shell.

```bash
pacman -S mingw-w64-x86_64-gcc make
pacman -S mingw-w64-x86_64-imagemagick mingw-w64-x86_64-ffmpeg
pacman -S unzip zip
pacman -S mingw-w64-x86_64-ca-certificates
```


## &#x1FA9F; Basic PhyiCell installation

### &#x2728; Install PhysiCell:

Open the MSYS2 MINGW64 shell.

```bash
mkdir -p /c/Users/$USER/src
cd /c/Users/$USER/src
curl -L https://github.com/MathCancer/PhysiCell/archive/refs/tags/$(curl https://raw.githubusercontent.com/MathCancer/PhysiCell/master/VERSION.txt).zip > download.zip
unzip download.zip
rm download.zip
rm -fr PhysiCell
mv PhysiCell-$(curl https://raw.githubusercontent.com/MathCancer/PhysiCell/master/VERSION.txt) PhysiCell
```

### &#x2728; Test the PhysiCell installation with the template sample project:

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


## &#x1FA9F; Essential installation

We will generate a python3 environment with the default python installation, where we will install all PhysiCell modelling related python libraries.
We will name this python3 environment pcvenv (PhysiCell virtual environment).

### &#x2728; Install Python:

If you not already have installed python, please go to the Microsoft Stor and install the latest Python from the Python Software Foundation.

### &#x2728; Install PhysiCell-Studio:

Open a PowerShell.

```powershell
Set-ExecutionPolicy Unrestricted -Scope CurrentUser
```

```powershell
Set-Location ~\src
python.exe -m venv pcvenv
if (Test-Path ~\Documents\WindowsPowerShell) {} else {New-Item ~\Documents\WindowsPowerShell -Type Directory}
if (Test-Path ~\Documents\WindowsPowerShell\profile.ps1) {} else {New-Item ~\Documents\WindowsPowerShell\profile.ps1 -Type File}
$scavenge = Get-Content ~\Documents\WindowsPowerShell\profile.ps1
if ($scavenge -match 'Set-Alias -Name pcvenv -Value') {} else {
    {Set-Alias -Name pcvenv -Value "C:\Users\$ENV:UserName\src\pcvenv\Scripts\Activate.ps1"} >> ~\Documents\WindowsPowerShell\profile.ps1
}
Set-Alias -Name pcvenv -Value "C:\Users\$ENV:UserName\src\pcvenv\Scripts\Activate.ps1"
pcvenv
curl -L https://github.com/PhysiCell-Tools/PhysiCell-Studio/archive/refs/tags/v$(curl https://raw.githubusercontent.com/PhysiCell-Tools/PhysiCell-Studio/refs/heads/main/VERSION.txt).zip > download.zip
Expand-Archive download.zip
Remove-Item download.zip
Remove-Item -fr PhysiCell-Studio
Move-Item PhysiCell-Studio-$(curl https://raw.githubusercontent.com/PhysiCell-Tools/PhysiCell-Studio/refs/heads/main/VERSION.txt) PhysiCell-Studio

pip3.exe install -r PhysiCell-Studio\requirements.txt
Set-Location ~\src\pcvenv\bin
{python3 C:\Users\$ENV:UserName\src\PhysiCell-Studio\bin\studio.py $*} > pcstudio.exe
Set-Location ~\src
```

### &#x2728; Test the PhysiCell-Studio installation:

```powershell
Set-Location ~/src/PhysiCell
pcvenv
pcstudio
```

### &#x2728; Official PhysiCell Studio manual:

+ https://github.com/PhysiCell-Tools/Studio-Guide/tree/main



## &#x1FA9F; Advanced installation

Open a PowerShell or MSYS2 MINGW64 shell.

### &#x2728; Install PhysiCell data loader (pcdl) and iPython:

```bash
pcvenv
pip3.exe install pcdl ipython
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



## &#x1FA9F; IDE VSCode integration (optional)

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

5. Link pcvenv (the python environment we generated above):

```
View | Command Palette… | Python: Select Interpreter |
Enter interpreter path… | Find… | src/pcvenv
```

6. Link msys2 MINGW64 as default shelll:

```
View | Command Palette… | Preferences: Open Workspace Settings (JSON)
```

copy the msys2 configuration json for visual studio code (not sublime text!) found at  https://www.msys2.org/docs/ides-editors/#visual-studio-code and pasted it into the vs code settings.json .

```
close the settings.json tab # a dialog window will pop up.
click Save
Terminal| New Terminal # a msys2 shell integrated into the vs code IDE should open.
```
