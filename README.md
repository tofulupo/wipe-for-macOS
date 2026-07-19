# wipe — with macOS support

This is a fork of [berke/wipe](https://github.com/berke/wipe), a Unix tool for secure file deletion written by **Berke Durak**. All credit for the software belongs to him.

## Why this fork exists

I wanted to have `wipe` working on macOS. Nothing more — the goal is to keep the original software usable on modern macOS, not to redesign it.
I'm maintaining this fork to keep the software usable on macOS (as good as I can). For any bug fixes or improvements to the core functionality, please open a ticket in the [original repo](https://github.com/berke/wipe/issues) or contribute upstream.

## What changed

- **Added `macos` target** to the Makefile with compiler flags for macOS (tested on macOS 26.5 Tahoe). Includes feature detection defines matching the existing Linux target where applicable (`HAVE_DEV_URANDOM`, `HAVE_STRCASECMP`, `HAVE_RANDOM`, `HAVE_OSYNC`).
- **Modified `version.h` generation** — replaced the non-POSIX `which` command with `command -v` and added a guard for building outside a git repository (tarball builds).
- **Unified install target** — replaced OS-specific install targets with a single POSIX-compliant `install` target using `PREFIX`, `BINDIR`, and `MANDIR` variables. Drops hardcoded `-o root -g root` ownership flags, eliminating the need for Debian's `050_rootless.patch`. macOS users install with `make PREFIX=/usr/local install`.
- **POSIX-compliant install** — no GNU-isms; works with native `make` and `install` on SunOS, AIX, Solaris, FreeBSD, and other legacy Unix.
- **Added a downstream patch** https://github.com/tyll/wipe/commit/0ad42af17b3e7745a4be07cde8ad5a0259b40d15 and https://sources.debian.org/patches/wipe/0.24-11/020_fix-warnings.patch/
- **Modified man page and `-h` option** — Update version from 0.22 (2010) to 0.24 (November 2016), Remove `-a` option documentation (not implemented in source code), Add missing options: `-b` (buffer size), `-P` (filename passes), `-T` (search tries), Correct the obfuscated email address to match the original.

Everything else is untouched.

## Building

```console
# Xcode Command Line Tools (includes clang/gcc, make, headers)
xcode-select --install
```

```console
make macos
```

## Installing 

```console
# macOS (to /usr/local/bin, SIP-safe)
make macos && sudo make PREFIX=/usr/local install
```
```console
# Linux (to /usr/bin)
make linux && sudo make install
```
```console
# Packaging/staging (Debian/RedHat)
make linux && make install DESTDIR=/tmp/pkg-staging
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

## Tested On

| OS | Version | Architecture | Status |
|---|---|---|---|
| Debian GNU/Linux | 13 (trixie) | x86_64 | Tested |
| Debian GNU/Linux | 10 (buster) | x86_64 | Tested |
| Ubuntu | 24.04.4 LTS (Noble Numbat) | x86_64 | Tested |
| Ubuntu | 14.04.6 LTS | x86_64 | Tested |
| macOS | 26.5.2 (Tahoe) | ARM64 | Tested |
| Solaris | 9 (s9_58shwp13) | SPARC32 | Tested |
| OpenIndiana | Hipster 2026.04 | x86_64 | Pending |
| macOS | 10.3 (Panther) | — | Pending |
| SunOS | 5.5.1 | — | Not Tested |
| Solaris | 2.6 | SPARC | Not Tested |
| Solaris | 2.6 | x86 | Not Tested |
| FreeBSD | 2.2.6-STABLE | — | Not Tested |
| AIX | 4.2 | — | Not Tested |
| Digital/Compaq UNIX | Alpha | Alpha | Not Tested |

## Credit

All original code and design by **Berke Durak**. This fork only adds platform support and a build fix for personal use.
