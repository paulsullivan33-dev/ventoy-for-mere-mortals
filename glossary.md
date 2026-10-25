# Glossary

**Answer file** — a file that answers
an installer's questions in advance
(unattended installation). Windows
calls its format an unattended XML;
Debian uses preseed or autoinstall;
Red Hat family, kickstart. Chapter 10.

**Backend file** — the file a live
system writes its changes into under
persistence. An ordinary file holding
a Linux filesystem with a magic
label. Chapter 9.

**BIOS (legacy mode)** — the older
firmware style: starts a disk by
reading its master boot record.
Modern machines also offer it as a
compatibility mode. Chapter 2.

**Boot menu (firmware)** — the
machine's one-time choice of what to
start from, usually a key like F12
at power-on. Distinct from Ventoy's
own menu, which appears after it.

**Checksum (SHA-256)** — a short
fingerprint computed from a file.
If yours matches the published one,
the file is the file. Chapter 3.

**Contiguous** — stored in one
unbroken run of blocks, not scattered
as fragments. Some boot modes need an
image contiguous. Chapter 14.

**exFAT** — the filesystem of a
Ventoy stick's first partition:
readable everywhere, no 4 GB file
limit. Chapter 2.

**GRUB** — the bootloader Ventoy's
menu is built on. Its core image
does the legacy booting; its
configuration language powers themes
and custom menus. Chapters 2, 8.

**Hybrid image** — an ISO built to
be both a CD image and a disk image,
so it can be written raw to a stick
and still boot. Chapter 12.

**Injection** — Ventoy's plugin that
unpacks an archive into a Windows
install environment as it boots —
drivers and scripts, delivered.
Chapter 10.

**ISO file** — a disk image,
originally of a CD or DVD, now the
universal shape of installers and
live systems.

**Label** — a filesystem's name,
readable by software hunting for it:
`Ventoy` for the data partition,
`VTOYEFI` for the boot partition,
`casper-rw` and friends for
persistence backends. Chapters 2, 9.

**Live system** — an operating system
that runs from the image itself,
without being installed. Forgets
everything at shutdown unless
persistence gives it a backend.
Chapter 9.

**Loop device** — Linux's way of
treating a file as if it were a
disk. The persistence helper needs
one; this guide's sandbox had none,
which is why transcripts t05 reads
as it does.

**MBR / GPT** — the two partition
table styles: MBR the older default,
GPT the modern one, chosen at
install time with `-g`. Chapter 4.

**Memdisk mode** — booting an image
by loading it wholly into memory
first (the menu's F1). A rescue for
small, stubborn images. Chapter 5.

**MOK Manager** — the screens UEFI
firmware shows for enrolling a
Machine Owner Key: the ceremony that
lets Secure Boot accept Ventoy's
signature. Chapter 13.

**Persistence** — a live system's
ability to keep changes between
boots, via a backend file. Chapter 9.

**Plugson** — Ventoy's browser-based
configuration editor, which writes
`ventoy.json` for you. Chapter 8.

**Secure Boot** — UEFI's rule that
boot code must be signed by a trusted
key. Ventoy supports it, with the
one-time enrolment of chapter 13.

**UEFI** — the modern firmware.
Starts boot programs as files —
`EFI/BOOT/BOOTX64.EFI` — from a FAT
partition such as `VTOYEFI`.
Chapter 2.

**ventoy.json** — the stick's one
configuration file, in the `ventoy`
folder of its first partition:
menus, aliases, passwords,
persistence, answer files.
Chapter 8.

**vlnk** — a small link file on a
stick that points at an image on a
computer's internal disk, letting
the stick boot images it does not
carry. Chapters 1, 11.

**WIM** — Windows Imaging format: a
Windows installation in one file,
which Ventoy can boot directly.
Chapter 6.
