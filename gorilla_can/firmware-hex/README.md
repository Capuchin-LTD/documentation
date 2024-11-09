# Updating GorillaCAN firmware

Download the latest hex file from this folder along with `gorilla_can.conf`. Install latest AVRDUDE. Use the  following command to upload firmware.

```avrdude.exe -PCOM3 -e -p GorillaCAN_1 -c GorillaProgrammer_1 -b19200 -D -C C:\Users\monkey\gorilla_can.conf -U flash:w:C:\Users\monkey\firmware_v0.1.hex:i```

Substituting `COM3` for your port, `C:\Users\monkey\firmware_v0.1.hex` for the path to your hex file and `C:\Users\monkey\gorilla_can.conf` for the path to the configuration file.

You may encounter an error about device signature not mathcing from time to time. If this happens then trying again once or twice normally will succeed.