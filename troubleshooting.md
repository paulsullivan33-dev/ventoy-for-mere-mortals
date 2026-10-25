# Troubleshooting

In the order you will meet them:
getting the machine to start from the
stick at all, then Ventoy's menu,
then the image you picked.

## The machine does not offer the
stick in its boot menu

- **Try the other entry.** Many
  machines list a stick twice (plain
  and `UEFI:`). If one will not start,
  the other often will — the stick
  carries both kinds of boot code
  (chapter 2).
- **Try another port.** Front-panel
  and hub ports are the flaky ones;
  a rear port, straight into the
  machine, is the control experiment.
- **Check the stick in a running
  system.** It should mount as a
  drive named `Ventoy`. If it does
  not, the install never landed or
  the stick has died: reinstall
  (chapter 4) on another stick to
  separate the two suspects.
- **BIOS age and modes.** Some old
  firmware needs "USB HDD" boot
  enabled, or chokes on very large
  sticks; a smaller stick is a
  diagnostic, not a surrender.

## "Verification failed" / Access
Denied at first boot

That is Secure Boot meeting Ventoy
for the first time (chapter 13).
Either work through the MOK Manager
screens to enrol the key or the
hash, or — with the machine owner's
agreement — turn Secure Boot off in
the firmware. If a stick must serve
machines where neither is possible,
rebuild it with secure-boot support
off and use it on machines that
allow it.

## The machine boots, but not into
Ventoy's menu — a black screen or
an instant reboot

Usually firmware-meets-menu trouble,
not a dead stick:

- Switch the menu to **text mode**
  (chapter 8's `control` section,
  `VTOY_DEFAULT_MENU_MODE` and the
  theme's `display_mode` set to
  `CLI`): some firmware mishandles
  the graphical menu's resolution.
- On very old BIOS machines the
  project's `ventoy_4k` variant
  exists for disks with 4K sectors;
  a normal stick on such a machine
  is a known hard case — check the
  project's documentation for your
  model before fighting it.

## The image is not in the menu

| Symptom | Likely cause | Fix |
|---|---|---|
| A file you copied is missing | copied to the wrong partition, or the copy never finished | mount the stick; the file belongs on the big `Ventoy` partition; copy again and eject properly |
| Shown but with a strange name | macOS `._` shadow files | set `VTOY_FILT_DOT_UNDERSCORE_FILE` (chapter 8) and delete the shadows |
| Present on disk, never listed | a name Ventoy's scanner skips, or the file is inside the reserved `ventoy` folder | rename to plain ASCII, no spaces (chapter 6); keep images out of `/ventoy/` |

## The image starts, then stalls or
fails

- **Checksum it.** Ventoy's F5 tools
  can verify an image's sums at the
  menu; a bad download fails late
  and weirdly. Compare against the
  publisher's sums and re-download
  if they differ.
- **Memdisk mode (F1).** For small
  images that stall in normal mode.
- **Fragmentation.** An image copied
  onto a nearly full stick in pieces
  can fail in modes that need it
  contiguous; the F5 tools report
  fragmentation — copy it fresh
  onto an emptier stick.
- **Accept the verdict.** Some
  images do not boot under Ventoy
  (chapter 13). Check the project's
  tested list; give that one image a
  stick of its own via chapter 12.

## Persistence "does not work" — the
system boots but forgets

In order of likelihood: the backend
file's **label** does not match what
that family of systems looks for
(chapter 9's table — the usual
casualty is Ubuntu's `casper-rw`
versus Debian's `persistence`); the
`persistence` section in
`ventoy.json` names the image path
or backend path wrongly (paths start
at the partition root); the JSON is
invalid and silently ignored
(chapter 8's check); or the live
system's boot needs its own
"persistent" option chosen at *its*
menu, which some distributions ask
for explicitly. Nothing here errors
loudly — the system just boots
amnesiac — so change one thing at a
time.

## The installer says the device is
"NOT a valid device", or refuses

The lab met this one directly:

```text
/dev/ventoydoesnotexist is NOT a valid device
```

(transcript t02). The installer
checks that its target exists and is
a disk before it will do anything.
If your real stick gets the refusal:
the device name is the *partition*
(`/dev/sdb1`) rather than the disk
(`/dev/sdb`), the stick is not being
seen at all (`lsblk` will show no
such disk — a cable/port/stick
problem), or something has the
device mounted and busy, in which
case unmount its partitions first.
Two more honest refusals from the
lab's bench, same family: Plugson
prints its usage when given no
device (t04), and `VentoyVlnk.sh`
refuses images living on filesystems
it cannot support —

```text
btrfs is not supported!
```

(t09). The tool saying no, early and
in words, is a feature. Believe it
the first time.
