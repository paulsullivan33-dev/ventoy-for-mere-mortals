# Recipes

Four sticks worth making. Every
ingredient is a previous chapter;
nothing here needs anything new.

## 1. The technician's stick

For whoever fixes the machines.

- A 64 GB stick, Ventoy installed
  with the defaults (chapter 4).
- Folders per chapter 6: `linux/`
  (two distributions you know well),
  `windows/` (the current Windows
  installer), `rescue/` (a dedicated
  rescue live system, a memory
  tester `.efi`, a disk-cloning
  live system).
- A `tools/` folder of ordinary files
  beside the images — the stick is
  storage too.
- `menu_alias` entries so the menu
  reads like a workbench, not a
  directory listing (chapter 8).

## 2. The rescue stick that remembers

For one machine you care about,
kept in a drawer.

- A small stick — 16 GB is plenty.
- One live system you trust, chosen
  for fixing, not showing off.
- A persistence backend made for it
  (chapter 9), so drivers you fetch
  and notes you take while rescuing
  are still there at the next
  rescue. The Debian label rules
  from chapter 9's table apply.
- Write the machine's name on the
  stick with a marker. Future you,
  in a hurry, says thanks.

## 3. The offline installer shelf

For a machine (or a room of them)
with no network worth the name.

- The biggest honest stick you have,
  or a small USB hard disk —
  Ventoy installs on those too, with
  the same commands, and GPT
  (`-g`) suits large disks.
- Every installer you might need,
  one folder per system, each with
  its answer file attached
  (chapter 10) and, for Windows, a
  driver archive injected for the
  hardware you actually own.
- A `VTOY_MENU_TIMEOUT` in
  `ventoy.json`, if the shelf doubles
  as a hands-off re-imaging station.

## 4. Boot the images on the computer's own disk (vlnk)

A Ventoy stick can boot an image
that is not on the stick at all.
**vlnk** files are the pointers:

```bash
# on the machine that holds the image:
sudo bash VentoyVlnk.sh -c /path/to/image.iso
# produces image.iso.vlnk — copy it to the stick
```

The stick's menu treats the `.vlnk`
file like the image itself; at boot,
Ventoy follows the pointer to the
internal disk and boots that file.
The point: a big library that lives
on the machine, with the stick as a
universal key — or a rescue stick
that can boot the very disk images
the machine keeps for itself.

The lab met this tool's fine print
for real. `VentoyVlnk.sh -c` reads
where the image lives and refuses
filesystems it cannot promise to
find again at boot; offered a file
on this sandbox's btrfs volume, the
reply was plain:

```text
btrfs is not supported!
```

(transcript t09). ext-family, XFS,
NTFS, exFAT and FAT filesystems are
the supported ground — on a normal
machine's ordinary disk, that is
where your images already are.
