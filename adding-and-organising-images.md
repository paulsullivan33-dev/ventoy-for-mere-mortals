# Adding and Organising Images

Day to day, a Ventoy stick is just a
drive. This chapter is about keeping
that drive a library instead of a
junk drawer.

## What Ventoy will boot

| Extension | What it usually is | Notes |
|---|---|---|
| `.iso` | an installer or live system image | the everyday case; Linux, Windows, BSD and more |
| `.wim` | a Windows installation image | booted with Ventoy's WIM support; a bare `.wim` can serve as a Windows installer source |
| `.img` | a raw disk image | booted as a hard-disk image; results vary by what made it |
| `.vhd`, `.vhdx` | a fixed-size virtual disk | dynamic (growing) VHDs are not supported — fixed only |
| `.efi` | a single EFI boot program | memory testers, shell and diagnostic tools ship this way |

Ventoy scans the whole first
partition, subfolders included, at
every boot. There is no index to
rebuild and nothing to register: copy
a file on, it is in the menu; delete
it, it is gone.

## Folders are the menu

The boot menu mirrors your folder
layout, so organise for the day you
are standing in front of a dead
machine, not for today:

```text
Ventoy (the drive)
├── linux/
│   ├── ubuntu-desktop.iso
│   └── debian-netinst.iso
├── windows/
│   ├── windows-11.iso
│   └── windows-10.iso
├── rescue/
│   ├── memtest86.efi
│   └── clonezilla-live.iso
└── ventoy/
    └── ventoy.json        (chapter 8)
```

Keep the root shallow and the names
honest. A year from now, `rescue/` is
findable at 2 a.m.; `misc2/` is not.

## File names that behave

Ventoy itself is tolerant, but the
images it boots are not always so —
some boot machinery stumbles on names
it did not expect. The project's
standing advice, worth adopting as a
rule:

- **No spaces** in file or folder
  names: write `ubuntu-24.04.iso`,
  not `Ubuntu 24.04 (64-bit).iso`.
- **Plain ASCII letters, digits,
  `-`, `_` and `.`** only. Accents,
  brackets and other punctuation are
  asking a stranger's bootloader to
  have a good day.
- Keep the name **short enough to
  read in the menu** — the menu is
  not the place for a version essay.

## Room, speed and housekeeping

- **Big images are fine.** exFAT has
  no 4 GB file limit, which is the
  main reason the first partition uses
  it (chapter 2). A 9 GB Windows image
  is an ordinary citizen.
- **Defragmentation is a real
  consideration for one case:** Ventoy
  expects an image to be a single
  contiguous file in some boot modes,
  and the F5 tools can check. Copying
  images one at a time onto a stick
  that is not nearly full keeps files
  contiguous in practice.
- **The stick is also storage.**
  Nothing stops you keeping documents
  or a tools folder beside the images;
  Ventoy ignores anything it does not
  recognise. One caution: backup
  software and "cleanup" tools love
  unfamiliar partitions — the 32 MB
  `VTOYEFI` partition sometimes gets
  offered a drive letter in Windows.
  Leave it alone; nothing in it is
  yours to tidy.
- **Verify downloads.** Images carry
  published checksums for the same
  reason chapter 3 gives for Ventoy
  itself. An image that fails its
  checksum will fail creatively at
  boot; checking first is cheaper.

## Removing images

Delete the file. That is all — the
menu rebuilds itself at every boot
from what it finds, and there is no
Ventoy-side state to clean up. The
stick's own Ventoy installation is
untouched by anything you do on the
first partition, which is also why
updating (next chapter) can leave
your library exactly where it sits.
