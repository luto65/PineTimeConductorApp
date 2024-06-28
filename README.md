# PineTimeConductorWatch
This repository describes the final setup that allows me to build the PineTimeConductorWatch: an InfiniTime based version of PineTime focused on the needs of Orchestra Conductors. Of all specialised wearables for Conductors that I tried, the best of them is surely the  SB Pulse. However, the price tag and the control capabilities it provided did not met my needs and therefore I decided to try out PineTime. It has not been easy, however now that I achieved the first results, it is highly rewarding and the appropriate way for me. While I recognize that not all conductors
be as skilled as I am also in programming, perhaps some will like to use what I  do for themselves too

## Contents
The *PineTimeConductorWatch* contains only Apps and features that I found useful for a Conductor. 
Purposely i removed all those features from the original distributions that I found not directly contributing to the purpose of the conductor.
Therefore in this distribution you will not find
- games
- heart measurements
- timers
- many different watchfaces
- step counter
- painting
- wheel metronome
- a tuner (since PineTime does not emit sounds)

However what you will find are
- ConMetronomeApp
  a realistically usable metronome (the one in the original distribution was too difficult for me to be of any use)
- ConWatchFace
  a watchface that I developed to facilitate creating compositions based on all church modes
- ConCircleOf5ths
  how often do we need it and do not dear to look it up on paper ..
- ConTranspositionMap
  and how often do we remeber appropriately how to explain what a Concert tone for a specific transposing instrument ?
- ConReferenceTone
  inspite of not emitting sound, i found away to generate a clear reference tone that be useful to the conductor for example when singing
  a part for an instruments
- ConOrchestralRanges
  always remember the colour of the ranges of each instrument and how best to match them ?
- ... more ideas .. let me know


## Development Workflow
First one has to develop the C++ code, and at best test it out in the simulator (called InfiniSim). 
Due to the use of SDL2 2 framework, there are several graphical Integrated Development Environments available, some free (e.g. EEZ Studio) some one the web, and some commercial. While I tried several of them, I found that the best way is simply to learn it the hard way, through the documentation and the large number of available code. Frankly speaking, I also tried the help of chatgpt in generating code. At the end, it did not help much and the code that finally now runs with my ConductorMetroApp has been entirely written by myself, whereby relying on the original MetronomeApp for the most important part ... controlling the motor that generates the vibration. Without that code, i would have not been able to get it up and running so fast.



### Build to Test
The PineTime development is *monolythic* that means that both the operating system and the applications need to be deployed and packaged in one go. Therefore the typical development cycle involes
- knowing what you want to do (e.g. the GUI, the actions attached to the elements of the GUI)
- designing what needs to be global ( .h) and what local (.cpp)
- check the API for the objects you want to use (make sure to always use the version 7.11 as that is the SDL that is suported by InfiniSim)
- build for the simulator
`
cmake -S . -B build
`
that command executed in the same location where you have installed InfiniSim will generate (if successfull) the appropriate Makefile required for the rest of the InfiniSim activities
`
cmake --build build -j4
`
this command is the one that you will execute again and again after modifying the cpp and the h of your application. It generates in the build/ folder the executable called "infinisim"
`
build/infinisim
`
is finally the command that executes the simulator and let you check if your idea was properly implemented or not.
Once you are satisfied, the way to push it to the PineTime is not that hard, if you follow the following recipe

### Build to Deploy
The  process for building to deploy takes place in the InfiniTime folder and while it does not require you writing new code,
it requires several components to be properly installed, and to choose a specific deployment strategy.
Since in my case I have a sealed PineTime, the only way to deploy the PineTimeConductorWatch is to package it as mcuboot-app-dfu.
This is easily achieved by executing the following command in the InfiniTime folder:
`
cmake -DARM_NONE_EABI_TOOLCHAIN_PATH=/home/luca/gcc-arm-none-eabi-10.3-2021.10 -DNRF5_SDK_PATH=/home/luca/nRF5_SDK_15.3.0_59ac345 -DBUILD_DFU=1 -DBUILD_RESOURCES=1 -DTARGET_DEVICE=PINETIME
`
followed by
`
cmake pinetime-mcuboot-app-dfu
`
this generates in Infinitime/src/ the file that needs then to be uploaded to the PineTime:
*pinetime-mcuboot-app-dfu-1.14.0.zip*

##Deploy to PineTime
I tried Android, i tried Linux tooling, however none of them worked due to the complexity of properly working of BT between host and virtualbox, and perhaps also due to the old version of the hardware i am using and perhaps even on the choice of Open Suse instead of
other linux distributions.
The only approach that reliably worked has been by
#### load the pinetime-mcuboot-app-dfu-1.14.0.zip on my iPhone
#### using InfiniLink 1.1 ( in Beta ) connect to PineTime, and then flash it with the above zip

and voila ... the magic is done and the PineTimeConductorWatach is up and running.


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

