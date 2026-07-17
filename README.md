# wipe — with macOS support

This is a fork of [berke/wipe](https://github.com/berke/wipe), a Unix tool for secure file deletion written by **Berke Durak**. All credit for the software belongs to him.

## Why this fork exists

I wanted to have `wipe` working on macOS. Nothing more — the goal is to keep the original software usable on modern macOS, not to redesign it.
I'm maintaining this fork to keep the software usable on macOS (as good as I can). For any bug fixes or improvements to the core functionality, please open a ticket in the [original repo](https://github.com/berke/wipe/issues) or contribute upstream.

## What changed

- **Added `macos` target** to the Makefile with compiler flags for macOS (tested on macOS 26.5 Tahoe). Includes feature detection defines matching the existing Linux target where applicable (`HAVE_DEV_URANDOM`, `HAVE_STRCASECMP`, `HAVE_RANDOM`, `HAVE_OSYNC`).
- **Modified `version.h` generation** — replaced the non-POSIX `which` command with `command -v` and added a guard for building outside a git repository (tarball builds).
- **macOS install target** — respects SIP by installing to `/usr/local/bin` and `/usr/local/share/man/man1`.

Everything else is untouched.

## Building

```console
make macos
```

Then install as root:

```console
sudo make mac-install
```

See the original [README](https://github.com/berke/wipe/blob/master/README) for full usage instructions.

## Compatibility

All original targets remain unchanged and should work as before:

- Linux
- SunOS
- AIX
- Solaris SPARC / x86
- FreeBSD
- Digital/Compaq Alpha UNIX
- macOS (new)
- Generic Unix

## Credit

All original code and design by **Berke Durak**. This fork only adds platform support and a build fix for personal use.
