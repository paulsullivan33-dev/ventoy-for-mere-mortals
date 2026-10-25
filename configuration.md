# Configuration

A Ventoy stick works with no
configuration at all. When you want
one to behave *your* way — a menu
that times out into a default, nicer
names than file names, a password —
the settings live in one file:

```text
/ventoy/ventoy.json     on the stick's first partition
```

Create the `ventoy` folder (all
lower case) at the root of the drive
if it is not there, and put the file
in it. The settings travel with the
stick and apply in every machine.

## The shape of the file

`ventoy.json` is ordinary JSON: a set
of named sections, each a list of
small objects. The release package
ships an example — the lab kept a
copy in `ventoy-lab/fixtures/` and
verified it parses as valid JSON
(transcript t04) — and its bones are
these:

```json
{
    "control": [
        { "VTOY_DEFAULT_MENU_MODE": "1" },
        { "VTOY_FILT_DOT_UNDERSCORE_FILE": "1" }
    ],
    "theme": {
        "file": "/ventoy/theme/theme.txt",
        "display_mode": "GUI"
    },
    "menu_alias": [
        {
            "image": "/ISO/MX-19.1_x64.iso",
            "alias": "MX 19.1 ISO file For me"
        }
    ]
}
```

Two habits keep this file honest:
**edit it on a computer, not in the
boot menu** (there is no in-menu
editor), and **check it after
editing** — one missing comma makes
the whole file silently ignored, and
Ventoy falls back to defaults rather
than complain. Any JSON checker will
do; on Linux, `python3 -m json.tool
ventoy.json` prints the file back if
it is valid, and complains precisely
if it is not. (The lab ran exactly
that check on the shipped example.)

## The sections you will actually use

- **`control`** — switches and knobs
  as name/value pairs. The ones that
  earn their place: `VTOY_MENU_TIMEOUT`
  (seconds before the default entry
  boots by itself; leave it unset and
  the menu waits for you),
  `VTOY_DEFAULT_IMAGE` (which entry
  that default is),
  `VTOY_DEFAULT_MENU_MODE` (`0` list,
  `1` folder tree),
  `VTOY_WIN11_BYPASS_CHECK` (skip
  Windows 11's hardware checks during
  install), and
  `VTOY_FILT_DOT_UNDERSCORE_FILE`
  (hide the `._` files macOS scatters).
  The project's control-plugin page
  lists all of them; there are dozens,
  and most sticks need none.
- **`menu_alias`** — display names.
  The file stays `ubuntu-24.04.1.iso`;
  the menu can say *Ubuntu 24.04 LTS*.
  Paths in `ventoy.json` start at the
  partition root and use `/` — exactly
  as in the example above.
- **`menu_class`** — an icon for files
  whose name contains a key (`ubuntu`,
  `debian`…), or everything in a
  folder. Cosmetic, and genuinely
  useful on a crowded stick.
- **`theme`** — a GRUB theme folder
  under `/ventoy/theme/`, with a
  `theme.txt` describing the layout.
  The package ships a working sample
  theme in `plugin/ventoy/theme/` —
  the lab read its `theme.txt`: a
  background image, a menu box, a
  scroll bar, colours. Copy it onto
  the stick and edit from a real,
  working starting point.
- **`password`** — a password on the
  boot menu or on individual images,
  stored as a hash, not the password
  itself. It keeps honest people and
  curious children out; it is not
  encryption, and chapter 13 says so.
- **`persistence`, `auto_install`,
  `injection`** — the working
  sections: chapters 9 and 10.

## Plugson: editing in a browser

Hand-writing JSON is optional.
**Plugson** is the project's
configuration helper: plug in the
stick, run

```bash
sudo bash VentoyPlugson.sh /dev/sdX
```

and browse to `http://127.0.0.1:24681`.
Every section becomes forms — pick
images from the stick, set the
timeout, choose a theme — and Plugson
writes a correct `ventoy.json` to the
stick for you. The lab ran Plugson
far enough to confirm it insists on
being given the stick's device and
prints its usage without one
(transcript t04); with no stick in
the sandbox, the pages themselves
are documented-only here.

## Custom menus (F6)

Beside `ventoy.json`, the `ventoy`
folder can hold `ventoy_grub.cfg`:
your own GRUB menu entries, shown
when you press F6 at the boot menu.
The package ships an example (a
custom entry and a submenu, kept
with the sample files). This is the
deep end — GRUB's own language, able
to boot things Ventoy's scanner does
not know about — and most sticks
never need it.
