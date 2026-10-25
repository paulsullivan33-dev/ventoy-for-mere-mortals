# Limits

What Ventoy cannot do, and what it
costs, stated plainly.

## Not every image boots

Ventoy boots images by understanding
their innards, and the catalogue of
things it understands is long — the
project's tested list runs to over a
thousand images — but it is not
everything. The honest failure modes:

- **An image format from before the
  catalogue.** Old or exotic installers
  (and some commercial rescue disks
  with unusual boot machinery) may
  start and stall, or refuse outright.
  Memdisk mode (F1) rescues small
  ones; large ones may have no route.
- **Images that demand to be the
  whole disk.** A few systems check
  their medium in ways a file on
  someone else's partition cannot
  satisfy.
- **Anti-tamper designs.** A handful
  of installers verify their media
  and object, on principle, to being
  a guest.

The project's compatibility lists
and issue tracker are the authority
here; check an unusual image there
before a trip depends on it. The
fallback is honourable and old:
write that one image to its own
stick with chapter 12's writers.

## Secure Boot has a toll booth

With Secure Boot on, firmware runs
only boot code signed by keys it
trusts. Ventoy's support for this is
on by default, and it works — with
one ceremony: the **first** boot on
a machine that has never seen
Ventoy's key stops with a
verification failure, and you must
enrol the key or one file's hash
through the **MOK Manager** screens
that Ventoy presents (the key file,
`ENROLL_THIS_KEY_IN_MOKMANAGER.cer`,
sits in the `VTOYEFI` partition —
the lab found it there, transcript
t08 — waiting for exactly this).
After that once, the machine boots
Ventoy sticks without ceremony.

Consequences worth knowing: on a
machine that is not yours, the
ceremony is a request for trust you
may not be positioned to make;
disabling Secure Boot in the
firmware is the other door, and it
is the firmware owner's call, not
yours by default. If you build a
stick for machines where neither is
acceptable, install with secure-boot
support off (`-S`) and accept that
Secure Boot machines will refuse it.

## One stick is one point of failure

The library is only as safe as the
stick. Sticks corrupt, walk away,
and wear out — persistence backends
(chapter 9) fastest of all. Keep the
*source* images somewhere else too;
a Ventoy stick is a copy of your
library, and should never be the
only one. Rebuilding a stick from
the shelf takes minutes (chapters
4 and 6), which is the real reason
not to grieve one.

## What the menu password is not

Chapter 8's password keeps the menu
and chosen images behind a prompt.
It is a hash checked by Ventoy's
own menu — it does not encrypt the
stick. Anyone with the stick and a
computer can read every file on its
first partition, answer files
(chapter 10) included. Plan
accordingly.

## Ventoy is not for internal disks

The installer will write to any disk
you name — including a computer's
internal drive, erasing it with the
same efficiency it erases a stick.
Nothing about the tool *wants* the
internal disk; a few people install
it there deliberately as a boot
menu, which is expert territory
with the safety off. This guide's
position is the plain one: removable
media are the tool's home, and the
device name deserves its second
look (chapter 4).

## What this guide could not test

Restated from `criteria.md`, because
a limits chapter is where it
belongs: the lab's sandbox had no
disk it could write to — no spare
stick, no loop devices — so the
installation itself, booting from a
finished stick, updating one, the
Secure Boot enrolment, Plugson's
pages against a real stick, and a
live system actually using a
persistence backend are taught from
the project's documentation and the
tool's observed behaviour, and
labelled so wherever they appear.
Everything around those steps —
downloading and checksumming, the
installer's usage and refusals, the
web installer's server and API, the
configuration file's shape, the
persistence file's structure, and
the boot payload taken apart file
by file — was run and read for
real. Your bench almost certainly
allows the blocked list; the
sandbox's refusals are its own.
