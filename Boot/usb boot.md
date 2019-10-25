## boot from usb disk

prepare rPi

Create a sd-card to program the OTP bit for booting from usb
Burn a sd-card with a raspbian image.

Add following lines to the /boot/config.txt file
```
# Program OTP bit for booting from usb disk
program_usb_boot_mode=1
```

Now boot with the prepared sd-card.
After booting login and check if the OTP has been programmed
```
$ vcgencmd otp_dump | grep 17:
17:3020000a
```

Remove the lines added to config.txt to prevent updating the OTP bit on the next rPi you use the sd-card in !

Now prepare a usb disk with an (raspbian) image with the normal tools. Remove sd-card insert usb disk and apply power!

notes :  
Only works for the models Pi 3B, 3B+, 3A+, and 2B v1.2 !  
As this writes to an OTP memory it cannot be undone !  
Official page is at https://www.raspberrypi.org/documentation/hardware/raspberrypi/bootmodes/msd.md
