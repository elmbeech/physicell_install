# Setup PhysiCell on Windows

We will install everything PhysiCell related under the src folder that we will place in your Windows Home directory.
If you prefer another folder name, please adjust the commands accordingly.


## &#x1FA9F; Operating system dependencies

### &#x2728; Download and install MSYS2 x86\_64:

+ https://www.msys2.org/

### &#x2728; Install GCC, ImageMagick, Unzip, Zip, and ca-certificates:

In the Windows Taskbar Search box, type msys2.
Open the MSYS2 MINGW64 shell ~ the one with the blue MSYS2 icon, no other color!

```bash
pacman -S mingw-w64-x86_64-gcc make mingw-w64-x86_64-imagemagick mingw-w64-x86_64-ffmpeg unzip zip mingw-w64-x86_64-ca-certificates
```


## &#x1FA9F; Basic PhyiCell installation

Important: before you run the basic physicell installation, you have to install the operating system dependencies!

### &#x2728; Install PhysiCell:

Open the MSYS2 MINGW64 shell ~ the one with the blue MSYS2 icon, no other color!
Copy paste (mouse right click) the code below into the shell and press the enter key.

```bash
install='Y'
uart='None'
if [ -d /c/Users/$USER/src/PhysiCell ]
then
    echo "WARNING : /c/Users/$USER/src/PhysiCell already exists! do you wanna re-install? data will be lost! [Y,N]"
    read uart
else
    uart='Y'
fi
if [ $install == $uart ]
then
    mkdir -p /c/Users/$USER/src
    cd /c/Users/$USER/src
    curl -L https://github.com/MathCancer/PhysiCell/archive/refs/tags/$(curl https://raw.githubusercontent.com/MathCancer/PhysiCell/master/VERSION.txt).zip > download.zip
    unzip download.zip
    rm download.zip
    rm -fr PhysiCell
    mv PhysiCell-$(curl https://raw.githubusercontent.com/MathCancer/PhysiCell/master/VERSION.txt) PhysiCell
else
    echo 'installation terminated.'
fi
```

### &#x2728; Test the PhysiCell installation with the template sample project:

Run this code line by line.

