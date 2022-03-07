---
title: "Installing BirdNET on a Windows Machine and running it from RStudio"
author: "Cathleen Balantic"
date: "3/7/2022"
output: html_document
---


If you're interested in birds and bioacoustics, you're probably aware of [BirdNET](https://birdnet.cornell.edu/), a bird sound recognition program developed by the [Cornell Center for Conservation Bioacoustics](https://www.birds.cornell.edu/ccb/). This is a promising free tool for processing a ton of audio data relatively quickly and understanding something about which avian species are present.

My challenge with BirdNET was actually *accessing* it. It was developed for Linux and Python users, and in my day job, I work on a Windows machine where I don't have admin rights. I'm also much more comfortable in R than I am in Python. After many failures, I have finally installed BirdNET on my Windows machine, and I'm now running it directly from RStudio. After all the frustration to install it, it was so worth the effort! I suspect BirdNET is going to be a game-changer for avian bioacoustics. I would not have been able to do this without helpful tips from a few particular colleagues. Thank you so much for your assistance, patience, and time.

 
**Disclaimer:**

*The steps below worked on a Dell running Windows 10, and I installed [BirdNET-Lite](https://github.com/kahst/BirdNET-Lite/). Your mileage may vary; software and operating systems change, documentation is hard to maintain, and depending on your workplace, you might not have the admin rights to complete all of these steps. I'm not a computer scientist. My aim is simply to share what worked for me, and start filling the gap between BirdNET and it's potential user base. There are probably vastly better ways to do all of this (virtual machine, docker container, etc.?). If you know of a better way, I encourage you to document your process and tell people about it -- let's help this field move forward.*


# Part 1. Installing BirdNET on a Windows machine

What ultimately worked for me was to activate [Windows Subsystem for Linux (WSL)](https://docs.microsoft.com/en-us/windows/wsl/about). I tried several workarounds to avoid using WSL, all resulting in headaches and time wasted. You will need admin rights or access to IT staff with rights to activate WSL on your Windows machine. 

## 1. Activate WSL
Go to [How to install Linux WSL2 on Windows 10 and Windows 11](https://www.windowscentral.com/how-install-wsl2-windows-10). This link contains the instructions I used to activate WSL a few weeks ago. With the rollout of Windows 11, it looks like the contents of the link have already changed slightly, so your process may be a bit different, but the point is just to get WSL activated. This step required an IT override on my work machine.

## 2. Install an Ubuntu Distribution
See [Manual installation steps for older versions of WSL](https://docs.microsoft.com/en-us/windows/wsl/install-manual) for more info. Since I was installing on a work machine, I did not have access to the Microsoft store, so I made sure to pay attention to the section on "Downloading distributions". Based on recommendations from others, I installed Ubuntu. I highly recommend reading through that link to get an idea of what you'll be doing. I did not need an IT override for this step.

### 2.1. Download Ubuntu
The [“Downloading distributions” section](https://docs.microsoft.com/en-us/windows/wsl/install-manual#downloading-distributions) contains options for Ubuntu, Ubuntu 20.04, Ubuntu 20.04 ARM, Ubunto 18.04, 18.04 ARM, and 16.04. I found this confusing but after some internet searching, decided on Ubuntu 20.04 and got on with my life. I clicked to manually download Ubuntu 20.04. 
