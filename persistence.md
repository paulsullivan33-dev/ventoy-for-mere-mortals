# Persistence

Boot a live system from a stick and
it forgets everything when you shut
it down: files saved, settings
changed, software installed — gone,
because the image it booted from is
read-only by design. **Persistence**
gives a chosen live system a place
to write: a separate **backend file**
on the stick whose contents the
live system wears as its own
writable layer. Your documents and
settings survive from boot to boot;
the image itself stays pristine.

*Provenance: the lab built and
inspected a backend file for real —
see "Making the file", below — but
no live system ever booted with one
in the sandbox, so the behaviour of
a running system is documented-only
in this chapter and in `criteria.md`.*

## The three pieces

1. **The backend file.** A plain
   file, usually named
   `persistence.dat`, holding a Linux
   filesystem (ext4 by default) with
   a **label** the live system knows
   to look for. It sits anywhere on
   the stick's first partition.
2. **The mapping.** A `persistence`
   section in `ventoy.json`
   (chapter 8) that says "when this
   image boots, offer it this backend
   file":

   ```json
   "persistence": [
       {
           "image": "/linux/ubuntu-live.iso",
           "backend": "/persistence.dat"
       }
   ]
   ```

   One image can list several backend
   files (the menu asks which), and
   several images can share one —
   carefully: two systems writing to
   one writable layer will treat each
   other's files as their own.
3. **The label.** Each family of live
   systems hunts for its own label:

   | Live system family | Label to use |
   |---|---|
   | Ubuntu and its flavours | `casper-rw` |
   | Debian live | `persistence`, plus a `persistence.conf` inside the file containing `/ union` |
   | MX Linux / antiX | `MX-Persist` / `antiX-Persist` |

   A wrong label is not an error —
   the system simply boots as if no
   backend existed. Chapter 14 has the
   symptom in its table.

## Making the file

The package's helper, run as root,
makes a backend file in one step:

```bash
bash CreatePersistentImg.sh                 # 1 GB, ext4, label casper-rw
bash CreatePersistentImg.sh -s 4096          # 4 GB
bash CreatePersistentImg.sh -l MX-Persist    # for MX Linux
bash CreatePersistentImg.sh -s 2048 -c persistence.conf   # Debian-style, config included
```

and `ExtendPersistentImg.sh
persistence.dat 2048` grows an
existing file later, keeping its
contents.

The lab ran this script for real —
and it is a tidy summary of the whole
guide's situation. The script wrote
out the image file, then reached for
a **loop device** to format it
through, and the sandbox has none:
the run stops there (transcript t05).
On an ordinary Linux machine that
step is unremarkable and the script
finishes; loop devices are how Linux
formats and mounts file-shaped
disks, and their absence is the
sandbox's quirk, not the script's.

What the script *produces*, the lab
verified directly, by performing its
final steps against a file with
`mkfs.ext4` itself (no loop device
needed for that) and reading the
result back without mounting:

```text
$ blkid persistence.dat
persistence.dat: LABEL="casper-rw" TYPE="ext4" ...

$ debugfs -R "cat /persistence.conf" persistence.dat
/ union
```

(transcript t06). A backend file is
exactly that: an ext4 filesystem in
a file, wearing the right label,
optionally carrying its one-line
config. Nothing about it is Ventoy
magic — which is why the next
sentence is safe to say.

Check: at the boot menu, an image
with persistence gets a note in its
entry (and a choice, if it has
several backends). Inside the booted
system, the change you make — a
file on the desktop, a setting —
should still be there after a
reboot of the same image with the
same backend. That check is yours to
run; it needs a machine, a stick,
and a reboot this guide's lab did
not have.

## Honest cautions

- **It is a wear item's spare tyre.**
  A writable layer on a cheap stick,
  written constantly by a running
  system, is the fastest way to wear
  the stick out. Fine for a rescue
  system's notebook; poor hosting for
  a daily system.
- **Backends are per-image in
  practice.** A Debian system and an
  Ubuntu system sharing one backend
  will not corrupt each other on
  purpose, but neither will make
  sense of the other's overlay. Give
  each system its own file.
- **Encryption is possible** (the
  helper's `-e` option) and supported
  by few live systems; the default,
  plain file is the documented road.
- **Size is hard to shrink**, easy
  to grow (`ExtendPersistentImg.sh`).
  Start smaller than you think.
