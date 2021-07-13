The purpose of this tutorial is to show how to **convert your OpenViBE data files into universally available .gdf files**.
It might seems obvious for some, but for clueless users I believe this tutorial might help a bit.

# What does it do?

Put your `data*.ov` files in a folder and this scripts convert them in `data*.gdf`

## Convert OpenVibe .ov files into gdf files

You need to donwload [OpenViBE](http://openvibe.inria.fr/downloads/) 

We will use the  `openvibe-convert.cmd` (does not require the GUI).

- Open a windows command shell (not a powershell) and reach the folder containing the data you want to convert (for example in a data directory on the desktop)

  `cd /d %USERPROFILE%\Desktop\data`
  
- find and specify the openvibe installation folder:

  `SET "PATH_OV=C:\Program Files\openvibe-3.1.0-64bit"`

- Test whether the path is recognized:

  `IF EXIST "%PATH_OV%\openvibe-convert.cmd" (ECHO converted found) ELSE (ECHO INCORRECT PATH)`

- execute the following function to convert all .ov in the data folder to .edf:

   `for /F %i in ('dir /b *.ov') do ("%PATH_OV%/openvibe-convert.cmd" %~ni.ov %~ni.edf )`
   
- execute the following function to convert all .ov in the data folder to .gdf (note: mne gdf read seems to fail when reading the event table):

   `for /F %i in ('dir /b *.ov') do ("%PATH_OV%/openvibe-convert.cmd" %~ni.ov %~ni.gdf )`
   
Ignore the contrib-pybox.dll warning message.
Your data is now converted to gdf !

# Disclaimer
I noticed an issue when loading the .gdf files into python MNE. In the annotations, the column containing the stimuli labels `desc` is shifted, **meaning ALL your stimuli positions are shifted !!!**. Waiting for a better solution I therefore shift the list of annotations backwards using this function `raw.annotations.description = np.roll(raw.annotations.description, -1)`. The last stimulation is (and was anyways) lost in the process.

# Translating stimuli

You can use those .gdf files in pretty much all EEG analysis softwares.

Tip: if your data include events, it could be difficult to intepret them from raw values, but hopefully OpenViBE provides the specification for each events [here](http://openvibe.inria.fr/stimulation-codes/)
