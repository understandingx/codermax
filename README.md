# CoderMax

**The coding agent you own, in your terminal.**

CoderMax is an AI coding agent that runs in your terminal. You type a request, it
streams the model's answer back as it arrives, and it works on your codebase through
a small fixed set of built-in tools: reading and writing files, applying patches,
searching code, and running shell commands behind a safety gate. It ships as one
self-contained binary that installs itself on first run, works on the directory you
launch it from, and saves every session to disk so you can close the terminal and
pick the same conversation up later.

Sixteen model providers are supported with your own API key, and you can switch
vendor or model in the middle of a session without losing your transcript. One
licence is **$25 a year** and covers **three machines**.

This repository holds the ready to run CoderMax builds, one folder per operating
system. There is no source code here: each file is the whole product, runtime
included. Buy a licence, read the documentation and manage your account at
<https://codermax.laxtic.com>.

---

## Contents

1. [What you get](#what-you-get)
2. [Requirements](#requirements)
3. [Download](#download)
4. [Install](#install)
5. [First run and activation](#first-run-and-activation)
6. [Everyday use](#everyday-use)
7. [Where your files live](#where-your-files-live)
8. [Updating](#updating)
9. [Uninstalling](#uninstalling)
10. [Privacy and security](#privacy-and-security)
11. [Pricing](#pricing)
12. [Support and links](#support-and-links)
13. [Legal](#legal)

---

## What you get

* **It runs your checks before it says done.** A turn that edits three files and
  never runs anything has not finished, it has stopped. When a turn is about to end
  with files changed and no check run, CoderMax asks for your project's check
  command by name and lets the turn continue. `/check` runs it yourself.
* **A risk gate that cannot be switched off.** Catastrophic shell commands are
  refused outright and no setting unlocks them. The middle tier, a glob, a `$VAR`
  target, a path outside the project, has to justify itself. Autonomy levels `ask`,
  `edit` and `full` decide how often you are asked, never whether the gate exists.
* **Sixteen providers, switched mid-session.** Anthropic, OpenAI, OpenRouter,
  TeamoRouter, Google, Ollama, NVIDIA, Moonshot, Xiaomi, MiniMax, Alibaba, Cerebras,
  DeepSeek, Groq, Mistral and xAI. Move between vendors, and between models inside a
  vendor, without losing the transcript. Ollama on localhost needs no key at all.
* **Keys in an encrypted vault.** API keys go into an AES-256-GCM store whose master
  key is held in the system keychain on macOS. Entry echoes bullets, the value never
  lands in a file or your shell history, and every screen shows a source such as
  `env` or `vault`, never a value.
* **Web search, on your own key.** As well as reading a URL you name, CoderMax can
  search the web through Brave Search, Tavily or Serper. You bring the key, so the
  results and the quota are yours. Set one vendor as the default and another as the
  fallback for when the first cannot answer, and switch either web tool off
  completely when a project has no business reaching the network.
* **Sessions that never overflow.** Context is managed for you, with a live meter in
  the status line and `/compact` when you want it explicit. Long sessions keep going
  instead of hitting a wall.
* **Close the terminal, come back tomorrow.** Every session is persisted with its
  transcript, its working directory and its change snapshots. `codermax -r <id>`
  picks it up exactly where you left it, in the directory it was working on.
* **A change ledger, and `/undo`.** Every file a session touches is snapshotted on
  first touch and every mutation is tallied against it. `/changes` lists them with
  line counts; `/undo` puts one file, or all of them, back the way they were.
* **Server mode, attach from anywhere.** A local daemon holds your sessions so a long
  turn survives closing the window. `codermax send` runs one turn from a script or a
  git hook, `codermax attach` gives you the full screen again, and several windows
  can watch one session at once.
* **Sub-agents for the wide reads.** The `task` tool forks a helper with its own
  context window for work whose output matters and whose transcript does not, so
  twenty files of searching never lands in your window.
* **Orchestrate, in parallel, on any model.** Tell a session to delegate execution
  to a stronger model and it holds for the whole conversation, or set it yourself
  with `/delegate`. Up to four sub-agents run at once, each on its own live row; one
  can run in the background while you keep talking; a running one can be corrected
  and a finished one can be asked a follow-up in the context it already has.
  `/tasks` lists them, and nothing outlives the session.
* **Project memory that stays out of your repo.** Durable facts about a project
  accumulate in a small Markdown file read fresh on every prompt build. It lives in
  CoderMax's own home, keyed by the project's path, so your repository never grows a
  file it did not ask for.
* **A full-screen terminal interface, plus a plain fallback.** `--plain` gives the
  line by line prompt with the same commands, engine and sessions, and is chosen
  automatically whenever output is not a terminal.
* **Installs itself on first run.** Run the file once and it copies itself into a per
  user folder, registers a login service, puts `codermax` on your PATH and asks for
  your licence key. No `sudo`, no Node.js, no package manager. Every later run
  repairs anything missing.

---

## Requirements

| Requirement | Detail |
| --- | --- |
| Operating system | macOS, Linux or Windows, 64-bit |
| Licence | A CoderMax licence, $25 a year, from <https://codermax.laxtic.com> |
| API key | One key from any of the sixteen supported providers, or a local [Ollama](https://ollama.com) daemon, which needs no key at all |
| Anything else | Nothing. No Node.js, no package manager, no administrator rights |

---

## Download

Each operating system has its own folder in this repository:

* **`macOs/`** holds one universal macOS build that runs on both Apple Silicon
  and Intel Macs.
* **`Linux/`** holds the two Linux builds, for x86-64 and for ARM64.
* **`Windows/`** holds the Windows build for x86-64.

Every download is a compressed archive holding exactly one file, the program itself,
with no folder around it.

| Platform | Architecture | Download | Program inside | Folder | Status |
| --- | --- | --- | --- | --- | --- |
| macOS | Universal (Apple Silicon and Intel) | `codermax-macos-universal.zip` | `codermax-macos-universal` | `macOs/` | Ready |
| Linux | x86-64 | `codermax-linux-x64.tar.gz` | `codermax-linux-x64` | `Linux/` | Ready |
| Linux | ARM64 | `codermax-linux-arm64.tar.gz` | `codermax-linux-arm64` | `Linux/` | Ready |
| Windows | x86-64 | `codermax-win-x64.zip` | `codermax-win-x64.exe` | `Windows/` | Ready |

Open the folder for your platform, click the archive that matches your machine, and
use **Download raw file**. Unpack it and the single file inside is the whole product:
the runtime is built in, and there is nothing else to install.

The archives exist because GitHub refuses to store any single file larger than
100 MB, and the macOS build is 124 MB uncompressed. Compressed, every build is
between 22 MB and 46 MB.

Each folder also carries three small text files. `INSTALL.md` is the install note for
that platform, including how to unpack. `SHA256SUMS.txt` lets you check the download,
and it carries two lines per build: the archive exactly as you downloaded it, and the
program inside it. On macOS and Linux, `shasum -a 256 -c --ignore-missing
SHA256SUMS.txt` checks whatever is in the folder, so it passes on the archive alone
before you unpack, and plain `shasum -a 256 -c SHA256SUMS.txt` checks both once you
have. On Windows use `certutil -hashfile <file> SHA256` and compare by eye.
`VERSION.json` records exactly which build the folder holds, with the size and SHA-256
of both the archive and the program inside it.

The same four builds are available to licence holders on the download page at
<https://codermax.laxtic.com/download>, which unlocks the links once your key checks
out.

---

## Install

There is no installer to babysit and no package manager involved. **The first run is
the installation**, and every later run repairs whatever went missing.

### macOS

Double-click the zip in Finder to unpack it, or do the same from a terminal, then run
the program once:

```bash
ditto -x -k ./codermax-macos-universal.zip .
chmod +x ./codermax-macos-universal
./codermax-macos-universal
```

There is one file for every Mac, Apple Silicon and Intel alike. The zip is the same
kind of container the build was notarised from, so Gatekeeper checks Apple's ticket
the first time the unpacked program runs: that first run wants a network connection,
and no run after it does.

### Linux

```bash
tar -xzf ./codermax-linux-x64.tar.gz
./codermax-linux-x64
```

Use the file name you downloaded. The tar archive carries the executable bit, so there
is nothing to `chmod`; if something along the way dropped it, run
`chmod +x ./codermax-linux-x64` first.

### Windows

Right-click `codermax-win-x64.zip` and choose **Extract All**, then from a terminal
opened in that folder:

```powershell
.\codermax-win-x64.exe
```

### What that one run does

Before anything else it checks the machine and, finding nothing installed, does all
four of these for your user account, with no `sudo` and no administrator prompt:

1. **Copies the program into place** under a private per-user folder. The file you
   downloaded can be thrown away afterwards.
2. **Registers a login service** so the CoderMax daemon comes up when you log in: a
   LaunchAgent on macOS, a systemd user service on Linux, a Task Scheduler logon task
   on Windows.
3. **Puts `codermax` on your PATH**, so the command works from any directory in a new
   terminal.
4. **Asks for your licence key** and activates this machine.

It says what it did, one short line each, and then runs whatever you actually typed:

```text
codermax: installed to ~/Library/Application Support/CoderMax
codermax: registered the login service
codermax: repaired the PATH command
```

**Every later run re-checks that work and repairs anything missing.** Deleted the
`codermax` link, or had a system update clear your login items? The next `codermax`
puts it back and says so in one line. A healthy install is completely silent.

### Leaving a part out

The automatic check installs everything. `codermax install` is the explicit form, and
it is the way to leave a part out:

| Flag | Effect |
| --- | --- |
| `--no-service` | Install the command without the login service |
| `--no-path` | Install without touching your PATH |
| `--no-license` | Install without asking for a licence key now |
| `--yes` | Do not prompt. It never deletes data on its own |

A decline is remembered, so the automatic repair never puts back a part you said no
to. Run `codermax install` again without the flag to change your mind.

### Verify

Open a **new** terminal so the PATH change is picked up, then:

```bash
codermax --version
codermax service status
```

`codermax service status` changes nothing. It reports whether the command is
installed, whether the installed copy matches the build you ran, whether the login
service is registered and running, whether the daemon answers, and the state of your
licence.

---

## First run and activation

CoderMax is a paid product with no trial and no demo mode. Buy a licence at
<https://codermax.laxtic.com> and the key arrives by email.

A key looks like this, four groups of five characters:

```text
LX-XXXXX-XXXXX-XXXXX-XXXXX
```

Keys never contain the letters `I` or `O` or the digits `0` and `1`, so they can be
read aloud without ambiguity. Case and stray spaces do not matter, and the shape is
checked on your machine before any request is made, so a typo costs no round trip.

The first run asks for your key, so most people never type the command. To activate
at any other time:

```bash
codermax license activate LX-XXXXX-XXXXX-XXXXX-XXXXX
```

| Command | What it does |
| --- | --- |
| `codermax license status [--json]` | What is installed, read from the local cache, no network. Shows the plan, the expiry, the seats and when it last checked |
| `codermax license activate <KEY>` | Check a key online once and install it on this machine |
| `codermax license refresh` | Ask the licence service again right now |
| `codermax license deactivate` | Release this machine's seat, so you can use it elsewhere |

Exit codes are `0` for licensed, `1` for a licence problem and `2` for a usage
problem.

### Three machines

One licence covers **up to three machines** at a time: a laptop, a desktop, and one
build or remote box is the shape it is sized for. Activating a fourth is refused
until you free a seat.

Moving is easy. Run `codermax license deactivate` on the machine you are leaving, or
`codermax uninstall`, which releases the seat for you, then activate the same key on
the new machine. The old machine's history is kept, so activating there again later
costs nothing. Deactivate before you wipe or reimage a machine, otherwise the seat
stays held until support releases it.

### Offline

CoderMax works offline after one online activation. What activation stores is a
signed licence, and CoderMax verifies that signature on your own machine with no
network involved.

In the background it refreshes roughly every four hours when it can reach the
network, and you see nothing when it does. If it cannot reach the licence service at
all, it keeps honouring your last valid licence for up to seven more days and tells
you when that window closes, so a flight, a train or a client site with no outbound
access is not a lockout. **A network failure is never treated as a rejection.** Only
a genuine refusal from the service, an expiry, a cancellation or a revocation stops
it.

The model provider still needs a network, unless you run Ollama locally.

### Before you are licensed

The background service still starts, but it stays locked and refuses to run sessions
until a valid licence is present. It unlocks the moment you activate one, with no
restart. An interactive `codermax` on an unlicensed machine tells you how to buy and
activate, and exits.

---

## Everyday use

### Add a provider key

CoderMax defaults to the `anthropic` provider and the `claude-haiku-4-5` model, so
the shortest path is an Anthropic key. Any of the sixteen providers works just as
well.

```bash
export ANTHROPIC_API_KEY=sk-ant-...
```

Add that line to `~/.zshrc` or `~/.bashrc` to make it stick across terminals. On
Windows PowerShell, `setx ANTHROPIC_API_KEY "sk-ant-..."` does the same for your user
account and takes effect in new terminals.

Or store the key in the encrypted vault from the setup screen, which never writes it
to a file:

```bash
codermax setup
```

That opens a full-screen, two-pane page for provider setup, browsing providers and
models, defaults, web search, your licence and settings. On a fresh install it opens by itself.
Inside a session, `/setup` opens the same screen over the transcript. A key is never
displayed: entry echoes bullets, the value goes straight into the encrypted store,
and it is probed on the spot so you find out immediately whether what you pasted
works.

Ollama is the one provider that needs no key when it runs on localhost:

```bash
codermax --provider ollama --model qwen3-coder
```

### Turn on web search

CoderMax can search the web, not only read a URL you paste. It searches on **your**
account, so it needs a key from one of three search vendors:

| Vendor | Where to get a key |
| --- | --- |
| Brave Search | <https://api.search.brave.com>, under Subscriptions |
| Tavily | <https://app.tavily.com>, under API Keys |
| Serper | <https://serper.dev>, under API Key. Google results |

One key is enough. Add it the same way you add a provider key, from the setup
screen's **Web search** page, or from inside a session:

```text
/web key set brave
```

The key is asked for on the next line with the echo masked, and goes straight into
the encrypted store. Exporting `BRAVE_API_KEY` (or `TAVILY_API_KEY`, or
`SERPER_API_KEY`) works too and wins over the stored one.

Then say which vendor answers first, and which one covers for it:

```text
/web provider brave
/web fallback tavily
```

The fallback is used when the first vendor has no key, errors, times out or cannot
be reached. A search that simply found nothing is an answer, not a failure, so it is
not retried elsewhere. `/web fallback none` makes one attempt and stops.

`/web` on its own prints the current state, `/web test` runs one real search to prove
the key works, and either tool can be switched off so the model is never offered it:

```text
/web search off
/web fetch off
```

### Start a session

CoderMax works on **the directory you launch it from**, so change into the project
you want help with:

```bash
cd ~/projects/my-app
codermax
```

Or name the project instead of moving to it, which is the same thing in one line:

```bash
codermax --project ~/projects/my-app
```

Then type a request:

```text
> read package.json and tell me what this project depends on
```

CoderMax streams the answer and prints one line per tool call as it works. Press
**Ctrl+C** once to cancel a running turn, and again at an idle prompt to exit.

### The commands worth knowing

Type `/` at the start of the composer to open the command palette.

| Command | What it does |
| --- | --- |
| `/provider`, `/model` | Switch vendor or model without losing the transcript |
| `/keys`, `/models` | See which key source each vendor resolves to, and what models it offers |
| `/web` | Web search and web fetch: the switches, the provider, the fallback, and a key per search vendor |
| `/delegate` | Which model this session's sub-agents run on. `/delegate "Opus 4.8" high` locks one for the session, `/delegate effort <level>` changes only the effort, `/delegate off` gives it back |
| `/tasks` | The sub-agents this session has: id, state, model, description, turns and cost. `/tasks cancel <id\|all>` and `/tasks close <id>` |
| `/check` | Run this project's check command and show the summary |
| `/changes`, `/undo` | List what this session changed, and put a file or all of them back |
| `/autonomy` | `ask`, `edit` or `full`, for the session or saved |
| `/compact` | Compact the context explicitly |
| `/sessions`, `/resume <id>`, `/new` | Move between conversations |
| `/config` | Print the effective settings and the exact path of the settings file |
| `/setup` | Open the setup screen over the transcript |
| `/quit` | Leave |

Press `Enter` while a turn is running and the message goes **into** the turn rather
than behind it, so "no, not that file" arrives while it still means something.

### Server mode

The login service means a daemon is usually already running, so a long turn survives
closing the window and a script can talk to a session:

```bash
codermax new                     # a session for this directory
codermax send "run the tests"    # one turn, streamed to stdout, then exit
codermax attach                  # the full screen, on that same session
codermax sessions                # what the daemon is holding
codermax status                  # is one running, and what is it doing
codermax stop                    # save every session and stop
```

Nothing has to be started by hand: every client command starts a daemon when there is
none. `codermax send` exits `0` on completion, `1` on error and `130` when cancelled,
which is what makes it usable from a git hook, a job or a build step. For unattended
runs set `CODERMAX_AUTONOMY=full`, because a permission question with nobody there to
answer it is denied. No autonomy level unlocks the catastrophic tier.

---

## Where your files live

Nothing is installed system-wide, and nothing is written outside your own user
account.

| | Location |
| --- | --- |
| macOS | `~/Library/Application Support/CoderMax` |
| Linux | `~/.local/share/codermax` |
| Windows | `%LOCALAPPDATA%\CoderMax` |

Inside that folder, CoderMax keeps its own home: your settings in `config.toml`
(created on first run, readable only by you), your API keys encrypted beside it, your
sessions with their full transcripts, and the durable tool state that makes `/undo`
work after a resume. Run `/config` to print the exact path. None of it is written
into your repository.

`CODERMAX_HOME` relocates the whole tree if you would rather keep it somewhere else.

---

## Updating

Download the newer archive for your platform, unpack it, and run the program once. It
replaces the installed copy and restarts the login service, so the daemon serving your
sessions is the build you just ran:

```text
codermax: updated the installed copy and restarted the service
```

There is no separate update command and no package manager involved. Every build
released during your licence term is included in the price.

---

## Uninstalling

```bash
codermax uninstall
```

`codermax --uninstall` is the same thing. It first tells you exactly what it will
remove and what it will keep, then asks you to confirm, with No as the default. On
confirmation it releases this machine's licence seat, stops and unregisters the login
service, removes the `codermax` command from your PATH, and deletes the installed
program.

**Your sessions, your configuration and your stored API keys are kept.**

| Flag | Effect |
| --- | --- |
| `--yes` | Skip the question. It never deletes your data on its own |
| `--purge` | Also delete your sessions, configuration, stored keys and the system keychain entry. This cannot be undone, so it asks you to type a confirmation phrase first |

To move CoderMax to another computer rather than remove it, uninstall on the old
machine, which frees the seat, then download the build for the new one, run it once,
and activate the same key.

---

## Privacy and security

CoderMax is a tool you install, not a service you log into. There is no account to
keep, no workspace in someone else's cloud, and no path by which your repository
leaves your machine except the conversation you started with the provider you chose.

* **Your keys are encrypted.** Each vendor gets its own AES-256-GCM entry with its own
  nonce, and the vendor id is bound into the encryption, so an entry cannot be moved
  from one vendor's row to another's. On macOS the master key lives in your login
  keychain rather than beside the file; elsewhere it is a file readable only by you.
  A key is never displayed, never written into a config file, and never lands in your
  shell history. If you would rather not store keys at all, export the vendor's
  environment variable instead: environment variables are read first and never land
  in a file.
* **Model traffic goes straight from your machine to the provider you chose**, with
  your own API key, as part of the conversation you started.
* **The licence check is separate from your model traffic** and carries five things:
  your licence key, a random device id generated on your machine, your hostname, the
  CoderMax version and your platform. **No source code, no prompts, no file paths, no
  project names and no telemetry.** The device id is a random identifier, not a
  hardware fingerprint: nothing about your CPU, disk or network hardware is read or
  transmitted.
* **The local daemon is local only.** There is no TCP listener and no way to make one.
  It binds a Unix domain socket, a named pipe on Windows, inside a private per-user
  directory, and every connection has to present a random token stored alongside it.
  Another user on the same machine cannot drive your agent, and nothing off the
  machine can reach it at all.
* **Nothing runs with elevated privileges.** There is no `sudo` step and no
  system-wide install: the program, the login service and the PATH entry all belong
  to your user account.
* **Anything CoderMax helps you write is yours.** No interest is claimed in your code,
  your prompts or your output.

---

## Pricing

| | |
| --- | --- |
| Plan | CoderMax Annual |
| Price | **$25 a year** |
| Term | 365 days |
| Machines | 3 per licence |
| Trial | None. There is no demo mode |
| Refunds | 48 hours from the payment timestamp |

One plan, and it is the whole product. No tiers, no per-seat add-ons and no feature
held back for a bigger cheque:

* Three machines on one licence
* All sixteen model providers, switchable mid-session
* Every build released during your licence term
* Full-screen terminal interface, plus a plain fallback
* Verification loop, risk gate and change ledger
* Encrypted key store and system keychain support
* Server mode with a login-time daemon
* Sessions, project memory and sub-agents
* macOS, Linux and Windows builds
* Email support from Laxtic Software Services

What the price does not include is model usage. CoderMax is the agent, not the model
vendor, so you hold the provider account and pay that vendor directly for the tokens
you use, or point it at a local Ollama daemon and pay nothing at all.

**Buy at <https://codermax.laxtic.com>.** The licence key arrives by email.

A refund is available within 48 hours of payment, counted from the payment timestamp
on your receipt. Ask through the contact page, quoting your licence key or the email
address you bought with. A refund revokes the licence and deactivates every machine
activated on it. The full policy is at <https://codermax.laxtic.com/refunds>.

---

## Support and links

| | |
| --- | --- |
| Product site, purchase and account | <https://codermax.laxtic.com> |
| Downloads | <https://codermax.laxtic.com/download> |
| Check a licence | <https://codermax.laxtic.com/license> |
| Contact and support | <https://codermax.laxtic.com/contact> |
| Feature requests, and voting on them | <https://codermax.laxtic.com/suggestions> |
| Terms | <https://codermax.laxtic.com/terms> |
| Privacy | <https://codermax.laxtic.com/privacy> |
| Refunds | <https://codermax.laxtic.com/refunds> |

When you get in touch, tell us what happened, which platform you are on, and the
output of `codermax --version` and `codermax service status`. Those two commands
report the state of the install, the login service, the daemon and your licence, and
they usually answer the question on their own.

---

## Legal

CoderMax is published and licensed by **Laxtic Software Services**. A licence is a
non-exclusive, non-transferable right to install and use the software for the
duration of your licence term, on up to three devices at a time, for personal work,
commercial work, and work you are paid for by a client or an employer. Copying,
hosting, reselling, sublicensing, renting or redistributing the software is not
granted, and the builds here require a valid CoderMax licence key to run. The terms
at <https://codermax.laxtic.com/terms> are the authority.
