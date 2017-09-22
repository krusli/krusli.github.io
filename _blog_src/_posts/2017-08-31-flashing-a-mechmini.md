---
layout: post
title: Restoring default firmware on a Mechkeys.ca Mechmini 1
---

I had QMK installed on my Mechmini and was looking for a way to restore it to the original firmware. A quick Google search found nothing particularly useful, so I thought I'd note this down in case anyone needs any help with this.

The steps were done on a Mac, but it should work just fine on Windows too (bar the additional driver installations, if any).

## Files
- `mechmini.hex` from [Mechkeys.ca's old site](https://oldsite.mechkeys.ca/help/)
- `ps2avrGB_firmware_V1.3.2_170531.zip` from [ps2avrU GitHub repo](https://github.com/showjean/ps2avrU/releases)
- BootMapper Client from this [Bootmapper client guide](https://github.com/unxmaal/mechkeys_howto)
- [my .json keymap](https://krusli.me/files/bootmapper.json). Right click the link and click on `Save link as file` (or similar).

## Steps
1. Open BootMapper Client and go to the *Options* tab.
2. Hold down `L_Ctrl` when plugging in the keyboard to enter bootloader mode on the keyboard. Flash the `ps2avrGB_NKRO.hex` file from `ps2avrGB_firmware_V1.3.2_170531.zip`.
3. Flash `mechmini.hex` afterwards. This might not be necessary as the `mechmini.hex` file just seems to be a keymap (which shows up as all jumbled on my end).
4. Go to the *Key Mapper* tab. If you see a jumble of keys, just load my `.json` keymap and click *Upload* after tweaking it to your liking.

## Links
- https://github.com/unxmaal/mechkeys_howto
- https://github.com/qmk/qmk_firmware/tree/master/keyboards/mechmini
