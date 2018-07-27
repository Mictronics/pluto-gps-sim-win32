# PLUTO-GPS-SIM Win32

PLUTO-GPS-SIM Win32 generates a GPS baseband signal IQ data stream, which is then transmitted by the 
software-defined radio (SDR) platform [ADALM-Pluto](https://wiki.analog.com/university/tools/pluto).

Project based on [gps-sdr-sim](https://github.com/osqzss/gps-sdr-sim).

[Linux version](https://github.com/Mictronics/pluto-gps-sim)

### Generating the GPS signal
The user is able to assign a static location directly through the command line.

The user also specifies the GPS satellite constellation through a GPS broadcast 
ephemeris file. The daily GPS broadcast ephemeris file (brdc) is a merge of the
individual site navigation files into one. The archive for the daily file is:

[ftp://cddis.gsfc.nasa.gov/gnss/data/daily/](ftp://cddis.gsfc.nasa.gov/gnss/data/daily/)

These files are then used to generate the simulated pseudorange and
Doppler for the GPS satellites in view. This simulated range data is 
then used to generate the digitized I/Q samples for the GPS signal
which are then feed into ADALM-Pluto SDR to transmit the GPS L1 frequency.

The simulation start time can be specified if the corresponding set of ephemerides
is available. Otherwise the first time of ephemeris in the RINEX navigation file
is selected.

### Build instructions
#### Dependencies

1. Download ADALM-Pluto drivers for Windows: https://wiki.analog.com/university/tools/pluto/drivers/windows
2. Download libiio for Windows: https://github.com/analogdevicesinc/libiio
3. Download libad9361 32-bit for Windows: https://github.com/analogdevicesinc/libad9361-iio

#### Building with Visual Studio
4. Open as Visual Studio project using pluto-gps-sim.sln.
5. Build as Debug or Release.

#### Requirements

5. Copy DLL's from libiio and libad9361 to output folder (Debug or Release) or system32 folder.
6. Requires a copy of zcat.exe either in output folder (Debug or Release) or somewhere referenced by PATH variable.

:exclamation: Tested on Windows 7 only. Builds also as x64 target.

### Usage
```
pluto-gps-sim [options]
Options:
  -e <gps_nav>     RINEX navigation file for GPS ephemerides (required)
  -f               Pull actual RINEX navigation file from NASA FTP server
  -c <location>    ECEF X,Y,Z in meters (static mode) e.g. 3967283.15,1022538.18,4872414.48
  -l <location>    Lat,Lon,Hgt (static mode) e.g. 30.286502,120.032669,100
  -t <date,time>   Scenario start time YYYY/MM/DD,hh:mm:ss
  -T <date,time>   Overwrite TOC and TOE to scenario start time (use ```now``` for actual time)
  -s <frequency>   Sampling frequency [Hz] (default: 2600000)
  -i               Disable ionospheric delay for spacecraft scenario
  -v               Show details about simulated channels
  -A <attenuation> Set TX attenuation [dB] (default -20.0)
  -B <bw>          Set RF bandwidth [MHz] (default 5.0)
  -U <uri>         ADALM-Pluto URI (eg. usb:1.2.5)
  -N <network>     ADALM-Pluto network IP or hostname (default pluto.local)
```

Set static mode location:

```
> pluto-gps-sim -l 38.0,24.0,300 -e brdc.n -T now -A 0.0 -U usb:3.5.5
```

Set TX attenuation:
```
> pluto-gps-sim -e brdc.n -A -30.0
```
Default -20.0dB. Applicable range 0.0dB to -80.0dB in 0.25dB steps.

Set RF bandwidth:
```
> pluto-gps-sim -e brdc.n -B 3.0
```
Default 3.0MHz. Applicable range 1.0MHz to 5.0MHz

### Transmitting the samples

The TX port of a particular SDR platform is connected to the GPS receiver
under test through a DC block and a fixed 50-60dB attenuator.

### License

Copyright &copy; 2018 Mictronics
Distributed under the [MIT License](http://www.opensource.org/licenses/mit-license.php).
