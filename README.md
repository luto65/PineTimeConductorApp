# PineTimeConductorWatch
This repository describes the final setup that allows me to build the PineTimeConductorWatch: an InfiniTime based version of PineTime focused on the needs of Orchestra Conductors. Of all specialised wearables for Conductors that I tried, the best of them is surely the  SB Pulse. However, the price tag and the control capabilities it provided did not met my needs and therefore I decided to try out PineTime. It has not been easy, however now that I achieved the first results, it is highly rewarding and the appropriate way for me. While I recognize that not all conductors
be as skilled as I am also in programming, perhaps some will like to use what I  do for themselves too.

## Development Workflow
First one has to develop the C++ code, and at best test it out in the simulator (called InfiniSim). 
Due to the use of SDL2 2 framework, there are several graphical Integrated Development Environments available, some free (e.g. EEZ Studio) some one the web, and some commercial. While I tried several of them, I found that the best way is simply to learn it the hard way, through the documentation and the large number of available code. Frankly speaking, I also tried the help of chatgpt in generating code. At the end, it did not help much and the code that finally now runs with my ConductorMetroApp has been entirely written by myself, whereby relying on the original MetronomeApp for the most important part ... controlling the motor that generates the vibration. Without that code, i would have not been able to get it up and running so fast.

### Build and Test
The PineTime development is *monolythic* that means that both the operating system and the applications need to be deployed and packaged in one go. Since the packaging could be tricky, the way I develop the PineTimeConductorWatch


### Deploy





0. MacBook Pro 2013
This is my host, running macOS Big Sur (11.7.10), and with a fat SSD (1 Terabyte).

1. Purchase Pine64
For  the price of 26.99$ , and 11.99$ shipping and additional 6 € import taxes i purchased from https://pine64.com/ a sealed unit
of the famous https://pine64.org/devices/pinetime/ PineTime Watch. No need to purchase the unsealed version.

2. Install Oracle VirtualBox, and as well the extensions, so that transfer from /to the host per cp be possible

3. Download the Open Suse ISO image Tuumbleweed

4. create a new VM in VirtualBox using the freshly downloaded OpenSuse ISO Image

5. at this point starts the delicate part of setting up the development environment. This consists of several packages and their dependencies are not always straightforward to figure out. While the core development for the PineTime Time is quite simple and done in c++ just with any editor (and infact vi would suffice) the build of the image to be uploaded on the PineTime requires several Python modules, and as any developer knows ... python developers can be very creative on getting things complicated to maintain.

The foundations are:
- gcc-arm-none-eabi-10.3-2021.10
  https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads
  This contains the GNU Arm Embedded Tooolchain 10.3-2021.10 that is required to build the binary for the PineTime
  While for prototyping (using the InfiniSim) ) this is not used, when it comes to build the "real" then one needs it
  Unfortunately there is no distribution specific for Suse and one has to download it manually
  
- nRF5_SDK_15.3.0_59ac345
  Likewise the following package, that is required only when building the final image for the PineTime but not for prototyping
  with the simulator, needs to be downloaded and expanded in its own folder
  https://developer.nordicsemi.com/nRF5_SDK/nRF5_SDK_v15.x.x/nRF5_SDK_15.3.0_59ac345.zip
  
- InfiniSim
  by executing 'git clone --recursive https://github.com/InfiniTimeOrg/InfiniSim.git' one does not have only the simulator code ready to be 
  compiled, but as well the core of the operating system and all other code required to build the final image.

- SDL2
  Not required for the final build, but for doing the code development and testing with the simulator, one needs to properly
  install the whole environment for building and runnign the simulator.
  'sudo zypper install cmake libSDL2-devel gcc-c++ gcc npm libpng16-devel patch'
- Adafruit_nRF52_nrfutil
  This is reqhis is required for the final build and cannot be installed easily on Suse
- Several python modules

 

2. Install InfiniLink 1.1 on my iPhone
https://github.com/InfiniTimeOrg/InfiniLink turned out to be the only way of uploading custom built that i was able to get to work.
Uploading custom built is something that is only possible with the version 1.1. (currently in TestFlight) since version 1.0 does not allow that.
I tried Gadgetbridge on an Android, but the connectivity to the PineTime was unreliable.
Before coming to that, I've tried also all other methods mentioned in the documentation for Linux, but perhaps due to my Linux be an
Open Suse running in Virtualbox, with the host be a 12 years old Mac ... the bluetooth connectivity did not work.
Therefore the only thing that worked was InfiniLink 1.1

