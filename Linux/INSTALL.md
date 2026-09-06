# CoderMax for Linux

Version 0.2.0. One self-contained program: no Node.js, no npm, no
administrator rights, and nothing else to download.

Each download is a compressed archive holding exactly one file, the program
itself, with no folder around it.

| Machine | Download | Program inside |
|---------|----------|----------------|
| x86-64 | `codermax-linux-x64.tar.gz` | `codermax-linux-x64` |
| ARM64 | `codermax-linux-arm64.tar.gz` | `codermax-linux-arm64` |

## Install

Download the archive that matches your machine, unpack it, and run the
program once:

```
tar -xzf ./codermax-linux-x64.tar.gz
./codermax-linux-x64
```

The tar archive carries the executable bit, so there is nothing to `chmod`. If
your browser or file manager unpacked it for you and dropped that bit, run
`chmod +x ./codermax-linux-x64` first.

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
shasum -a 256 -c --ignore-missing SHA256SUMS.txt
shasum -a 256 -c SHA256SUMS.txt
```

The first command checks whatever is present and skips the rest, so it passes on
the archive alone. The second wants both files and is the one to run once you
have unpacked.

## Uninstall

```
codermax uninstall
```

That removes the installed copy, the login service and the PATH entry, and
releases the licence seat so you can activate on another machine.
