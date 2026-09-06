# CoderMax for macOS

Version 0.2.0. One self-contained program: no Node.js, no npm, no
administrator rights, and nothing else to download.

Each download is a compressed archive holding exactly one file, the program
itself, with no folder around it.

| Machine | Download | Program inside |
|---------|----------|----------------|
| Apple Silicon and Intel | `codermax-macos-universal.zip` | `codermax-macos-universal` |

## Install

Download the archive above and unpack it. Double-clicking it in Finder does
that, and so does the first line below. Then mark the program executable and
run it once:

```
ditto -x -k ./codermax-macos-universal.zip .
chmod +x ./codermax-macos-universal
./codermax-macos-universal
```

That first run **is** the installation. It copies CoderMax into a private
per-user folder, registers a service that starts at login, puts a `codermax`
command on your PATH and asks for your licence key. Every later run checks that
install and repairs anything missing, and running a newer build upgrades the
installed copy. Open a new terminal afterwards so `codermax` is on your PATH.

`--help` and `--version` never install anything. To turn the automatic check
off entirely, set `CODERMAX_NO_AUTOINSTALL=1`.

## Signed and notarised by Apple

The program inside the zip is signed with the SmartRobot PTY LTD Developer ID
certificate and has been notarised by Apple. There is no quarantine flag to
clear, no "Open Anyway" trip through System Settings, and nothing else to do:
download it, unpack it, mark it executable, run it.

The download is a `ditto` zip, which is the same kind of container the
notarisation was submitted from. Gatekeeper checks the ticket the first time
the unpacked program runs, and it asks Apple online, so that first run wants a
network connection and every run after it does not.

You can confirm all of that yourself before you run it:

```
codesign -dvv ./codermax-macos-universal
spctl --assess --type open --context context:primary-signature -vv ./codermax-macos-universal
```

`codesign` names the publisher and the team id. `spctl` should say
**accepted** with `source=Notarized Developer ID`; that is the exact check
macOS makes when it opens a downloaded file.

One answer looks alarming and is not: `spctl --assess --type execute` says
`rejected (the code is valid but does not seem to be an app)`. That is the
assessment type for an application bundle, and CoderMax is a single
command-line program rather than a `.app`. Note that it says the code IS
valid. Use the `--type open` command above instead.

Nothing is stapled to this file, because a bare executable has nowhere to hold
a ticket, so macOS checks it with Apple online instead. What Apple registered
is a CDHash per architecture, and `VERSION.json` in this folder lists both of
them next to the notarisation request id. Yours should match:

```
codesign -dvvv -a arm64 ./codermax-macos-universal 2>&1 | grep '^CDHash='
shasum -a 256 ./codermax-macos-universal
```

One file covers every Mac. It is a universal binary: an Apple Silicon Mac runs
the arm64 code and an Intel Mac runs the x86-64 code, natively, with no Rosetta
in between.

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
