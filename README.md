# Ventoy for Mere Mortals

Installing a computer used to mean making
a **bootable USB stick**: download an
installer's disk image (an **ISO file**),
run a special tool that writes the image
onto a stick in just the right way, boot
from it, install, and then start over
with a different stick — or the same
stick wiped — for the next image.
**Ventoy** ends that cycle. You run its
installer on a USB stick *once*. From
then on, adding something bootable is
copying a file: drop ISO, WIM, IMG, VHD
or EFI files onto the stick with your
ordinary file manager, in whatever
folders you like. When a machine boots
from the stick, Ventoy shows a menu of
the files it found and boots the one you
pick, directly from the file, unmodified.
This guide explains how that works, how
to make the stick, how to organise and
configure it, how to give live systems a
place to save their work, and where the
limits honestly are.

The lab behind this guide worked with
the real release: **Ventoy 1.1.18**, the
current version, downloaded from the
project's official release page and
checked against the project's published
SHA-256 sums before anything ran. On a
Linux machine the package's own parts
were exercised one by one: the command
line installer's usage text and its
refusal of a device that does not exist;
the web installer, started for real and
asked real questions through its own API;
the configuration helper's usage; the
sample `ventoy.json` that ships in the
package, validated; the persistence
image script, run up to the exact step
this machine could not do; and the boot
payload itself — the master boot record,
the GRUB core image, and the little
second partition every Ventoy stick
carries — taken apart and listed file
by file. The companion LiveCD image
was downloaded, checksummed and
inventoried too.

What the lab could not do, it did not
fake. The lab machine is a restricted
sandbox: it has **no loop devices** and
no spare disk of any kind, and its rules
forbid creating device files. Ventoy's
installer writes to a whole disk, and
there was no disk — real or safely
pretend — to write to. So the one thing
this guide does not contain is a
transcript of an installation, or of a
machine booting from the finished stick.
Those steps are taught carefully from
the tool's own behaviour and the
project's documentation, and labelled
as documented-only wherever they appear:
**the installation itself, booting,
the Secure Boot key enrolment, updating
a real stick, and persistence used by a
real live system.** `criteria.md` keeps
the full ledger of what was run and
what was only read.

## What this guide covers

- [**What Ventoy is**](what-ventoy-is.md) —
  the mental model: a stick that is a
  menu of files, not a written image;
  what Ventoy is not (a burning tool, an
  operating system, a hard-disk tool)
- [**The moving parts**](the-moving-parts.md) —
  the two partitions, the boot code for
  old and new firmware, the GRUB menu,
  and the helper programs in the box
- [**Getting Ventoy**](getting-ventoy.md) —
  the official download, checking the
  checksums, choosing a stick, and the
  Windows, Linux and LiveCD forms the
  tool comes in
- [**Installing Ventoy**](installing-ventoy.md) —
  putting Ventoy on a stick with the
  graphical tool, the command line and
  the web page; the options that matter;
  and the warning that install erases
- [**Your first boot**](your-first-boot.md) —
  copying an image on, booting a machine
  from the stick, reading the menu, and
  the keys that change how a file boots
- [**Adding and organising images**](adding-and-organising-images.md) —
  the file types Ventoy boots, folders
  as menus, file names that behave, and
  the stick as ordinary storage besides
- [**Updating Ventoy**](updating-ventoy.md) —
  updating the boot code while your
  images stay put, checking the version
  on a stick, and starting over cleanly
- [**Configuration**](configuration.md) —
  `ventoy.json`: menu timeouts and
  defaults, aliases and icons, passwords,
  and the Plugson helper that edits it
  in a browser
- [**Persistence**](persistence.md) —
  giving a live system a notebook: a
  backend file it can write to, the
  labels each family of systems expects,
  and how Ventoy joins the two together
- [**Answer files and injection**](answer-files-and-injection.md) —
  unattended installations: answer files
  attached to an image, and extra files
  slipped into a Windows install
- [**Recipes**](recipes.md) — the
  technician's stick, the rescue stick,
  an offline installer shelf, and a
  stick that boots the images on a
  computer's own disk
- [**The neighbours**](the-neighbours.md) —
  Rufus, balenaEtcher, YUMI, Easy2Boot,
  writing images with `dd`, and PXE
  booting: what each trades away
- [**Limits**](limits.md) — not every
  image boots, Secure Boot has a toll
  booth, one stick is one point of
  failure, stated plainly
- [**Troubleshooting**](troubleshooting.md) —
  the stick the firmware cannot see, the
  image that is not in the menu, the
  Secure Boot refusal and its remedy,
  and the errors this guide's lab met
- [**Glossary**](glossary.md) — the
  terms, collected in one place

## What it doesn't cover

Installing the operating systems
themselves. Ventoy gets an installer or
a live system started; what Ubuntu,
Windows or a rescue disk does after
that is its own subject, with its own
books. Making your own bootable images,
repairing a damaged stick's filesystem
byte by byte, and booting Ventoy itself
from a network are likewise pointed at,
not taught. Installing Ventoy onto an
internal hard disk is mentioned only to
warn about it: the tool is meant for
removable media, and its installer does
not care which disk you point it at.

## How to use this guide

Read chapter 1 for the model and
chapter 2 before installing anything —
knowing which partition holds what turns
later confusion into a named problem.
Chapter 3 gets Ventoy onto your machine
and a stick chosen; chapter 4 makes the
stick, and chapter 5 boots it in the
same sitting. Chapter 8 (configuration)
and chapter 9 (persistence) are where
a plain stick becomes *your* stick;
chapter 11 is the one to reread when
something is already going wrong. The
lab's transcripts live in
`ventoy-lab/transcripts/`; its notes
are `examples/lab-notes.txt`.

---

## Part of the series

This guide is one of the [For Mere Mortals](https://paulsullivan33-dev.github.io/mere-mortals/) guides: plain-language guides to the tools programmers and makers use.

Related guides:

- [Clonezilla for Mere Mortals](https://paulsullivan33-dev.github.io/clonezilla-for-mere-mortals/)
- [Linux for Mere Mortals](https://paulsullivan33-dev.github.io/linux-for-mere-mortals/)
- [Virtual Machines for Mere Mortals](https://paulsullivan33-dev.github.io/virtual-machines-for-mere-mortals/)

## License

The writing in this guide is licensed under [CC BY 4.0](https://github.com/paulsullivan33-dev/ventoy-for-mere-mortals/blob/main/LICENSE). Code examples and lab scripts are licensed under the [MIT License](https://github.com/paulsullivan33-dev/ventoy-for-mere-mortals/blob/main/LICENSE-CODE).
