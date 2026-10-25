# Writing Criteria

Every guide in this repo is written to the same
standard. If you're adding or editing a guide —
human or AI — hold it to these.

1. **Plain words, real ideas.** Simple language,
   never dumbed-down concepts. Explain jargon on
   first use; collect terms in the glossary.
2. **No hype.** No marketing adjectives, no
   "revolutionary," no promises about what
   something will do for the reader.
3. **Honest about limits.** Say what the thing
   *can't* do. State uncertainty instead of
   smoothing it over. Validation notes include
   what remains unverified.
4. **No invented numbers.** Benchmarks, defaults,
   and specs are either measured or qualified with
   "depends on your setup." If it wasn't tested,
   say so.
5. **Checkable.** Commands are meant to be run,
   with "Check:" blocks showing what success looks
   like. Sources are linked, not asserted.
6. **Respect the reader's setup.** Call out
   Windows/Mac/Linux differences. Don't assume a
   GPU, a budget, or prior experience.
7. **Safety isn't skipped.** Real risks get
   stated plainly, not buried.
8. **Scoped.** Each guide says what it covers and
   points elsewhere for the rest. No guide
   pretends to be the whole subject.
9. **Structure over prose.** Tables, before/after
   examples, symptom → fix. Scannable beats essay.

## Verification notes for this guide

Built and verified 2026-10-24/25 against
**Ventoy 1.1.18**, the current release,
downloaded from the project's official
release page (GitHub, ventoy/Ventoy) on
an x86_64 Ubuntu sandbox machine.

**The downloads were verified before
anything ran.** The Linux package
(`ventoy-1.1.18-linux.tar.gz`,
20,320,548 bytes) and the LiveCD image
(`ventoy-1.1.18-livecd.iso`,
195,989,504 bytes) both match the
SHA-256 sums in the release's official
`sha256.txt` exactly (transcripts
t00, t11).

**The sandbox, declared.** The lab
machine is a restricted sandbox. It has
**no loop devices** (`/dev` contains no
loop nodes, and creating device files
is refused even as root), no spare
block device of any kind, and no
display. Ventoy's installer writes to
a whole disk; there was no disk —
physical, virtual or loop-backed — it
could honestly be pointed at, and it
was never pointed at any real device.
That single fact decides this guide's
verification ceiling: everything about
the tool *around* the installation was
run for real; the installation and
everything downstream of a finished
stick is documented-only, labelled in
the chapters wherever it appears (4, 5,
7, 10, and parts of 8 and 9) and
restated in chapter 13.

**Ran for real** (transcripts in
`ventoy-lab/transcripts/`):

- t00 — release identification
  (v1.1.18), the checksum match, and
  the Linux package's inventory:
  scripts, per-architecture tools,
  the boot payload, the sample plugin
  files.
- t01 — `Ventoy2Disk.sh` with no
  arguments prints the installer's
  full usage text, including the
  `-l` (list), `-L` (label) and `-n`
  (non-destructive) options that the
  package README does not mention.
- t02 — the installer against a
  nonexistent device, for both `-i`
  and `-l`: `/dev/ventoydoesnotexist
  is NOT a valid device`, clean exit.
  Nothing was ever installed anywhere.
- t03 — `VentoyWeb.sh`'s server
  started and served for real on
  127.0.0.1:24680: the installer page
  itself (45,730 bytes of the tool's
  own HTML), a `sysinfo` API call
  answering with version 1.1.18 and a
  session token, a tokenless device
  request refused (`tokenerror`), and
  `get_dev_list` with the token
  returning an empty list — the
  sandbox offers the installer no
  target devices. The issuing session
  token is redacted in the transcript.
- t04 — `VentoyPlugson.sh` with no
  device prints its usage and exits
  (it requires the stick's device);
  the package's sample `ventoy.json`
  passes `python3 -m json.tool`. The
  sample is kept at
  `ventoy-lab/fixtures/ventoy.json.sample`,
  with the shipped `ventoy_grub.cfg`
  and theme examples beside it.
- t05 — `CreatePersistentImg.sh -s 64`
  run for real: it wrote the 64 MiB
  image file, then failed at
  `losetup` (no loop devices), and —
  kept as evidence — carried on into
  an `mkfs` against the empty device
  name before exiting. On a normal
  machine the loop step succeeds.
- t06 — the artefact that script
  produces, verified directly: a file
  formatted with `mkfs.ext4 -E
  nodiscard -L casper-rw` reports
  `LABEL="casper-rw" TYPE="ext4"` to
  `blkid`, and a `persistence.conf`
  containing `/ union` was written
  into it and read back with
  `debugfs`, unmounted. Labelled in
  chapter 9 as the script's final
  steps performed directly, not as
  the script's own run.
- t07 — payload identification:
  `boot/boot.img` is a DOS/MBR boot
  sector; `boot/core.img.xz`
  decompresses to a 1,048,064-byte
  GRUB core image; `ventoy/ventoy.disk.img.xz`
  decompresses to exactly
  33,554,432 bytes (32 MiB).
- t08 — that image opened with mtools:
  a FAT16 filesystem, label `VTOYEFI`,
  holding `grub/` (a 91,301-byte
  `grub.cfg`, per-distro configs,
  themes, modules for x86_64, i386,
  ARM64 and MIPS), `EFI/`, `ventoy/`,
  `tool/`, and
  `ENROLL_THIS_KEY_IN_MOKMANAGER.cer`
  (1,420 bytes) at its root — the
  Secure Boot key of chapter 13,
  present in the real payload.
  `boot.img`'s final two bytes are
  `55 aa` (od).
- t09 — `VentoyVlnk.sh -c` against a
  dummy image file on the sandbox's
  btrfs volume stops with
  `btrfs is not supported!` — the
  tool's filesystem check, exercised
  live.
- t10 — `VentoyGUI.x86_64` launched
  with no display exits immediately
  (code 1, no output). The GUI is
  therefore documented, not shown.
- t11 — the LiveCD opened with 7z:
  an El Torito ISO whose `EFI/boot`
  holds `bootx64.efi`, an 11 MB
  kernel and a 178 MB initrd carrying
  the live installer system.

**Documented only, and labelled as
such in the chapters:** installing
Ventoy onto any device (the sandbox
ceiling above); booting any machine
from a Ventoy stick, including the
Secure Boot MOK enrolment ceremony;
the menu's actual behaviour and
hotkeys in a live boot; updating a
real stick (`-u`); non-destructive
install (`-n`); the graphical
installer on Windows and Linux;
Plugson's pages against a real stick
(it was run only far enough to prove
it requires one); a live system
actually using a persistence backend
(the backend *file* was built and
verified; no system booted with it);
auto-install templates and injection
archives in a real installation
(chapter 10 in full); and vlnk files
actually booting (creation refused
the sandbox's filesystem, for the
recorded reason).

**Fixtures:** everything the lab made
is small and generic — a 23-byte dummy
image file for the vlnk test, a 64 MiB
persistence image built to be
inspected and deleted, and copies of
the package's own shipped sample files.
Nothing is modelled on any real
person, organisation or event.
