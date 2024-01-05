# geo-tester
Research tool to assess gps device behavior when passed different traffic.

Pairs great with https://github.com/era1112/pi-otg (gpio control of scripts)

Findings:
- "primitive" garmin-type gps's accept the stream and decode a position
- "smarter" devices (ie. android handsets) recieve the beacons, enumerate SV's, but the kernel does not accept it as a valid lat/long.
- However, the device does not enter a "gps error" state. It indefinitely holds in "finding location" but never throws a "no location" alert. Some apps will continue to show that last known position.

Setup:
- Download real-world RINEX data:
  - https://cddis.nasa.gov/archive/gnss/data/daily/ (free login)
  - download the file at: /[current year]/[highest number]/??n/brdc*
-  Produce research data:
    ```
    - gps-sdr-sim -e <RINEX_FILE> -b 8 -l 0,0,200 -o ./researchtape.bin -v -s 2600000 -d 7200 -T 2021/02/23,20:34:00
    ```
    - b8, s2600000 are rf parameters to match the hackrf, l is location in lat/long, 200 is elevation, d is length of tape in seconds, T is the start time and forces toc/toe to align, o is output file.
    
- Modulate research data:
  ```
  -   ./gps-sim -e rinex/*.23n -l 0,0,200 -s now -g 47 -a -d 1800 -v -i -r hackrf --disable-almanac
  ```
  -   e is input, l is loc, s is start time, g is gain, a sets amplifier on, d is duration, r is radio mode, vi is verbose interactive
  -   disable-almanac removes coarse almanac data from the stream (more complex to transmit/recieve, not necessary for this experiment). 

Documentation:
- This readme

Dependencies:
- hackrf, gnuradio, gnuradio-osmosdr, base-devel, multi-sdr-gps-sim
- Download and make: https://github.com/osqzss/gps-sdr-sim 
- Tested on hackrf one with 5cm dipole antenna

Future:
- Antenna testing, more reciever-specific testing
