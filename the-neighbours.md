# The Neighbours

Ventoy is not the only way to get an
image onto a stick. The honest
comparison, by the job each is for.

## Rufus (Windows)

The classic single-image writer.
Rufus takes one ISO and makes one
bootable stick, expertly — with
extras Ventoy does not offer, like
writing in "DD mode", downloading
Windows ISOs for you, and building
Windows To Go sticks (a whole
Windows on the stick, not just its
installer). If you are making one
stick for one install, Rufus is at
least as good. What it does not do
is a library: the next image costs
the stick again.

## balenaEtcher (Windows, macOS,
Linux)

The friendly writer: pick image,
pick drive, flash, with verification
afterwards and a near-total refusal
to let you choose your system disk
by mistake. The safest hands for a
first-timer making a single-purpose
stick — a Raspberry Pi card, a
one-off installer. Same trade as
Rufus: one stick, one image, no
memory of the last one.

## YUMI and Easy2Boot

Ventoy's true relatives: multiboot
sticks you add files to. YUMI is the
older Windows-side tool, with a
guided list of distributions it
knows. Easy2Boot is the veteran —
more configurable, able to boot some
images Ventoy struggles with, and
famously more demanding of its user.
Between the three, Ventoy's edge is
the plain one this guide is built
on: no conversion, no special
layout, files are files.

## Writing the image yourself (`dd`)

On Linux, `dd if=image.iso
of=/dev/sdX` makes a bootable stick
from most modern Linux ISOs, because
they are built as hybrid images —
both CD and disk at once. It is
free, instant and entirely without
safety rails: name the wrong output
device and you have overwritten a
disk with an installer, a mistake
every guide including this one
warns about because everyone makes
it exactly once. Single image,
single stick, gone when rewritten —
but a fine way to make Ventoy's own
LiveCD bootable (chapter 3).

## Booting from the network (PXE,
netboot.xyz)

A machine with no working stick at
all can boot an installer over the
network, if something on the network
serves one. netboot.xyz is the
delightful end of this idea — one
small boot file that pulls up a menu
of everything. It needs a network
set up to offer it, which is another
subject; keep it in mind for the day
the machine in front of you has a
dead USB controller and a live
network port.

## When to choose which

| Your situation | The tool |
|---|---|
| One install, one time, safest path | balenaEtcher |
| One Windows stick with all the options | Rufus |
| A pocket library for many machines | **Ventoy** |
| The image Ventoy will not boot | Easy2Boot, or write it alone with Rufus/Etcher |
| No USB at all, a willing network | PXE / netboot.xyz |

Nothing here is exclusive. The
technician's drawer holds a Ventoy
stick *and* knows where Etcher is;
the library carries the everyday
ninety percent, and the writers
handle the stubborn remainder —
chapter 13 is about exactly that
remainder.
