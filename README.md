# geo-tester
Expriment with gps device behavior when passed different traffic.
Pairs great with https://github.com/era1112/pi-otg (gpio control of scripts)


Setup:
- make "payload.sh" point at the script you want to run when you push the button.
- look at the script that "interface.sh" points at, connect pushbutton to appropriate gpio pin. If you want to use a different gpio button or do more complex triggering, change the script.
- set up systemctl to keep interface.sh running (see attached .docx).
- reboot pi, push the button, your script will run

Flow of control:
- systemctl runs "pi-otg.service" at boot, which is responsible for keeping "interface.py" (symlink to the interface script you really want) running at all times.
- the interface script polls on GPIO and calls "payload.sh" (symlink to the actual desired payload script) when the desired trigger is met (gpio, time, gps, whatever).
- the payload script carries out the system functions when in the triggered state.

Project layout:
- interface.py and payload.sh are symlinks that you alter when you want a different trigger interface or payload
- the interfaces and payloads folders have interfaces and payloads to choose from

Docs:
- main documentation is the docx at top level, although the project deviates some names and details

Dependencies:
- python3, gpiozero. Maybe some other stuff, there's like 3 scripts, if it doesn't work then just read em lol.

Future:
- add protection against multiple-execution of the payload
- add on/off functionality of payload by pressing button
- add LED indication of payload status
