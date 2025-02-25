# Setup PhysiCell on Windows

## &#x1FA9F; Essential installation

### Python environment dedicated for PhysiCell

We will generate a python3 environment with the default Windows python installation, where we will install all PhysiCell modelling related python libraries. We will name this Python3 environment pcpyenv (PhysiCell Python environment), and we install it in the src folder where just before have installed PhysiCell.
Here we generate the environment with the regular python. If you run mamba or conda, please adjust the commands accordingly.

Open the Windows PowerShell terminal!
The first command will let you know if you have python installed. If not, then please go to the Microsoft Store and install the latest release available from the Python Software Foundation.

```powershell
Get-Command python.exe
```


```powershell
Set-Location ~
python.exe -m venv src/pcpyenv
Set-Alias -Name pcpyenv -Value C:\Users\$USER\src\pcpyenv\Scripts\activate"
```

+ Activate the pcpyenv python environment using the alias generated before:

```powershell
pcpyenv
```

### PhysiCell Studio

+ Download PhysiCell-Studio and install it in the ~/src folder.

```powershell
Set-Location ~\src
git clone https://github.com/PhysiCell-Tools/PhysiCell-Studio.git
pip3.exe install -r PhysiCell-Studio\requirements.txt
Set-Location ~\src\pcpyenv\bin
echo "python3 C:\Users\$USER\src\PhysiCell-Studio\bin\studio.py $*" > pcstudio.exe
Set-Location ~
```

## &#x1FA9F; Advanced installation



