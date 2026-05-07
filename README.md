# Update macOS volume name in OpenCore picker

OpenCore uses `.disk_label` file if it exists, otherwise it tries `.contentDetails` and, if this doesn't exist, it tries .`disk_label.contentDetails`.

You have 3 possible methods.

### Using bless

- In Terminal: `diskutil info / | grep "APFS Volume Group"`
- You get a long string with letters and numbers (e.g. 654436D1-8026-4D83-81DA-942C2FE63961)
- `cd /System/Volumes/Preboot/654436D1-8026-4D83-81DA-942C2FE63961/System/Library/CoreServices/`
- `sudo bless --folder . --label "NOMBRE"` where NOMBRE is the name you want to be applied
- Reboot.

![Hidden-files](Hidden-files.png)

To check, in Terminal, one of these commands:

```
cd /System/Volumes/Preboot/654436D1-8026-4D83-81DA-942C2FE63961/System/Library/CoreServices/
pico .disk_label.contentDetails
```

or

`printf "NOMBRE" | sudo tee .disk_label.contentDetails; echo sudo chgrp wheel .disk*`

### Editing text file

- SIP disabled
- mount Preboot volume from Terminal (I see Preboot volume without mounting it but other users have to mount that volume)
- `sudo pico /System/Volumes/Preboot/UUID-number/System/Library/CoreServices/.contentDetails` and `/System/Volumes/Preboot/UUID-number/System/Library/CoreServices/.disk_label.contentDetails` (hidden files)
- change the volume name to anything else (plain text, make it simple)
- reboot.

**Note**: UUID-number is the UUID of the APFS volume group where Preboot volume is located, you can do `diskutil list` and `diskutil info diskx` where diskx is the APFS Container Scheme.

### Intel Power Gadget 

Install or reinstall Intel Power Gadget and reboot. It rebuilds preboot Volume and the new name is set.

 
