# Answer Files and Injection

An installer asks questions. An
**answer file** answers them in
advance — language, disk layout,
accounts, no questions asked — and
the install runs unattended. Ventoy
can attach an answer file to an
image it boots, and can slip extra
files into a Windows installation as
it goes. Both live in `ventoy.json`
(chapter 8), in the `/ventoy` folder
on the stick.

*These features act inside another
program's installer, on a machine
this guide's lab did not have. This
chapter is documented-only end to
end, and `criteria.md` says so.*

## Auto-install: the `auto_install`
section

```json
"auto_install": [
    {
        "image": "/windows/windows-11.iso",
        "template": "/ventoy/script/windows_unattended.xml"
    }
]
```

When an image with a template boots,
Ventoy offers the unattended run
(the menu shows it alongside the
ordinary boot, or asks, depending on
the template setup); the installer
finds its answers as if a local
answer file had been provided by the
usual means. What an answer file
says is the installer's own subject:

- **Windows** reads an XML answer
  file (`autounattend.xml`): edition,
  disk layout, local account, and —
  with the documented settings —
  skipping the out-of-box questions.
- **Debian-family Linux** reads a
  preseed file or, for Ubuntu's newer
  installer, an autoinstall YAML.
- **Red Hat family** reads a
  kickstart file.

Writing a good answer file is a
subject of its own; Ventoy's part is
only the delivery, and it is reliable
delivery: the template travels on the
stick, named per image, so one stick
can install a laptop's Windows and a
server's Linux, each with its own
answers. Give `template` a **list**
instead of one file and the boot menu
offers a choice between them — the
shipped example in
`ventoy-lab/fixtures/` does exactly
that for a Windows Server image with
six candidate files.

One caution that deserves bold:
**an answer file contains secrets by
nature** — account names, sometimes
passwords, licence keys. The stick
that carries it is no longer a
harmless thing to lend (chapter 13).

## Injection: the `injection`
section

Windows Setup accepts extra files —
drivers, scripts, a setup-complete
command — placed in known folders on
its installation media. Ventoy's
**injection** plugin fakes exactly
that for an ISO it boots from a
file:

```json
"injection": [
    {
        "image": "/windows/windows-11.iso",
        "archive": "/ventoy/driverpack.zip"
    }
]
```

The archive (`.zip`, `.rar`, `.7z`,
`.tar.gz` and kin) is unpacked into
the install environment as the
Windows image boots — the classic
use is a folder of storage or network
drivers for a machine whose disk the
installer would otherwise not see,
or scripts that run at the end of
setup. The archive's own layout must
match what Windows expects; Ventoy
delivers it faithfully and does not
check your packing for you.

## The two, together

Attaching answers and drivers is how
a technician's stick stops being a
menu and becomes a pipeline:
chapter 11's third recipe is built
from these two sections and nothing
else. Keep the pieces separate —
answers in `script/`, archives in
their own folder, the JSON mapping
on one screen — and a stick you built
a year ago stays readable at the
moment you most need it to be.
