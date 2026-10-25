# Your First Boot

With Ventoy on the stick (chapter 4),
everything left is file copying and
one reboot. *The boot itself is
documented-only in this guide — the
lab had no machine it could reboot
into a stick — and the steps below
are the tool's ordinary, documented
behaviour.*

## 1. Copy an image onto the stick

Plug the stick into any computer. It
appears as a drive named **Ventoy**.
Download an installer image — say a
Linux distribution's ISO — and copy
the file onto the drive, exactly as
you would copy a photo. Wait for the
copy to finish and **eject the stick
properly**: installers are read to
their last byte at boot, and a copy
cut short by yanking the stick is the
cause of more "corrupt image" reports
than bad downloads ever were.

That is the entire preparation. There
is no step 2.

## 2. Boot from the stick

Restart the target machine with the
stick plugged in, and call up its
**boot menu** — a one-time choice of
what to start from, usually reached
with a key pressed during the first
seconds of power-on (F12, F8, Esc or
F2 are common; the machine's manual
or first screen names it). Choose the
USB stick. Machines set to boot only
from their internal disk, and machines
with Secure Boot, have their own
sections in chapter 14.

On a UEFI machine the stick may be
listed twice, once plainly and once
with a `UEFI:` prefix: pick the UEFI
entry on a modern machine, the plain
one when the UEFI entry will not
start — the stick carries both kinds
of boot code (chapter 2).

## 3. The menu

Ventoy's screen lists the image files
on the stick — folders appear as
folders, so the menu mirrors how you
organised the drive (chapter 6). Pick
an entry with the arrow keys, press
Enter, and the image boots as if it
had the machine to itself. From here
you are in that image's own world:
its installer, its live desktop, its
rules.

A handful of keys change how a file
boots. They appear at the bottom of
the menu screen:

| Key | What it does |
|---|---|
| **F1** | Memdisk mode: load the whole image into memory, then boot it — a rescue for small images that will not boot the direct way |
| **F2** | Browse the machine's own disks and boot a file found there |
| **F3** | Switch the menu between a folder tree and a flat list |
| **F4** | Boot from the local disk instead — the polite exit |
| **F5** | Tools: check an image file's checksums, debug options |
| **F6** | Your own extra menu entries, if you have written any (chapter 8) |

Check: a healthy first run is boring.
The menu lists exactly the files you
copied, the one you pick starts, and
its installer or live system behaves
as it would from a stick of its own.
When it is not boring, chapter 14
names the usual reasons in the order
to check them.
