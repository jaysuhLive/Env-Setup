# install ubuntu on nuc (should install with usb key/mouse)
- 1. Format ssd
- 2. set boot priority USB image to top
- 3. install ubuntu with grub nomodeset command
- 4. set disk partion as swap area - (512mb), / (root, all), EFI - (512mb)
- 5. bootloader should be default

# boot with esc to pop grub
- press e to edit grub command line
- add nomodeset right after 'quite splash'

# successfully boot, set mod grub command line
```
sudo gedit /etc/default/grub
```
- add 'nomodeset' to 'GRUB_CMD_LINE_LINUX_DEFAULT="quite splash"', it should be seen like this
```
GRUB_CMD_LINE_LINUX_DEFAULT="quite splash nomodeset"
```
- updqte grub
```
sudo update-grub2
```
