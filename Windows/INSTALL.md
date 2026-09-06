# CoderMax for Windows

Version 0.2.0. One self-contained program: no Node.js, no npm, no
administrator rights, and nothing else to download.

Each download is a compressed archive holding exactly one file, the program
itself, with no folder around it.

| Machine | Download | Program inside |
|---------|----------|----------------|
| x86-64 | `codermax-win-x64.zip` | `codermax-win-x64.exe` |

## Install

Download the archive above, right-click it and choose **Extract All**. That
leaves `codermax-win-x64.exe` in the folder. Run it once from a
terminal opened there:

```
.\codermax-win-x64.exe
```

That first run **is** the installation. It copies CoderMax into a private
per-user folder, registers a service that starts at login, puts a `codermax`
command on your PATH and asks for your licence key. Every later run checks that
install and repairs anything missing, and running a newer build upgrades the
installed copy. Open a new terminal afterwards so `codermax` is on your PATH.

`--help` and `--version` never install anything. To turn the automatic check
off entirely, set `CODERMAX_NO_AUTOINSTALL=1`.

## Check your download

`SHA256SUMS.txt` carries two lines for every build: the archive exactly as you
downloaded it, and the program inside it. Check the archive before you unpack,
and both afterwards.

```
certutil -hashfile codermax-win-x64.zip SHA256
certutil -hashfile codermax-win-x64.exe SHA256
```

Compare each answer with the matching line in `SHA256SUMS.txt`.

## Uninstall

```
codermax uninstall
```

That removes the installed copy, the login service and the PATH entry, and
releases the licence seat so you can activate on another machine.
