# Updating Ventoy

Ventoy improves steadily — new images
that did not boot start booting, new
firmware quirks get worked around.
Updating the stick is a designed-for
operation, and the design is the
chapter 2 split: the boot machinery
lives in its own small partition and
the boot code, your images live on
the big one. An update rewrites the
machinery and **leaves every image
and file of yours untouched**.

*Like the installation, updating a
real stick is documented-only in this
guide: the lab had no Ventoy stick to
update. The commands below are the
tool's own, cross-checked against the
usage text the lab captured live.*

## Check what a stick is running

On Linux, with the stick plugged in:

```bash
sudo bash Ventoy2Disk.sh -l /dev/sdX
```

lists the Ventoy version installed on
that device. The graphical tool shows
the same information in its window —
current version on the stick beside
the version you are running — and the
stick itself can tell you too: the
boot menu's footer shows its version
when you start from it.

## Update

- **Graphical:** select the stick and
  press **Update** instead of Install.
- **Command line:**

```bash
sudo bash Ventoy2Disk.sh -u /dev/sdX
```

- **Web:** the same Update button in
  the page on port 24680 (chapter 4).

Update keeps your images, your
folders, your `ventoy.json` and any
persistence files. What it cannot
keep is a hand-modified `VTOYEFI`
partition — if you have been editing
the machinery by hand, you already
know, and this sentence is your
warning.

## How often?

When a new image refuses to boot,
when you make a new stick anyway, or
when a release mentions a fix for a
machine you own. There is no harm in
a stick staying a few versions behind
if everything on it boots; there is
also no prize. Reading the project's
release notes is the honest way to
decide — they are short, and they
name what was fixed.

## Starting over instead

If a stick's layout itself is the
problem — wrong partition style, a
reserve you no longer want — copy
your images off, run a fresh install
(chapter 4), and copy them back.
Ventoy keeps no state anywhere else,
so a clean reinstall loses nothing
but the time the copies take. To
remove Ventoy from a stick entirely,
repartition it with any disk tool
(a single ordinary partition, any
filesystem): the stick becomes a
plain drive again, and nothing of
Ventoy survives outside its two
partitions.
