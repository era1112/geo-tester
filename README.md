# geo-tester
Experiment with gps device behavior when passed different traffic.

Pairs great with https://github.com/era1112/pi-otg (gpio control of scripts)

Findings:
- "primitive" garmin-type gps's accept the stream and decode a position
- "smarter" devices (ie. android handsets) recieve the individual beacons, but the OS does not accept it as a valid fix. Device does not throw a "gps error" state, seems to keep the last valid position or fails to provide a location without going into a "no location" alert.

Setup:
- Download real-world RINEX data:
  - https://cddis.nasa.gov/archive/gnss/data/daily/ (free login)
  - download the file at: /current year/highest number/??n/brdc*
-  Produce research data:
-    gps-sdr-sim -e <RINEX_FILE> -b 8 -l <COORDS>,100 -o ./researchtape.bin -v -s 2600000 -d 7200 -T 2021/02/23,20:34:00  // b8, s2600000 are rf parameters to match the hackrf, coords is lat/long, 100 is elevation, d is length of tape in seconds, T is the start time and forces toc/toe to align, o is output file.
- Broadcast research data:
-   ./gps-sim -e rinex/*.23n -l 0,0,200 -s now -g 47 -a -d 1800 -v -i -r hackrf --disable-almanac  // e is input, l is loc, s is start time, g is gain, a is amplifier on/off, d is duration, r is radio mode, vi is verbose interactive

Documentation:
- This readme

Dependencies:
- hackrf, gnuradio, gnuradio-osmosdr, base-devel, multi-sdr-gps-sim
- Download and make: https://github.com/osqzss/gps-sdr-sim 
- Tested on hackrf one with 5cm dipole antenna

Future:
- Antenna testing, more reciever-specific testing
