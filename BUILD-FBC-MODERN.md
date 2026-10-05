# Build with FBC-Modern — 64-bit only

This branch (`fbc-modern`) is compiled with **FBC-Modern**, not stock FreeBASIC.

- Compiler: [Alice2Git/FBC-Modern](https://github.com/Alice2Git/FBC-Modern), branch `lambda-fixes`, commit `68f487b`
  (`toolchains/fbc-modern-windows/fbc64.exe`, fbc 1.20.0, win64)
- Source change needed: `UString` renamed to `UStringX`, because FBC-Modern has a built-in `USTRING` type.

## Only the 64-bit binaries were rebuilt

| File | State |
|---|---|
| `mff64.dll`, `libmff64.dll.a` | rebuilt with FBC-Modern (2026-10-05) |
| `mff32.dll`, `libmff32.dll.a` | **removed** — the 32-bit build is not used |

**Why:** only the 64-bit version is used. The 32-bit binaries were removed, so that no
old build (made with stock FreeBASIC, still using the old `UString` name) stays next to the
new sources. This branch is worked on from two machines: do not expect 32-bit binaries,
and do not rebuild them unless that decision changes.

The sources themselves still support 32-bit (`#ifdef __FB_64BIT__` branches are untouched).

## Command used (Windows)

```bat
cd mff
fbc64.exe -b "mff.bi" "mff.rc" -dll -gen gcc -Wc -O2
```

Note: `mff.bi` contains `#cmdline "-x ../mff64.dll"`, which overrides any `-x` given on
the command line — the output always goes to `../mff64.dll`.
