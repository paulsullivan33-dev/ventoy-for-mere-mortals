# Getting Ventoy

Ventoy is free, open-source software
(GPLv3). Get it from the project's own
site, **ventoy.net**, which links to
its releases on GitHub — and nowhere
else. Lookalike download portals bundle
installers with advertising and worse;
the project publishes checksums
precisely so you can tell the real
thing from a repack.

## The downloads, by your computer

| Your machine | File | What you get |
|---|---|---|
| Windows | `ventoy-<version>-windows.zip` | the graphical installer, `Ventoy2Disk.exe` |
| Linux | `ventoy-<version>-linux.tar.gz` | scripts and a small GUI: `Ventoy2Disk.sh`, `VentoyGUI.<arch>`, `VentoyWeb.sh`, `VentoyPlugson.sh` |
| No working system | `ventoy-<version>-livecd.iso` | a bootable CD/USB image that runs the graphical installer |

The lab used the Linux package of
**Ventoy 1.1.18** (the current release
when the guide was written): 20 MB,
containing the tools listed in
chapter 2 for four processor families
(transcript t00).

Check: the release page also offers
`sha256.txt`. The lab's copy of the
Linux package summed to exactly the
published value before anything in it
was run:

```text
$ sha256sum ventoy-1.1.18-linux.tar.gz
d86ff9de63d94c8b8f1f6b14d23c55af4b88e8207bf2893890892a6a97b1ad54
```

Do the same with yours — on Windows,
`certutil -hashfile <file> SHA256`;
on Linux, `sha256sum <file>`; on a
Mac, `shasum -a 256 <file>` — and
compare with the published line for
your file. A mismatch means the
download is not the project's: delete
it and fetch it again from the
official page.

## Choosing a stick

- **Size is your library's size.**
  Ventoy itself needs almost nothing
  (chapter 2's 32 MB). The stick's
  size is the sum of the images you
  want at hand: one Linux installer is
  1–6 GB, a Windows installer 5–9 GB.
  32 GB is a sensible minimum; 64 GB
  carries a working library.
- **Speed matters more than brand
  claims.** Booting reads the whole
  image's early life from the stick.
  A stick that advertises USB 3 and a
  real read speed above ~100 MB/s
  makes installs noticeably less
  patient-testing. Bargain-bin sticks
  with fake capacities exist; buy from
  a maker whose name you recognise,
  and treat a too-cheap 1 TB stick as
  fiction until proven otherwise.
- **It will be erased.** Installation
  repartitions the whole device
  (chapter 4). Use a stick whose
  current contents you have elsewhere,
  or nowhere.

## A note on the LiveCD

The LiveCD is the escape hatch: a
small bootable system whose only job
is running the Ventoy installer, for
the machine whose own system is the
thing you are trying to fix. The lab
downloaded the 1.1.18 LiveCD image
(196 MB), verified its checksum the
same way, and inventoried what is
inside it; making a machine boot it
is itself a small burning job with
the tools in chapter 12.