```bash
cd /c/Users/$USER/src/PhysiCell
```
```bash
make data-cleanup clean reset
```
```bash
make template
```
```bash
make -j8
```
```bash
./project.exe
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


### &#x2728; Optional: make a shortcut from your MSYS2 home to your Windows home folder.

Run as Administrator a PowerShell (v7.x) or WindowsPowerShell (v5.1) ~ the regular one, not the ISE and not the x86 one!

Note that this time you have to run the shell as an administrator.
Note that because of that, you have to type out your username and cannot use the $USER variable.
Run this code line by line.

```powershell
Set-Location C:\msys64\home\<YOUR_USER_NAME>
```
```powershell
New-Item -ItemType SymbolicLink -Path C:\msys64\home\<YOUR_USER_NAME>\shortcut -Target C:\Users\<YOUR_USER_NAME>
```

Now, when you have opened a MSYS2 MINGW64 shell (e.g., to run PhysiCell), to change to your Windows home directory, you can simply do:
```bash
cd ~
cd shortcut
```


## &#x1FA9F; Essential installation

We will generate a Python3 environment with the default Python installation, where we will install all PhysiCell modeling related Python libraries.
We will name this Python3 environment pcvenv (PhysiCell virtual environment).

### &#x2728; Install Python:

If you have not already installed Python, please go to https://www.python.org/downloads/ and **install** the latest **Python** release.

### &#x2728; Get the Windows PowerShell ready:

Open a PowerShell (v7.x) or WindowsPowerShell (v5.1) ~ the regular one, not the ISE and not the x86 one!

If you wonder, why there is more than one PowerShell flavor, read this article. Our script will run on both flavors.
+ https://learn.microsoft.com/en-us/powershell/scripting/whats-new/differences-from-windows-powershell

To activate Python environments, we have to be able to run PowerShell scripts.
This is why we have to change the execution policy.
Please run the command below and confirm with Y.

```powershell
Set-ExecutionPolicy Unrestricted -Scope CurrentUser
```

### &#x2728; Install PhysiCell-Studio:

Copy past (use ctrl + v for paste and not the mouse menue) the code below into the shell and press the enter key to generate the pcvenev virtual Python environment.

```powershell
$install = 'Y'
$uart = 'None'
if (Test-Path ~\src\pcvenv) {
    $uart = Read-Host "WARNING : C:\Users\$ENV:UserName\src\pcvenv already exists! do you wanna re-install the pcvenv Python environment? data will be lost, and PhysiCell-Studio has to be re-installed too! [Y,N]"
} else {
    $uart = 'Y'
}
if ($install -eq $uart) {
    if (Test-Path ~\src) {} else { New-Item ~\src -Type Directory }
    Set-Location ~\src
    python.exe -m venv pcvenv
    if (!(Test-Path -Path $PROFILE)) { New-Item -ItemType File -Path $PROFILE -Force }
    $scavenge = Get-Content $PROFILE
    if ($scavenge -match 'Set-Alias -Name pcvenv -Value') {} else {
        Add-Content -Path $PROFILE -Value "Set-Alias -Name pcvenv -Value ""C:\Users\$ENV:UserName\src\pcvenv\Scripts\Activate.ps1"""
    }
    if (Test-Path C:\msys64\home\$ENV:UserName\.bash_profile) {} else { New-Item ~\.bash_profile }
    $scavenge = Get-Content C:\msys64\home\$ENV:UserName\.bash_profile
    if ($scavenge -match 'alias pcvenv=') {} else {
        Add-Content -Path  C:\msys64\home\$ENV:UserName\.bash_profile -Value "alias pcvenv=""source /c/Users/$ENV:UserName/src/pcvenv/Scripts/activate"""
    }
    'pcvenv virtual Python environment generated.'
} else {
    'installation terminated.'
}
```

Open a fresh PowerShell (v7.x) or WindowsPowerShell (v5.1) ~ the regular one, not the ISE and not the x86 one!
Copy past (use ctrl + v for paste and not the mouse menue) the code below into the shell and press the enter key to install PhysiCell-Studio into the pcvenv virtual Python environment.

```powershell
if (Test-Path ~\src\pcvenv) {
    $install = 'Y'
    $uart = 'None'
    if (Test-Path ~\src\PhysiCell-Studio) {
        $uart = Read-Host "WARNING : C:\Users\$ENV:UserName\src\PhysiCell-Studio already exists! do you wanna re-install PhysiCell-Studio? data will be lost! [Y,N]"
    } else {
        $uart = 'Y'
    }
    if ($install -eq $uart) {
        pcvenv
        curl.exe -L https://github.com/PhysiCell-Tools/PhysiCell-Studio/archive/refs/tags/v$(curl.exe https://raw.githubusercontent.com/PhysiCell-Tools/PhysiCell-Studio/refs/heads/main/VERSION.txt).zip --output download.zip
        Expand-Archive -Path download.zip -DestinationPath .
        Remove-Item download.zip
        if (Test-Path PhysiCell-Studio) { Remove-Item PhysiCell-Studio -Recurse }
        Move-Item PhysiCell-Studio-$(curl.exe https://raw.githubusercontent.com/PhysiCell-Tools/PhysiCell-Studio/refs/heads/main/VERSION.txt) PhysiCell-Studio
        pip3.exe install -r PhysiCell-Studio\requirements.txt
        Set-Location ~\src\pcvenv\Scripts
        Set-Content -Path pcstudio.ps1 -Value "python.exe C:\Users\$ENV:UserName\src\PhysiCell-Studio\bin\studio.py $args"
        Set-Content -Path pcstudio.exe -Value "python.exe /c/Users/$ENV:UserName/src/PhysiCell-Studio/bin/studio.py $*"
        Set-Location ~\src
    } else {
        'installation terminated.'
    }
} else {
    'ERROR : pcvenv virtual Python environment could not be detected. please run the code cell above to generate the pcvenev virtual Python environment!'
}
```

### &#x2728; Run PhysiCell-Studio in PowerShell:

Open a fresh PowerShell (v7.x) or WindowsPowerShell (v5.1) ~ the regular one, not the ISE and not the x86 one!
Run this code line by line.

Note the pcstudio **ps1** extension.

```powershell
Set-Location ~\src\PhysiCell
```
```powershell
pcvenv
```
```powershell
pcstudio.ps1
```

### &#x2728; Run PhysiCell-Studio in the MYSYS2 shell:

Open the MSYS2 MINGW64 shell ~ the one with the blue MSYS2 icon, no other color!
Run this code line by line.

Note the pcstudio **exe** extension.

```bash
cd  /c/Users/$USER/src/PhysiCell
```
```bash
pcvenv
```
```bash
pcstudio.exe
```

### &#x2728; Official PhysiCell Studio manual:

+ https://github.com/PhysiCell-Tools/Studio-Guide/tree/main


## &#x1FA9F; Advanced installation

### &#x2728; Install Python:

If you have not already installed Python, please go to https://www.python.org/downloads/ and **install** the latest **Python** source release.

### &#x2728; Install PhysiCell Data Loader (pcdl) and iPython:

Open a PowerShell or MSYS2 MINGW64 shell.
Run this code line by line.

```bash
pcvenv
```
```bash
pip3.exe install pcdl ipython
```

### &#x2728; Test the pcdl installation:

Fire up a python shell.

```bash
ipython
```

Inside the python shell write:

```python
import pcdl
print(pcdl.__version__)
exit()
```

### &#x2728; Official pcdl manual:

+ https://github.com/PhysiCell-Tools/python-loader


## &#x1FA9F; IDE VSCode integration (optional)

1. Install vs code, either from your operating system’s app store or from https://code.visualstudio.com/ .

2. Generate a vs code profile for physicell:

```
File | New Window with Profile > New Profile …
Name: physicell
Icon: choose something cool.
Create
Add Folder: C:\Users\<username>\src
click the profile icon (default is a gearwheel) on the left side bottom corner.
Profile > physicell
```

3. Open the Folder:

```
File | Open Folder… | C:\Users\<username>\src | Add Folder
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
View | Command Palette… | Python: Select Interpreter | Enter interpreter path… | Find… | C:\Users\<username>\src\pcvenv\Scripts\python.exe
```

6. Link msys2 MINGW64 shell:

```
View | Command Palette… | Preferences: Open User Settings (JSON)
```

copy the msys2 configuration json (https://www.json.org/json-en.html) for visual studio code (not sublime text!) found at  https://www.msys2.org/docs/ides-editors/#visual-studio-code and past it into the vs code settings.json .

```
IMPORTANT:
replace the MSYS2 UCRT name with MSYS2 MINGW64.
replace the -ucrt64 parameter with -mingw64.
close the settings.json tab # a dialog window will pop up.
click Save
```

Now we make msys2 MINGW64 the default shell for the physicell profile.

```
View | Command Palette… | Terminal: Select Default Profile
Choose MSYS2 MINGW64 (with the cmd.exe /c "C:\msys64\msys2_shell.cmd -defterm -here -no-start -mingw64" argument).
```

And open a default shell.

```
Terminal| New Terminal # a msys2 shell integrated into the vs code IDE should open.
```
