# What Ventoy Is

Ventoy is a **bootloader for USB
sticks** — a small program that lives on
the stick itself and whose whole job is
getting other programs started.

The usual way of making a bootable
stick is to *write an image onto it*:
a tool copies a disk image (usually an
`.iso` file) over the stick, sector by
sector, until the stick *is* that image.
The stick then holds exactly one thing,
and putting a second thing on it means
wiping it and starting again.

Ventoy splits the job in two:

1. **Once**, its installer prepares the
   stick: two partitions, boot code for
   old and new firmware, and Ventoy's
   menu program (chapter 2).
2. **Ever after**, you add bootable
   things by copying files. An Ubuntu
   installer, a Windows installer, a
   rescue disk — each is just a file
   sitting on the stick, in any folder
   you like.

When a computer boots from the stick,
Ventoy starts first, looks at the
files it finds, and offers them as a
menu. Pick one, and Ventoy hands over
to that file's own boot machinery —
the image boots *as itself*, unmodified,
which is why the same stick can also
hold your ordinary files, and why
"making a new bootable stick" becomes
"downloading a file".

## The three file families it boots,
and one trick

- **ISO files** — installers and live
  systems for Linux, Windows, BSD and
  more. The everyday case.
- **WIM and IMG files** — Windows
  installation images and raw disk
  images, booted through helpers Ventoy
  carries (chapter 6).
- **VHD/VHDX and EFI files** — fixed
  virtual disks, and individual EFI
  boot programs.

The trick is **vlnk**: a tiny link file
on the stick that points at an image
stored on a computer's *internal* disk,
so the stick can boot images that are
not on the stick at all. Chapter 11
uses it in a recipe; the lab met its
limits early (transcript t09).

## What makes it different

**Nothing is extracted or converted.**
Burning tools rewrite the stick and
often unpack the image. Ventoy boots
the file as it sits, so a stick can
hold many images side by side — a 64 GB
stick carries a small library — and
deleting one is deleting a file.

**One stick serves both firmware
generations.** Old machines boot through
**BIOS** (also called legacy mode); new
ones through **UEFI**. A Ventoy stick
carries boot code for both and the
matching menu for each, so the same
stick works in the old desktop and the
new laptop (chapter 2).

**The stick stays a stick.** Its main
partition is an ordinary filesystem
(exFAT by default). Plug it into a
running computer and it is just a
drive with files on it — which is also
how you update your library, straight
from the file manager.

Check: after installing (chapter 4),
copy any installer ISO onto the stick
and reboot from it — the file appears
in Ventoy's menu by name. The lab's
transcripts show the tool that makes
the stick and the parts it is made of
(t00–t08); the boot itself is the one
step this guide's lab could not take,
and says so wherever it matters.

## Who it is for

Ventoy suits anyone who installs or
rescues computers more than once:
the person who keeps a household's
machines running and wants every
installer and rescue tool in a drawer
on one stick; the technician who meets
a different machine every day; the
curious, who want to try six Linux
distributions without sacrificing six
sticks. The common thread is *more
than one image, more than once* —
for a single install on a single day,
the burning tools in chapter 12 are
just as good.

## What it is not

Ventoy is not an operating system:
its menu boots other things and then
gets out of the way. It is not an
installer for the systems it boots —
it starts Ubuntu's installer, it does
not install Ubuntu. It is not a backup
tool, although a stick full of rescue
images is a fine thing to have on the
day you need one. And it is not magic:
some images refuse to boot this way,
and chapter 13 is honest about that.
