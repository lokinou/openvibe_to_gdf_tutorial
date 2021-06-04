The purpose of this tutorial is to show how to **convert your OpenViBE data files into universally available .edf files**.
It might seems obvious for some, but for clueless users I believe this tutorial might help a bit.

# What does it do?

Put your `data*.ov` files in a folder and this scripts convert them in `data*.edf`

## Convert OpenVibe .ov files into gdf files

You need to donwload [OpenViBE](http://openvibe.inria.fr/downloads/) 

We will use the  `openvibe-convert.cmd` (does not require the GUI).

- Open a windows command shell (not a powershell)
- Copy the path where you stored your data (here called my_data and placed on the desktop), and test whether the path is recognized

```cmd
SET "WORKDIR=%USERPROFILE%\Desktop\my_data"
if exist "%WORKDIR%\NUL" (echo directory found) else (echo INCORRECT PATH)
```

- find the openvibe installation folder and test whether the path is recognized

```cmd
SET "PATH_OV=C:\Program Files\openvibe-3.1.0-64bit" 
if exist "%PATH_OV%\openvibe-convert.cmd" (echo converted found) else (echo INCORRECT PATH)
```

- execute the following function to convert all .ov in the data folder to .edf

```cmd
cd %WORKDIR%
for /F %i in ('dir /b *.ov') do ("%PATH_OV%/openvibe-convert.cmd" %~ni.ov %~ni.gdf )
```

# Translating stimuli

You can use those .gdf files in pretty much all EEG analysis softwares.

Tip: if your data include events, it could be difficult to intepret them from raw values, but hopefully OpenViBE provides the specification for each events [here](http://openvibe.inria.fr/stimulation-codes/)