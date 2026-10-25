# The Moving Parts

A Ventoy stick, the files that make
one, and the helper programs in the
download. Knowing which piece does
what turns "it won't boot" into a
question you can ask precisely.

## On the stick: two partitions

Installing Ventoy divides the stick
into two partitions:

| Partition | Size | Filesystem | Holds |
|---|---|---|---|
| 1st | the rest of the stick | **exFAT**, labelled `Ventoy` | your image files, in any folders; the `ventoy` folder with your configuration |
| 2nd | 32 MB | **FAT16**, labelled `VTOYEFI` | the UEFI boot files and the menu program |

The split is the whole design in
miniature. The big partition is
ordinary storage — exFAT because it is
read by Windows, macOS and Linux alike
and has no 4 GB file-size limit, so a
9 GB Windows image fits. The small
partition is the machinery: firmware
looks for boot files in a FAT
partition, so Ventoy keeps one, exactly
big enough, out of your way. You never
need to put anything in it by hand.

The lab did not have to take this on
faith. The release package carries the
second partition as a ready-made image,
and the lab took it apart: it is a
32 MiB FAT16 filesystem labelled
`VTOYEFI`, containing a `grub` folder
(the menu program and its settings),
an `EFI` folder (the boot programs
themselves), the project's own
`ventoy` and `tool` folders, and one
small file with a long name —
`ENROLL_THIS_KEY_IN_MOKMANAGER.cer`,
the Secure Boot key from chapter 13
(transcript t08).

## Boot code for two generations

Computers start in one of two ways,
and a Ventoy stick is ready for both:

- **Legacy BIOS** reads the stick's
  **master boot record** — the first
  512 bytes of the disk. Ventoy's
  installer writes its `boot.img`
  there (the lab checked the copy in
  the package: a genuine boot sector,
  ending in the `55 aa` signature every
  BIOS looks for), and that loads a
  larger **core image** — GRUB, the
  bootloader Ventoy's menu is built on.
- **UEFI** firmware instead reads
  files from the `VTOYEFI` partition:
  `EFI/BOOT/BOOTX64.EFI` and its
  siblings for other processor types.
  The same GRUB menu appears.

Either way you land in the same place:
a list of the image files on the first
partition. Menus can be graphical or
plain text; chapter 8 covers choosing.

## In the download: the tools

The release package (chapter 3) holds
more than the installer:

| Program | What it is for |
|---|---|
| `Ventoy2Disk` (Windows `.exe`, Linux GUI and `Ventoy2Disk.sh`) | the installer itself: makes and updates sticks |
| `VentoyWeb.sh` | the same installer as a local web page, for headless Linux machines |
| `VentoyPlugson.sh` | a web page for editing a stick's configuration (chapter 8) |
| `CreatePersistentImg.sh`, `ExtendPersistentImg.sh` | make and grow the files behind persistence (chapter 9) |
| `VentoyVlnk.sh` | makes the link files that boot images stored on a computer's own disk |
| the LiveCD | a bootable CD image containing the installer, for machines with no working system |

All of them drive the same small set
of worker programs per processor type
(the package carries x86_64, i386,
ARM64 and MIPS builds). The lab ran
the x86_64 set.

## On your computer: nothing

There is no Ventoy service, account or
cloud piece. The installer runs when
you run it and exits. The configuration
is a single file, `ventoy.json`, in a
folder named `ventoy` on the stick's
first partition — the stick carries
its own settings with it, so a stick
you configure behaves the same in
every machine (chapter 8).
