---
title: "Installing BirdNET on a Windows Machine and running it from RStudio"
layout: post
output:
  html_document:
      df_print: paged
---
- [Background](#background)
- [Part 1. Installing BirdNET on a Windows machine](#part-1-installing-birdnet-on-a-windows-machine)
  * [1. Activate WSL](#1-activate-wsl)
  * [2. Install an Ubuntu Distribution](#2-install-an-ubuntu-distribution)
  * [3. Install BirdNET-Lite](#3-install-birdnet-lite)
- [Part 2. Using BirdNET-Lite from R with Reticulate](#part-2-using-birdnet-lite-from-r-with-reticulate)
  * [1. Set up a conda environment](#1-set-up-a-conda-environment)
  * [2. Set up an R script from which to interface with BirdNET](#2-set-up-an-r-script-from-which-to-interface-with-birdnet)
  * [3. Modify the BirdNET analyze script](#3-modify-the-birdnet-analyze-script)
  * [4. Run BirdNET from RStudio](#4-run-birdnet-from-rstudio)

# Background

**Note: if you're reading this from my blog, [you might want to read it from Github for better code formatting](https://github.com/cbalantic/cbalantic.github.io/blob/master/_posts/2022-03-07-Install-BirdNET-Windows-RStudio.md).**

If you're interested in birds and bioacoustics, you're probably aware of [BirdNET](https://birdnet.cornell.edu/), a bird sound recognition program developed by the [Cornell Center for Conservation Bioacoustics](https://www.birds.cornell.edu/ccb/).  There's a BirdNET app you can use directly on your phone for on-the-go bird sound identifications, but more useful to me as a bioacoustics scientist is the [BirdNET Github repository](https://github.com/kahst/BirdNET-Lite). This is a promising free tool for processing a ton of audio data relatively quickly and understanding something about which avian species are present.

My challenge with BirdNET was actually *accessing* it. It was developed for Linux and Python users. In my job, I work on a Windows machine where I don't have admin rights, and I'm much more comfortable in R than I am in Python. After many failures, I have finally installed BirdNET on my Windows machine, and I'm now running it directly from RStudio. It was so worth the effort! I suspect BirdNET is going to be a game-changer for avian bioacoustics. I would not have been able to do this without helpful tips from patient and generous colleagues. I'm hoping to pay it forward by writing down how I got this to work, in case it helps someone else. 

**Disclaimer:**

*The steps below worked on a Dell running Windows 10, and I installed [BirdNET-Lite](https://github.com/kahst/BirdNET-Lite/). Your mileage may vary; software and operating systems change, documentation is hard to maintain, and depending on your workplace, you might not have the admin rights to complete all of these steps. I'm not a computer scientist. My aim is simply to share what worked for me, and start filling the gap between BirdNET and it's potential user base. There are probably vastly better ways to do all of this (virtual machine, docker container, etc.?). If you know of a better way, I encourage you to document your process and tell people about it -- let's help this field move forward.*


# Part 1. Installing BirdNET on a Windows machine

What ultimately worked for me was to activate [Windows Subsystem for Linux (WSL)](https://docs.microsoft.com/en-us/windows/wsl/about). I tried several workarounds to avoid using WSL, all resulting in headaches and time wasted. You will need admin rights (or access to staff with admin rights) to activate WSL.

## 1. Activate WSL
Go to [How to install Linux WSL2 on Windows 10 and Windows 11](https://www.windowscentral.com/how-install-wsl2-windows-10). This link contains the instructions I used to activate WSL a few weeks ago. With the rollout of Windows 11, it looks like the contents of the link have already changed slightly, so your process may be a bit different, but the point is to get WSL activated. This step required an IT override on my work machine.

## 2. Install an Ubuntu Distribution
See [Manual installation steps for older versions of WSL](https://docs.microsoft.com/en-us/windows/wsl/install-manual) for more info. Since I was installing on a work machine, I did not have access to the Microsoft store, so I made sure to pay attention to the section on "Downloading distributions". Based on recommendations from others, I installed Ubuntu. I highly recommend reading through that link to get an idea of what you'll be doing. I did not need an IT override for this step.

### 2.1. Download Ubuntu
The [“Downloading distributions” section](https://docs.microsoft.com/en-us/windows/wsl/install-manual#downloading-distributions) contains options for Ubuntu, Ubuntu 20.04, Ubuntu 20.04 ARM, Ubunto 18.04, 18.04 ARM, and 16.04. After some internet searching, I decided on Ubuntu 20.04 and got on with my life. I later realized that [BirdNET-Lite was developed under Ubuntu 18.04](https://github.com/kahst/BirdNET-Lite#setup-ubuntu-1804). Luckily, it seems to work great with 20.04 too.


### 2.2. Add the Ubuntu app

In the Windows search bar, type "Windows Powershell" and click to open the Windows PowerShell app. Figure out where your Ubuntu app bundle has downloaded (probably the Download folder?) and run the following command in Windows PowerShell (replacing it with the name of the distribution file): 

```r
Add-AppxPackage .\replace_app_name_here.appxBundle
```

Verify that you now have the orange Ubuntu app on your machine.
 
### 2.3. Set up Linux username and password

Open an Ubuntu terminal (using that orange Ubuntu app icon) and follow [these directions](https://docs.microsoft.com/en-us/windows/wsl/setup/environment#set-up-your-linux-username-and-password) to set up your Linux username and password. Tip: When entering your password, it will *look* like nothing is being entered. You are entering the password, but the Ubuntu terminal does not echo your password for security reasons.

## 3. Install BirdNET-Lite

**Note: if you're on a Department of Interior machine, there is an additional step you may need to complete before you can successfully install and use BirdNET-Lite. Please find me on the internet and reach out if you need to know more.**

Once WSL is activated and Ubuntu is installed, you are now positioned to install BirdNET. After reading up on the [differences between BirdNET and BirdNET-Lite](https://github.com/kahst/BirdNET-Lite/issues/14), I chose to install BirdNET-Lite on the hope that would run a little faster than the original BirdNET. I'm pleased with my decision.

### 3.1. Go to the [BirdNET-Lite Github page](https://github.com/kahst/BirdNET-Lite/)

Open up an Ubuntu terminal and run the following commands. The `git` command will clone BirdNET-Lite to your machine. If you have issues here, you may need to first install git, but I already had it and was able to proceed. The `cd` command then steps you into the BirdNET-Lite folder. 

```r
git clone https://github.com/kahst/BirdNET-Lite.git
cd BirdNET-Lite
```

### 3.2. Next, follow the installation instructions at the [BirdNET-Lite Github page (Setup section)](https://github.com/kahst/BirdNET-Lite#setup-ubuntu-1804)

You'll do all of this in your Ubuntu terminal. 

### 3.3. Figure out where BirdNET-Lite actually installed on your Windows machine. 

I found the Ubuntu / WSL environment confusing to navigate. [This page gives some explanation](https://devblogs.microsoft.com/commandline/whats-new-for-wsl-in-windows-10-version-1903/#accessing-linux-files-from-windows). You can run the following command in your Ubuntu terminal to open up the folder location in Windows Explorer (don’t forget the '.' at the end!)

```r
explorer.exe .
```

Your full filepath might look something like this: 
C:\\Users\\Username\\AppData\\Local\\Packages\\CanonicalGroupLimited.UbuntuonWindows_\\LocalState\\rootfs\\home\\Your_Ubuntu\\BirdNET-Lite\\

I would avoid messing around with this folder. That said, if you can't seem to get BirdNET to run as expected, you might need to try the next step...

### 3.4. Copy your BirdNET-Lite folder out of Ubuntu and somewhere onto your Windows machine

I tried a bunch of things, but could not get BirdNET to run directly out of that long folder in my WSL Ubuntu setup. After I ran out of ideas, I simply copied my BirdNET-Lite folder locally onto my machine to see if it would work from there, and lo and behold, it did. For all of the following steps, I am interacting with BirdNET directly from my locally copied folder as a Windows user. 

### 3.5. Test out BirdNET and verify that it works.
Open up your Ubuntu terminal and experiment with running the example commands provided by the [BirdNET-Lite usage documentation](https://github.com/kahst/BirdNET-Lite#usage). First, make sure to `cd` into the folder where you have copied BirdNET. Use quotes around your file path if it contains spaces.

```r
cd "/mnt/c/users/username/path/to/YourBirdNETCopiedFolder"
python3 analyze.py --i 'example/XC558716 - Soundscape.mp3' --lat 35.4244 --lon -120.7463 --week 18
```


### 3.6. Make any necessary Python dependency adjustments

If you can't get BirdNET to work, it may be due to a package dependency issue. I specifically ran into an issue with numpy versioning. Error messages indicated that numpy 1.22.1 was necessary, so I went back into my Ubuntu terminal and installed it with the following command: 

```r
sudo pip3 install NumPy==1.22.1
```

Once I had the right version of numpy installed, I opened up the `analyze.py` file in my copied BirdNET-Lite folder and made the following modifications. Essentially, you're going to add a few lines of code before the “import numpy as np” line that will allow you to specify numpy 1.22.1: 

```python
# Import correct version of numpy for BirdNET
import pkg_resources
pkg_resources.require("numpy==1.22.1")  
import numpy as np
```

Once you've saved those modifications in your `analyze.py` file, return to your Ubuntu terminal and try step 3.5 again. Check your copied version of the BirdNET-Lite folder and make sure that `results.csv` is showing up as recently modified. If so, BirdNET has been successfully installed on your machine, and you’re ready to move on to using it from R! I would definitely suggest continuing to experiment with it from Ubuntu first -- play with analyzing your own files, get a grasp on modifying the input arguments, etc. 




# Part 2. Using BirdNET-Lite from R with Reticulate

**This section assumes you already have BirdNET-Lite installed.**

I was pleased that I could run BirdNET from the Ubuntu terminal, but I wanted a lot more control over automating and modifying my BirdNET workflows. For me, ultimately, this meant using RStudio. Luckily, the [Reticulate R package](https://rstudio.github.io/reticulate/) enables us to interface with Python through R.

First, you’ll set up a conda environment in R. Next, you’ll set up an R script that allows you to process audio through BirdNET. Finally, you’ll make a few minor modifications to your `analyze.py` file.


## 1. Set up a conda environment

As an R user, one of the frustrating things about Python is the setup and package management. In R, it feels like I can just install RStudio, install whatever packages I need, and sail into the sunset. Python package dependency management feels a lot more complex and rife with pitfalls; one of the workarounds for dealing with that is to use conda environments. [As explained here](https://towardsdatascience.com/a-guide-to-conda-environments-bc6180fc533), conda environments help manage and isolate different package dependencies for specific projects. They're similar to virtual environments. In fact, I originally tried using a Python virtual environment from RStudio, but couldn't get R/Reticulate to work with it. I tried a conda environment instead, and that worked. I found [this guide](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-with-commands) helpful for understanding more about setting up and managing conda environments. 

### 1.1. You might want to install Python... again. This time, Anaconda. 

After many false starts with Reticulate/RStudio, I gave up on getting Reticulate to recognize my previously installed Python versions and ended up manually installing the latest version of [Anaconda](https://www.anaconda.com/). I sensed that my problems stemmed from the chaotic nature of my previous Python package installs, and hoped that Anaconda's promise of simplified package management would enable easier access from Reticulate. I was right!

### 1.2. Set up your BirdNET conda environment. 

First, open up an Anaconda prompt. In the Windows search bar, type and click “Anaconda Prompt (Anaconda3)”. I had a few versions of Anaconda prompts from various Python installs over the past year, but this was the one that worked correctly.

In the Anaconda prompt, run the following commands to reinstall all the necessary packages you’ll need to run BirdNET within your conda environment. Below, we are naming our conda environment “pybirdnet”, but you can name it whatever you want:

```python
conda create --name pybirdnet
conda install -n pybirdnet tensorflow                
conda install -n pybirdnet -c conda-forge librosa 
conda install -n pybirdnet ffmpeg                     
conda install -n pybirdnet -c conda-forge numpy=1.22.1  
```

Make sure you know where this pybirdnet environment was created. For me, this lives in C:\\Users\\Username\\Anaconda3\\envs\\pybirdnet

## 2. Set up an R script from which to interface with BirdNET

**Updated 4/15/2022: I'm keeping all of this below for posterity, but note that once you have installed BirdNET-Lite and set up a conda environment, now you can use the function [birdnet_run](https://github.com/nationalparkservice/NSNSDAcoustics#running-birdnet-from-rstudio-with-birdnet_run) from the package [NSNSDAcoustics](https://github.com/nationalparkservice/NSNSDAcoustics) instead of proceeding with steps 2, 3, and 4.**

Here is the general script outline I use. The R script below assumes you have a folder full of wave files from a single monitoring location that you want to analyze. It also assumes that your wave file names have the naming convention SITEID_YYYYMMDD_HHMMSS.wav. If this is not the case, you'll need to do some tinkering.

Copy and save this script to your machine and make necessary adjustments that will equip you to experiment with it (files, filepaths, lat longs, etc.). But don't actually run it until you've done step 3. 

```r

# install.packages(c('data.table', 'lubridate'))
library(data.table)
library(lubridate)

# Setenv to your pybirdnet conda environment (below is an example filepath)
# Note that you need to run this line BEFORE calling in reticulate library
Sys.setenv(RETICULATE_PYTHON = "C:/Users/Username/Anaconda3/envs/pybirdnet/python.exe")

# Call in reticulate AFTER running Sys.setenv()
library(reticulate)

# Verify that you're using the python and numpy versions you expect to use 
py_config()  

# Point Reticulate to your conda environment
use_condaenv(condaenv = "pybirdnet", required = TRUE)

# use_python is another way to tell Reticulate which Python to use
# I couldn't get this line to work; hence the previous Sys.setenv() command
use_python('C:/Users/Username/Anaconda3/envs/pybirdnet/python.exe', 
           required = TRUE)

# Below, I assume:
#  * you have a folder of wave files from a single monitoring location
#  * file names follow the naming convention SITEID_YYYYMMDD_HHMMSS.wav

# Identify file path string names
audio.fp <- 'Path:/To/Folder/Of/Audio/'
recIDs <- list.files(audio.fp, pattern = '.wav')
i.strings <- paste0(audio.fp, recIDs)

# Declare lat longs for your monitoring location
lat <- 42.4535
lon <- -76.4735

# Identify recording weeks for each file
wk <- week(as.Date(unlist(lapply(strsplit(x = recIDs, split = '_'), '[[', 2)),
                   format = '%Y%m%d')) 

# Create path names for storing output CSVs from BirdNET:
result.fp <- paste0('Path:/To/Folder/For/Storing/BirdNET-Results/BirdNET_', 
                    unlist(lapply(
                        strsplit(x = recIDs, split = '.', fixed = TRUE), 
                        '[[', 1)), '.csv')

# Customize any other params as desired:
# (currently set these to BirdNET defaults, but thinking ahead to R function)
ovlp <- 0.0
sens <- 1.0
minconf <- 0.1
customlist <- NULL

# Loop through audio folder and process each wave using BirdNET: 
for (i in 1:length(recIDs)) {
  cat(paste0('Working on ', i, ' of ', length(recIDs), ': ', recIDs[i], '\n'))
  setwd('C:/WhateverYouNamedYourLocalBirdNETFolder/')
  py_run_string(paste0("args_i = '", i.strings[i], "'"))
  py_run_string(paste0("args_lat = ", lat))
  py_run_string(paste0("args_lon = ", lon))
  py_run_string(paste0("args_week = ", wk[i]))
  py_run_string(paste0("args_overlap = ", ovlp))
  py_run_string(paste0("args_sensitivity = ", sens))
  py_run_string(paste0("args_min_conf = ", minconf))
  py_run_string(paste0("args_o = '", result.fp[i], "'"))
  
  # Source in your modified version of the analyze.py file
  source_python('reticulate-analyze.py')
}

# Check on a result!
(check <- data.table(read.csv(file = result.fp[i], header = TRUE, sep = ';')))
```


### 3. Modify the BirdNET analyze script 

**Updated 4/15/2022: I'm keeping all of this below for posterity, but note that once you have installed BirdNET-Lite and set up a conda environment, now you can use the function [birdnet_run](https://github.com/nationalparkservice/NSNSDAcoustics#running-birdnet-from-rstudio-with-birdnet_run) from the package [NSNSDAcoustics](https://github.com/nationalparkservice/NSNSDAcoustics) instead of proceeding with steps 2, 3, and 4. The modified BirdNET 'analyze.py' script described in this section can be installed directly with NSNSDAcoustics. It is located [here](https://github.com/nationalparkservice/NSNSDAcoustics/blob/main/inst/reticulate-analyze.py).**

Before actually running that R script, we need to make a few modifications to BirdNET's `analyze.py` script. The `analyze.py` script imports a Python package called `argparse`. This package is used toward the end of `analyze.py` in several lines that say `parser.add_argument()` in which the user can input `i`, `o`, `lat`, `lon`, `week`, etc. into the Ubuntu terminal. `argparse` is meant specifically for use through the command line. 

We won't be using the command line to process audio files from RStudio, so we’re going to comment out (or delete) any references to this `parser.add_argument()` code. Additionally, we will modify references to the `analyze.py` argument names and feed in our BirdNET arguments through the R script using Reticulate's `py_run_string()` function. 

There might be more elegant ways to do all of this, but I’m not adept enough with Python to know what they are.

**Note that I was also having a minor issue with the option to load a custom species list, so I opted to comment this out of the analyze.py file for now. I assume it's an easy fix, but I don't currently need it.**

The script below makes all of the changes I've outlined. Save this script in your BirdNET-Lite folder and give it a name like `reticulate-analyze.py` so that you don't overwrite your original `analyze.py` file. 

```python

import os
os.environ['TF_CPP_MIN_LOG_LEVEL'] = '3'
os.environ['CUDA_VISIBLE_DEVICES'] = ''


try:
    import tflite_runtime.interpreter as tflite
except:
    from tensorflow import lite as tflite

import argparse
import operator
import librosa
import math
import time

### CODE ADDED TO DEAL WITH DEPENDENCY ISSUES ###
# Import correct version of numpy for BirdNET
import pkg_resources
pkg_resources.require("numpy==1.22.1")  
import numpy as np

def loadModel():

    global INPUT_LAYER_INDEX
    global OUTPUT_LAYER_INDEX
    global MDATA_INPUT_INDEX
    global CLASSES

    print('LOADING TF LITE MODEL...', end=' ')

    # Load TFLite model and allocate tensors.
    interpreter = tflite.Interpreter(model_path='model/BirdNET_6K_GLOBAL_MODEL.tflite')
    interpreter.allocate_tensors()

    # Get input and output tensors.
    input_details = interpreter.get_input_details()
    output_details = interpreter.get_output_details()

    # Get input tensor index
    INPUT_LAYER_INDEX = input_details[0]['index']
    MDATA_INPUT_INDEX = input_details[1]['index']
    OUTPUT_LAYER_INDEX = output_details[0]['index']

    # Load labels
    CLASSES = []
    with open('model/labels.txt', 'r') as lfile:
        for line in lfile.readlines():
            CLASSES.append(line.replace('\n', ''))

    print('DONE!')

    return interpreter

def loadCustomSpeciesList(path):

    slist = []
    if os.path.isfile(path):
        with open(path, 'r') as csfile:
            for line in csfile.readlines():
                slist.append(line.replace('\r', '').replace('\n', ''))

    return slist

def splitSignal(sig, rate, overlap, seconds=3.0, minlen=1.5):

    # Split signal with overlap
    sig_splits = []
    for i in range(0, len(sig), int((seconds - overlap) * rate)):
        split = sig[i:i + int(seconds * rate)]

        # End of signal?
        if len(split) < int(minlen * rate):
            break
        
        # Signal chunk too short? Fill with zeros.
        if len(split) < int(rate * seconds):
            temp = np.zeros((int(rate * seconds)))
            temp[:len(split)] = split
            split = temp
        
        sig_splits.append(split)

    return sig_splits

def readAudioData(path, overlap, sample_rate=48000):

    print('READING AUDIO DATA...', end=' ', flush=True)

    # Open file with librosa (uses ffmpeg or libav)
    sig, rate = librosa.load(path, sr=sample_rate, mono=True, res_type='kaiser_fast')

    # Split audio into 3-second chunks
    chunks = splitSignal(sig, rate, overlap)

    print('DONE! READ', str(len(chunks)), 'CHUNKS.')

    return chunks

def convertMetadata(m):

    # Convert week to cosine
    if m[2] >= 1 and m[2] <= 48:
        m[2] = math.cos(math.radians(m[2] * 7.5)) + 1 
    else:
        m[2] = -1

    # Add binary mask
    mask = np.ones((3,))
    if m[0] == -1 or m[1] == -1:
        mask = np.zeros((3,))
    if m[2] == -1:
        mask[2] = 0.0

    return np.concatenate([m, mask])

def custom_sigmoid(x, sensitivity=1.0):
    return 1 / (1.0 + np.exp(-sensitivity * x))

def predict(sample, interpreter, sensitivity):

    # Make a prediction
    interpreter.set_tensor(INPUT_LAYER_INDEX, np.array(sample[0], dtype='float32'))
    interpreter.set_tensor(MDATA_INPUT_INDEX, np.array(sample[1], dtype='float32'))
    interpreter.invoke()
    prediction = interpreter.get_tensor(OUTPUT_LAYER_INDEX)[0]

    # Apply custom sigmoid
    p_sigmoid = custom_sigmoid(prediction, sensitivity)

    # Get label and scores for pooled predictions
    p_labels = dict(zip(CLASSES, p_sigmoid))

    # Sort by score
    p_sorted = sorted(p_labels.items(), key=operator.itemgetter(1), reverse=True)

    # Remove species that are on blacklist
    for i in range(min(10, len(p_sorted))):
        if p_sorted[i][0] in ['Human_Human', 'Non-bird_Non-bird', 'Noise_Noise']:
            p_sorted[i] = (p_sorted[i][0], 0.0)

    # Only return first the top ten results
    return p_sorted[:10]

def analyzeAudioData(chunks, lat, lon, week, sensitivity, overlap, interpreter):

    detections = {}
    start = time.time()
    print('ANALYZING AUDIO...', end=' ', flush=True)

    # Convert and prepare metadata
    mdata = convertMetadata(np.array([lat, lon, week]))
    mdata = np.expand_dims(mdata, 0)

    # Parse every chunk
    pred_start = 0.0
    for c in chunks:

        # Prepare as input signal
        sig = np.expand_dims(c, 0)

        # Make prediction
        p = predict([sig, mdata], interpreter, sensitivity)

        # Save result and timestamp
        pred_end = pred_start + 3.0
        detections[str(pred_start) + ';' + str(pred_end)] = p
        pred_start = pred_end - overlap

    print('DONE! Time', int((time.time() - start) * 10) / 10.0, 'SECONDS')

    return detections

def writeResultsToFile(detections, min_conf, path):

    print('WRITING RESULTS TO', path, '...', end=' ')
    rcnt = 0
    with open(path, 'w') as rfile:
        rfile.write('Start (s);End (s);Scientific name;Common name;Confidence\n')
        for d in detections:
            for entry in detections[d]:
                if entry[1] >= min_conf and (entry[0] in WHITE_LIST or len(WHITE_LIST) == 0):
                    rfile.write(d + ';' + entry[0].replace('_', ';') + ';' + str(entry[1]) + '\n')
                    rcnt += 1
    print('DONE! WROTE', rcnt, 'RESULTS.')

def main():

    global WHITE_LIST


    # CB: Comment out parser because not using command line in reticulate.
    #   ==> In R, we will input args as args_[name] in py_run_string(), e.g. py_run_string("args_lon = 135.8911")
    # Parse passed arguments
    # parser = argparse.ArgumentParser()
    # parser.add_argument('--i', help='Path to input file.')
    # parser.add_argument('--o', default='result.csv', help='Path to output file. Defaults to result.csv.')
    # parser.add_argument('--lat', type=float, default=-1, help='Recording location latitude. Set -1 to ignore.')
    # parser.add_argument('--lon', type=float, default=-1, help='Recording location longitude. Set -1 to ignore.')
    # parser.add_argument('--week', type=int, default=-1, help='Week of the year when the recording was made. Values in [1, 48] (4 weeks per month). Set -1 to ignore.')
    # parser.add_argument('--overlap', type=float, default=0.0, help='Overlap in seconds between extracted spectrograms. Values in [0.0, 2.9]. Defaults tp 0.0.')
    # parser.add_argument('--sensitivity', type=float, default=1.0, help='Detection sensitivity; Higher values result in higher sensitivity. Values in [0.5, 1.5]. Defaults to 1.0.')
    # parser.add_argument('--min_conf', type=float, default=0.1, help='Minimum confidence threshold. Values in [0.01, 0.99]. Defaults to 0.1.')   
    # parser.add_argument('--custom_list', default='', help='Path to text file containing a list of species. Not used if not provided.')

    # args = parser.parse_args()

    # Load model
    interpreter = loadModel()

    # CB Note: I couldn't get custom_list to work (and don't need to use it at this point)
    # So I commented this part out: 
    # Load custom species list
    # if not args_custom_list == '':
    #    WHITE_LIST = loadCustomSpeciesList(args_custom_list)
    # else:
    WHITE_LIST = []


    # Read audio data
    audioData = readAudioData(args_i, args_overlap)

    # Process audio data and get detections
    week = max(1, min(args_week, 48))
    sensitivity = max(0.5, min(1.0 - (args_sensitivity - 1.0), 1.5))
    detections = analyzeAudioData(audioData, args_lat, args_lon, week, sensitivity, args_overlap, interpreter)

    # Write detections to output file
    min_conf = max(0.01, min(args_min_conf, 0.99))
    writeResultsToFile(detections, min_conf, args_o)

if __name__ == '__main__':

    main()


```

### 4. Run BirdNET from RStudio

Once you've saved your `reticulate-analyze.py` file in your BirdNET-Lite folder, head back to the R script above and modify it to point to your files of interest. Hopefully, you'll soon be celebrating that you can run BirdNET from RStudio.

...and that's all I've got! Thank you to everybody who helped me figure out various components of how to do this (the install process took a village). Huge thank you to the BirdNET developers and everybody at Cornell Center for Conservation Bioacoustics for creating this tool and making it free. 

*Any views or opinions expressed on this website are mine alone, and do not represent endorsement by any affiliated institutions, employers, collaborators, or any U.S. federal agencies. This blog post merely shares what worked for me, and is not meant to serve as official guidance.*
