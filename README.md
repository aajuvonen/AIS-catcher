# AIS-catcher - A multi-platform AIS receiver
This package will add the ```AIS-catcher``` command - a dual channel AIS receiver for RTL-SDR dongles, Airspy Mini, Airspy R2, Airspy HF+, HackRF, SDRplay (RSP1, RSP1A and RSPDX for now), input from file and over ZMQ and (RTL) TCP. Output is send in the form of NMEA messages to either screen or broadcasted over UDP. 
The program provides the option to read and decode the raw discriminator output of a VHF receiver as well. 

![Image](https://raw.githubusercontent.com/jvde-github/AIS-catcher/media/media/containership.jpg)

## About this fork

The purpose of this fork is to enable reception using 432-438 MHz ISM band. The software is forked to specifically tune to a 433.800 MHz carrier, to which shifted AIS channels A and B are adjacent by an offset of 25 kHz above and below, respectively.

## Purpose

The aim of ```AIS-catcher``` is to provide a platform to facilitate continuous improvement of receiver models. Any suggestions, observation or sharing of recordings for setups where the current models are struggling is highly appreciated! The algorithm behind the default receiver model was created by investigating signals and trying different ways to get a coherent model running whilst keeping it simple at the same time. If I have some more free time I will try to expand the documentation and implement some improvement ideas.

### Disclaimer

```AIS-catcher```  is created for research and educational purposes under the MIT license. It is a hobby project and not tested and designed for reliability and correctness. 
You can play with the software but it is the user's responsibility to use it prudently. So,  DO NOT rely upon this software in any way including for navigation 
and/or safety of life or property purposes.
There are variations in the legislation concerning radio reception in the different administrations around the world. 
It is your responsibility to determine whether or not your local administration permits the reception and handling of AIS messages from ships. 
It is specifically forbidden to use this software for any illegal purpose whatsoever. 
This is hobby and research software for use only in those regions where such use is permitted.

## Installation and Windows Binary

Building instructions are provided below for many systems.

A Windows binary version of **v0.35** (ex SDRplay support) is available from my Google Drive (see below table). If you did not access these files before I might have to give you access. Furthermore, note that for the RTL-SDR you will have to install drivers using Zadig (https://www.rtl-sdr.com/tag/zadig/). After that, simply unpack the ZIP file in one directory and start the executable on the command line with the required parameters. 

Recent releases:
 | Version | Win32  | x64 | x64 SDRplay-only |
 | :--- | :--- | :---: |   :---: | 
   |Latest|  | [ZIP](https://drive.google.com/file/d/1-lBCfFejeZEl1-wXi_-_S6nBMNqIeQjA/view?usp=sharing) | |
   |v0.35| [ZIP](https://drive.google.com/file/d/1fvH7ez_jUx6d_pmMcuX0kMgUU92Wwizw/view?usp=sharing) | [ZIP](https://drive.google.com/file/d/1DDjjbNBlQdgaxfJiL-tjLubSGFtn2THI/view?usp=sharing) | |
  |v0.34| [ZIP](https://drive.google.com/file/d/1ivz0Pk1KsGfnq5k0E723nXUGfz79ya-d/view?usp=sharing) | [ZIP](https://drive.google.com/file/d/1yfjEnY9fqS6ifmqaatl3EzISdhliIk-j/view?usp=sharing) | |
 |v0.33 |  [ZIP](https://drive.google.com/file/d/1KFvvWQi47QquOl-jDPRK8mpmUnfQaM91/view?usp=sharing) | [ZIP](https://drive.google.com/file/d/1oE0rTMU7DF9pFzw2Pt1SAMDm-UWILJrT/view?usp=sharing)  | |
 
If you are looking for a Windows binary supporting SDRplay API 3.09 for RSP1/RSP1A/RSPDX, please get in contact with [me](mailto:jvde.github@gmail.com). Pre-built container images containing AIS-catcher are available from the GitHub Container Registry.

## Recent Developments

For testing, do not use the development version (edge) but instead download the latest release. The development version might not work. 

Edge: added new switch for RTL-SDR ```-gr BW``` which unlocks the bandwidth functionality on some RTL dongles. Early experimentation did not show improved reception with this setting. 

Version **0.35**: smaller fixes and improvements and unlocking support for SDRPlay RSP1 and RSPDX. For details see [Releases](https://github.com/jvde-github/AIS-catcher/releases).

## Usage
````
use: AIS-catcher [options]

	[-h display this message and terminate (default: false)]
	[-s xxx sample rate in Hz (default: based on SDR device)]
	[-v [option: 1+] enable verbose mode, optional to provide update frequency in seconds (default: false)]
	[-q suppress NMEA messages to screen (default: false)]
	[-n show NMEA messages on screen without detail]
	[-u xxx.xx.xx.xx yyy - UDP destination address and port (default: off)]

	[-r [optional: yy] filename - read IQ data from file, short for -r -ga FORMAT yy FILE filename, for stdin input use filename equals stdin or .]
	[-w filename - read IQ data from WAV file, short for -w -gw FILE filename]
	[-t [host [port]] - read IQ data from remote RTL-TCP instance]
	[-z [optional [format]] [optional endpoint] - read IQ data from [endpoint] in [format] via ZMQ (default: format is CU8)]

	[-l list available devices and terminate (default: off)]
	[-L list supported SDR hardware and terminate (default: off)]
	[-d:x select device based on index (default: 0)]
	[-d xxxx select device based on serial number]

	[-m xx run specific decoding model (default: 2), see README for more details]
	[-b benchmark demodulation models for time - for development purposes (default: off)]
	[-F run model optimized for speed at the cost of accuracy for slow hardware (default: off)]

	Device specific settings:

	[-gr RTLSDRs: TUNER [auto/0.0-50.0] RTLAGC [on/off] BIASTEE [on/off] FREQOFFSET [-150-150] BW [0+]
	[-p xx equivalent to -gr FREQOFFSET xx]
	[-gm Airspy: SENSITIVITY [0-21] LINEARITY [0-21] VGA [0-14] LNA [auto/0-14] MIXER [auto/0-14] BIASTEE [on/off] ]
	[-gh Airspy HF+: TRESHOLD [low/high] PREAMP [on/off] ]
	[-gs SDRPLAY: GRDB [0-59] LNASTATE [0-9] AGC [on/off] ]
	[-gf HACKRF: LNA [0-40] VGA [0-62] PREAMP [on/off]
	[-gt RTLTCP: HOST [address] PORT [port] TUNER [auto/0.0-50.0] RTLAGC [on/off] FREQOFFSET [-150-150] PROTOCOL [none/rtltcp]
	[-ga RAW file: FILE [filename] FORMAT [CF32/CS16/CU8/CS8]
	[-gw WAV file: FILE [filename]
	[-gz ZMQ: ENDPOINT [endpoint] FORMAT [CF32/CS16/CU8/CS8]

	Model specific settings:

	[-go Model: FP_DS [on/off] PS_EMA [on/off] (requires specification of model via -m)]
````

### Basic usage


To test a proper installation and/or compilation (see below) we can list the devices available for AIS reception:
```console
AIS-catcher -l
```
The output depends on the available devices but will look something like:
```console
Found 1 device(s):
0: Realtek, RTL2838UHIDIR, SN: 00000001
```
A specific device can be selected with the ``d``-switch like ``-d:0`` or ``-d 00000001``.


To start AIS demodulation, print some occasional statistics (every 10 seconds) and broadcast AIS messages via UDP, we can use the following command:
```console
AIS-catcher -v 10 -u 127.0.0.1 12345 -u 127.0.0.1 23456
```
If successful, NMEA messages will start to come in, appear on the screen and send as UDP messages to `127.0.0.1` port `12345` and port `23456`. 
These console messages can be suppressed with the option ```-q```. 


The following commands record a signal with ```rtl_sdr``` at a sampling rate of 288K Hz and pipes it to AIS-catcher for decoding:
```console
rtl_sdr -s 288K -f 162M  - | AIS-catcher -r . -s 288K -v
```
An example of using sox for downsampling the signal and then sending the result to AIS-catcher:
```console
sox -c 2 -r 1536000 -b 8 -e unsigned -t raw posterholt.raw -t raw -b 16 -e signed -r 96000 - |AIS-catcher -s 96K -r CS16 . -v
```
### A note on device sample rates
AIS-catcher automatically sets an approriate sample rate depending on your device but provides the option to overwrite this default using the ```-s``` switch. For example for performance reasons you can decide to use a lower rate or improve the sensitivity by picking a higher rate than the default. The decoding model supports the following rates:
```
12288K, 10000K (*), 6144K, 6000K (*), 3072K, 3000K (*), 2500K (*), 2340K, 2000K (*), 1920K (*), 1536K
1152K, 1100K (*), 1000K (*), 912K (*), 900K (*), 768K, 384K, 288K, 250K (*), 240K (*), 192K, 96K
```
Before splitting the signal in two seperate signals for channel A and B, AIS-catcher downsamples the signal to 96K samples/second by successively decimating the signal by a factor 2 and/or 3. The sample rates denoted with a (```*```) in the above are upsampled to a nearby higher rate to make it fit in this computational structure. Hence, there is no efficiency advantage of using these derived rates.
Please note that these are rates supported by the decoding model and might not be necesarily supported by the SDR hardware.

## Special topics

### Running on hardware with performance limitations

AIS-catcher implements a trick to speed up downsampling for RTLSDR input at 1536K samples/second by using fixed point calculations (```-m 2 -go FP_DS on```). In essence the downsampling is done 
in 16 bit integers performed in parallel for the I and Q channel using only 32 bit integers.
Furthermore a new model was introduced which uses exponential moving averages in the determination of the phase instead of a standard moving average as for the default model (```-m 2 -go PS_EMA on```).

![Image](https://raw.githubusercontent.com/jvde-github/AIS-catcher/media/media/raspberry.jpg)

Both features can be activated with the ```-F``` switch. 
To give an idea of the performance improvement on a Raspberry Pi Model B Rev 2 (700 MHz), I used the following command to decode from a file on the aforementioned Raspberry Pi:

```
AIS-catcher -r posterholt.raw -s 1536K -b -q -v
```
Resulting in 38 messages and the ```-b``` switch prints the timing used for decoding:
```
[AIS engine v0.31]	: 17312.1 ms
```
Adding the ```-F``` switch yielded the same number of messages but timing is now:
```
[AIS engine (speed optimized) v0.31]	: 7722.32 ms
```
This and other performance updates make the full version of AIS-catcher run on an early version of the Raspberry Pi with reasonable processor load.

### Connecting to GNU Radio via ZMQ

The latest code base of AIS-catcher can take streaming data via ZeroMQ (ZMQ) as input. This allows for an easy interface with packages like GNU Radio. The steps are simple and will be demonstrated by decoding the messages in the AIS example file from [here](https://www.sdrplay.com/iq-demo-files/). AIS-catcher cannot directly decode this file as the file contains only one channel, the frequency is shifted away from the center at 162Mhz and the sample rate of 62.5K SMPS is not supported in our program. We can however perform some decoding with some help from GNU Radio. First start AIS-catcher to receive a stream (data format is complex float and sample rate is 96K) at a defined ZMQ endpoint:
```
AIS-catcher -z CF32 tcp://127.0.0.1:5555 -s 96000
```
Next we can build a simple GRC model that performs all the necessary steps and has a ZMQ Pub Sink with the chosen endpoint:
![Image](https://raw.githubusercontent.com/jvde-github/AIS-catcher/media/media/SDRuno%20GRC.png)
Running this model, will allow us to succesfully decode the messages in the file:

![Image](https://raw.githubusercontent.com/jvde-github/AIS-catcher/media/media/SDRuno%20example.png)

The ZMQ interface is useful if a datastream from a SDR needs to be shared and processed by multiple decoders or for experimentation with different decoder models with support from GNU Radio.

### Multiple receiver models

The command line provides  the ```-m``` option which allows for the selection of the specific receiver models.  In the current version 4 different receiver models are embedded:

- **Default model** (``-m 2``): the default demodulation engine
- **Base model (non-coherent)** (``-m 1``): similar to RTL-AIS (and GNUAIS/Aisdecoder) with some modifications to PLL (different speeds at different stages) and main receiver filter (computed with a stochastic search algorithm).
- **Standard model (non-coherent)** (``-m 0``): as the base model with brute force timing recovery.
- **FM discriminator model**: (``-m 3``) as  the 'standard' model but assumes input is output of a FM discriminator, hence no FM demodulation takes place which allows ```AIS-catcher``` to be used as GNUAIS and AISdecoder.

The default model is the most time and memory consuming but experiments suggest it to be the most effective. In my home station it improves message count by a factor 2 - 3. The reception quality of the `standard` model over the `base` model is more modest at the expense of roughly a 20% increase in computation time. Advice is to start with the default model, which should run fine on most modern hardware including a Raspberry 4B and then scale down to ```-m 0```or even ```-m 1```, if needed.

 Notice that you can execute multiple models in one run for benchmarking purposes but only the messages from the first model specified are displayed. To benchmark different models specify ```-b``` for timing and/or ```-v``` to compare message count, e.g.
```console
AIS-catcher -s 1536K -r posterholt.raw -m 2 -m 0 -m 1 -q -b -v
```
The program will run and summarize the performance (count and timing) of three decoding models (on a Raspberry Pi 4B):
```
[AIS engine v0.35]:                      38 msgs at 6.3 msg/s
[Standard (non-coherent)]:               4 msgs at 0.7 msg/s
[Base (non-coherent)]:                   3 msgs at 0.5 msg/s
```
```
[AIS engine v0.35]:                      1036.54 ms
[Standard (non-coherent)]:               932.47 ms
[Base (non-coherent)]:                   859.065 ms
```
In this example the default model performs quite well in contrast to the standard non-coherent engine with 38 messages identified versus 4 for the standard engine. 
This is typical when there are few messages with poor quality. However, it increases the decoding time a bit and has a slightly higher memory usage so needs more powerful hardware. Please note that the improvements seen for this particular file are an exception.


### Input from FM discriminator
We can run AIS-catcher on a RAW audio file as in this [tutorial](https://github.com/freerange/ais-on-sdr/wiki/Testing-GNU-AIS):
```console
wget "https://github.com/freerange/ais-on-sdr/wiki/example-data/helsinki-210-messages.raw"
AIS-catcher  -m 3 -v -s 48K -r cs16 helsinki-210-messages.raw
```
On this file we should extract roughly ``362`` AIVDM lines. Notice that with switch ```-m 3``` on the command line AIS-catcher runs a decoding model that assumes the input is the output of an FM discriminator. In this case the program is similar to the following usage of GNUAIS:
```console
gnuais -l helsinki-210-messages.raw
```
which produces:
```
INFO: A: Received correctly: 153 packets, wrong CRC: 49 packets, wrong size: 4 packets
INFO: B: Received correctly: 52 packets, wrong CRC: 65 packets, wrong size: 10 packets
```

## Examples of device specific settings

The command line allows you to set some device specific parameters. AIS-catcher follows the settings and naming conventions for the devices as much as possible so that parameters and settings determined by SDR software for signal analysis (e.g. SDR#, SDR++, SDRangel) can be directly copied. Below some examples.

### RTL SDR
Gain and other settings specific for the RTL SDR can be set on the command line with the ```-gr``` switch. For example, the following command sets the tuner gain to +33.3 and the RTL AGC on:
```console
AIS-catcher -gr tuner 33.3 rtlagc ON
```
Please note that these settings are not case sensitive.

### Airspy HF+
Gain settings specific for the Airspy HF+ can be set on the command line with the ```-gh``` switch. For example, the following command switches off the preamp:
```console
AIS-catcher -gh preamp OFF
```
Please note that only AGC mode is supported.

### Airspy Mini/R2

The Airspy Mini and R2 require careful gain configuration as described [here](https://airspy.com/quickstart/). As outlined in that reference there are three different gain modes: linearity, sensitivity and so-called free. These can be set via the ```-gm```switch when using the Airspy. We can activate 'linearity' mode with gain ```10```using the following ```AIS-catcher``` command line:
```console
AIS-catcher -gm linearity 10
```
Finally, gains at different stages can be set as follows:
```console
AIS-catcher -gm lna AUTO vga 12 mixer 12
```
More guidance on setting the gain model and levels can be obtained in the mentioned reference.

### SDRplay RSP1/RSP1A/RSPDX (API 3.x)
Settings specific for the SDRplay  can be set on the command line with the ```-gs``` switch, e.g.:
```console
AIS-catcher -gs lnastate 5
```

### HackRF
Settings specific for the HackRF can be set on the command line with the ```-gf``` switch, e.g.:
```console
AIS-catcher -gf lna 16 vga 16 preamp OFF
```

### RTL TCP
AIS-catcher can process the data from a [`rtl_tcp`](https://projects.osmocom.org/projects/rtl-sdr/wiki/Rtl-sdr#rtl_tcp) process running remotely, e.g. if the server is on `192.168.1.235` port `1234` with a sampling rate of `240K` samples/sec:
```console
AIS-catcher -t 192.168.1.235 1234 -s 240000 -v
```

## Validation

### Experimenting with recorded signals
The functionality to receive radio input from `rtl_tcp` provides a route to compare different receiver packages on a deterministic input from a file. I have tweaked the callback function in `rtl_tcp` so that it instead sends over input from a file to an AIS receiver like `AIS-catcher` and `AISrec`. The same trick can be easily done for `rtl-ais`. The sampling rate of the input file was converted using `sox` to 240K samples/second for `rtl-tcp` and 1.6M samples/second for `rtl-ais`. The output of the various receivers was sent via UDP to AISdipatcher which removes any duplicates and counts messages. The results in terms of  number of messages/distinct vessels:
 | File | AIS-catcher v0.35  | AIS-catcher v0.33 | rtl-ais | AISrec 2.208 (trial - super fast) | AISrec 2.208 (pro - slow2)  | Source |
 | :--- | :--- | :---: | :---: | :---: | :---: | :---: | 
 |Scheveningen |   44/37| 43/37  | 17/16 | 30/27 | 37/31 | recorded @ 1536K with `rtl-sdr` (auto gain) |
 |Moscow| 213/35 |210/32 | 146/27 |  195/31 |  183/34 | shared by user @ 1920K in [discussion](https://github.com/jvde-github/AIS-catcher/issues/7) |
 |Vlieland | 93/54 |93/53  | 51/31| 72/44 | 80/52 | recorded @ 1536K with `rtl-sdr` (auto gain) |
 |Posterholt |  39/22  |39/22 |2/2 | 13/12 | 31/21 | recorded @ 1536K with `rtl-sdr` (auto gain) |
 
 **Update 1:** AISrec recently had a version update of 2.208 (October 23, 2021) with improved stability and reception quality and the table above has been updated to include the results from this recent version. 

 **Update 2:** Feverlaysoft has kindly provided me with a license for version 2.208 of AISrec allowing access to additional decoding models. Some experimentation suggests that "Slow2" works best for these particular examples and has been included in the above overview.
 
## Build process

### Ubuntu, Raspberry Pi, macOS, MSVC
The steps to compile AIS-catcher for RTL-SDR dongles are fairly straightforward on most systems. There is a simple Makefile, a ```solution``` file for MSVC discussed in the next section and you can use ```cmake```, as we are detailing now.

First step is to ensure you have the necessary dependencies and build tools installed for your device(s). 
For example, the following installs the minimum build tools for Ubuntu and Raspberry Pi:
```console
sudo apt-get update
sudo apt-get upgrade

sudo apt-get install git make gcc g++ cmake pkg-config -y
```
AIS-catcher requires libraries for the particular hardware you want to use. The following table summarizes the installation instructions for all supported hardware:

  System              | Linux/RPI/apt              | macOS/brew  |  MSVC/vcpkg   |     MSVC/PothosSDR |
 :--            | :--			| :--				| :--  | :--: | 
 Device    | **sudo apt install ...**      | **brew install ...** | **vcpkg install ...**            | [Download](https://downloads.myriadrf.org/builds/PothosSDR/) |
***RTL-SDR***          | librtlsdr-dev          | librtlsdr  | rtlsdr rtlsdr:x64-windows             | X |
***Airspy***          | libairspy-dev                             | airspy |    -                    | X |
***Airspy HF+***        | libairspyhf-dev                            | airspyhf  |    -                | X |
***HackRF***          | libhackrf-dev                             | hackrf    |    -                 | X |
***SDRplay 1A***         | [API 3.x](https://www.sdrplay.com/downloads/) | - | [API 3.x](https://www.sdrplay.com/downloads/)     | [API 3.x](https://www.sdrplay.com/downloads/)  |
***ZeroMQ***             | libzmq3-dev     | zeromq      | ZeroMQ ZeroMQ:x64-windows                  | X |

Once the dependencies are in place, the process to install AIS-catcher then on Linux based systems becomes:
```console
git clone https://github.com/jvde-github/AIS-catcher.git
cd AIS-catcher
mkdir build
cd build
cmake ..
make
sudo make install
```
For the SDRPlay the software needs to be downloaded and installed from the website of the manufacturer. Once installed, the AIS-catcher build process automatically includes it in the build if available. 

For Windows, clone the project and open the directory with AIS-catcher in Visual Studio 2019 or above. The ```cmake``` file provides two options as source for the libaries. The first is to install all the drivers via PothosSDR from [here](https://downloads.myriadrf.org/builds/PothosSDR/).  The cmake file will locate the installation directory and link against these libraries. The alternative is to use ```vcpkg``` which currently only offers the libraries for RTL-SDR and ZeroMQ (see next section as well). Of course, you can save yourself the hassle and download the Windows binaries from above.

### Microsoft Visual Studio 2019+ via solution file (RTL-SDR/ZMQ only)

Ensure that you have ```vcpkg``` [installed](https://vcpkg.io/en/getting-started.html) and integrated into Visual Studio via ```vcpkg integrate install``` (as Administrator). Then install the rtl-sdr drivers as follows:
```
vcpkg install rtlsdr rtlsdr:x64-windows ZeroMQ ZeroMQ:x64-windows
```
The included solution file in the mscv directory allows you to build AIS-catcher with RTL-SDR/ZMQ support in the Visual Studio IDE.

## Container images

Pre-built container images containing AIS-catcher are available from the GitHub Container Registry. Available container tags are documented on the [package's page](https://github.com/jvde-github/AIS-catcher/pkgs/container/ais-catcher), with `latest` (the latest release) and `edge` (the bleeding edge of the `main` branch) being the two main ones.

The following `docker run` command provides an example of the usage of this container image, running the latest release of AIS-catcher interactively:

```console
docker run --rm -it --pull always --device /dev/bus/usb ghcr.io/jvde-github/ais-catcher:latest <ais-catcher command line options>
```

Alternatively, the following `docker-compose.yml` configuration provides a good starting point should you wish to use [Docker Compose](https://docs.docker.com/compose/):

```yaml
services:
  ais-catcher:
    command: <ais-catcher command line options>
    container_name: ais-catcher
    devices:
      - "/dev/bus/usb:/dev/bus/usb"
    image: ghcr.io/jvde-github/ais-catcher:latest
    restart: always
```
Please note that the SDRplay devices are currently not supported in the Docker images.

## Considerations

AIS-catcher tunes in on a frequency of 162 MHz. However, due to deviations in the internal oscillator of RTL-SDR devices, the actual frequency can be slightly off which will result in no or poor reception of AIS signals. It is therefore important to provide the program with the necessary correction in parts-per-million (ppm) to offset this deviation where needed. For most of our testing we have used the RTL-SDR v3 dongle where in principle no frequency correction is needed as deviations are guaranteed to be small. For optimal reception though ensure you determine the necessary correction, e.g. [see](https://github.com/steve-m/kalibrate-rtl) and provide as input via the ```-p``` switch on the command line.

On some laptops we observed that Windows was struggling with high volume of data transferred from the RTL SDR dongle to the PC. I am not sure why (likely some driver issue as Ubuntu on the same machine worked fine) but it is worthwhile to check if your system supports transferring from the dongle at a sampling rate of 1.536 MHz with the following command which is part of the osmocom rtl-sdr package:
```console
rtl_test -s 1536000
```
In case you observe a high number of lost data, the advice is to run AIS-catcher at a lower sampling rate for RTL SDR dongles:
```console
AIS-catcher -s 288000
```
If your system allows for it you might opt to run ```AIS-catcher``` at a sample rate of ```2304000```. 

## Known issues

- call of ```rtlsdr_close```  in MS VC++ can result in a crash. Solution: ensure you have the latest version of the library with this patch [rtlsdr](https://github.com/osmocom/rtl-sdr/pull/18).
- pkg-config on Raspberry Pi returns ```-L``` as library path which results in a build error. Temporarily fixed by assuming lib is in standard location, long term fix: switch to cmake
- ...

## To do

- Marine VHF audio and DSC decoding from SDR input signal
- Optional filter for invalid messages
- DSP: improve filters (e.g. add droop compensation, large rate reductions), etc
- Decoding: add new improved models (e.g. using matched filters, alternative freq correction models), software gain control, document current model
- Testing: more set ups, assess gap with commercial equipment
- GUI: Windows, Android
- Multiple SDRs: validate location from signal (e.g. like MLAT), privacy filters
- Output: ZeroMQ, APRS, ...
- Input: ZeroMQ/TCP-IP protocols, SoapySDR, LimeSDR mini, ...
- ....
