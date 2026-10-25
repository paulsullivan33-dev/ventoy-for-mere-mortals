# Installing Ventoy

This is the one destructive chapter.
Installing Ventoy **repartitions the
whole device you point it at** — every
file on it goes. Read the device name
twice. Everything after this chapter
is harmless file copying; this one is
not.

*Provenance: the lab's sandbox had no
disk it was allowed to write — no
spare stick, no loop device — so no
installation transcript exists. What
follows is the tool's own documented
behaviour, cross-checked against
everything the lab ran around the
install (the installer's usage text,
its device validation, and its web
interface, all in the transcripts).
It is labelled documented-only here
and in `criteria.md`.*

## Before you start

1. **Identify the stick's device.**
   On Linux, plug it in and run
   `lsblk`: the stick is the disk whose
   size matches, mounted (if at all)
   under your media folder. Note the
   *disk* name — `/dev/sdb`, not the
   partition `/dev/sdb1`. On Windows,
   the installer lists removable
   devices by letter and size.
2. **Unplug other removable disks** if
   you can. The cheapest safety feature
   is having only one plausible target.
3. **Back up anything on the stick**
   you would miss. There is no undo.

## With the graphical tool (Windows
and Linux)

Run `Ventoy2Disk.exe`, or on Linux
`VentoyGUI.x86_64` from the extracted
package (or double-click it). The
window offers:

- **Device** — the stick, chosen from
  a list. Check the size column against
  your stick.
- **Install** — writes Ventoy and
  repartitions. The tool asks you to
  confirm **twice**, and both times it
  means it.
- **Update** — for a stick that
  already has Ventoy (chapter 7).
- Under **Option**: *Secure Boot
  Support* (on by default; chapter 13),
  *Partition Style* (MBR, or GPT for
  very large sticks and picky firmware),
  *Partition Configuration* (reserve
  space at the end of the disk, and
  the first partition's filesystem and
  label).

The Linux GUI is the same program in
a smaller coat; the lab's sandbox had
no display for it, so it is documented
here rather than shown (transcript
t10).

## From the command line (Linux)

The script's own usage, captured live
by the lab from version 1.1.18 —
this part is verified text:

```text
Usage:  Ventoy2Disk.sh CMD [ OPTION ] /dev/sdX
  CMD:
   -i  install Ventoy to sdX (fails if disk already installed with Ventoy)
   -I  force install Ventoy to sdX (no matter if installed or not)
   -u  update Ventoy in sdX
   -l  list Ventoy information in sdX

  OPTION: (optional)
   -r SIZE_MiB preserve some space (MiB) at the bottom of the disk (only for install)
   -s/-S       enable/disable secure boot support (default is enabled)
   -g          use GPT partition style, default is MBR (only for install)
   -L          Label of the 1st exfat partition (default is Ventoy)
   -n          try non-destructive installation (only for install)
```

So the ordinary install, as root:

```bash
sudo bash Ventoy2Disk.sh -i /dev/sdX    # your stick's device
```

`-i` refuses a disk that already has
Ventoy (use `-u`, chapter 7); `-I` is
the same install with the questions
removed — treat it with the respect
that deserves. The script checks its
target before anything destructive:
pointed at a device that does not
exist, the lab got

```text
/dev/ventoydoesnotexist is NOT a valid device
```

and a clean exit (transcript t02). It
is a check, not a safeguard to lean
on: an *existing* wrong device passes
it just fine.

Non-destructive install (`-n`) tries
to add Ventoy while keeping an
existing first partition and its
files, within documented conditions
(the partition's filesystem and
position must suit). It is the
exception, not the way in: when it
declines, the answer is a spare stick
and the ordinary install, not force.

## Through the web page (headless
Linux)

On a machine with no desktop, the
same installer runs as a local web
service:

```bash
sudo bash VentoyWeb.sh
# then browse to http://127.0.0.1:24680
```

The lab started this server for real.
Its page is the installer's own
interface, and its API answered real
questions: version 1.1.18 reported
itself, and the device list came back
empty — the sandbox offers it no
devices, which in a machine with a
stick plugged in is the list you
choose from (transcript t03). The
buttons map one-to-one onto the
graphical tool's: Install, Update,
and the same options.

## After installing

The stick now shows up, in any
operating system, as a drive labelled
**Ventoy** with the whole of its space
minus 32 MB free. That is the finished
product. No Ventoy program needs to be
running for the stick to work; the
magic is all on the stick itself.
Chapter 5 puts it to work.
