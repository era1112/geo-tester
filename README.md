# geo-tester
Expriment with gps device behavior when passed different traffic.

Pairs great with https://github.com/era1112/pi-otg (gpio control of scripts)

Findings:
- "primitive" garmin-type gps's accept the stream and decode a position
- "smarter" devices (ie. android handsets) recieve the individual beacons, but the OS does not accept it as a valid fix. Device does not throw a "gps error" state, usually just keeps the gps position as the last valid position.

Setup:
- Download real-world RINEX data:
  - https://cddis.nasa.gov/archive/gnss/data/daily/ (free login)
  - download the file at: /<current year>/<highest number>/??n/brdc*
-  Produce research data:
  - gps-sdr-sim -e <RINEX_FILE> -b 8 -l <COORDS>,100   // b8 is the iq sample size in bytes, hackrf expects 8. coords is lat/long, 100 is elevation.
- Broadcast research data:
  - 

Documentation:
- This readme

Dependencies:
- hackrf, gnuradio, gnuradio-osmosdr, base-devel, multi-sdr-gps-sim
- Download and make: https://github.com/osqzss/gps-sdr-sim 
- Tested on hackrf one with 5cm dipole antenna

Future:
- Antenna testing, more reciever-specific testing
