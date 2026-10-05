# Build with FBC-Modern

This branch (`fbc-modern`) is compiled with **FBC-Modern**, not stock FreeBASIC.

- Compiler: [Alice2Git/FBC-Modern](https://github.com/Alice2Git/FBC-Modern), branch `lambda-fixes`, commit `68f487b`
  (`toolchains/fbc-modern-windows/fbc64.exe`, fbc 1.20.0)
- Source change needed: `UString` renamed to `UStringX`, because FBC-Modern has a built-in `USTRING` type.

## Binaries

| File | Built with (2026-10-05) |
|---|---|
| `mff64.dll`, `libmff64.dll.a` | `fbc64.exe` |
| `mff32.dll`, `libmff32.dll.a` | `fbc64.exe -target win32` (**not** `fbc32.exe`, see below) |

### Why 32-bit is built with `fbc64.exe -target win32`

FBC-Modern's `fbc32.exe` (the compiler running as a 32-bit process) has a bug: on
VisualFBEditor it stops with `Expected identifier, found 'frmProjectProper'` on a correct
line, and the text after "found" changes when the line moves (`'frmProj'`), which points to
memory corruption inside the compiler. The code is fine: `fbc64.exe -target win32` compiles
the same sources without errors. `fbc32.exe` did build this DLL without errors, but to avoid
a compiler that is known to misbehave, both 32-bit projects are built with
`fbc64.exe -target win32`. Not fixed in FBC-Modern (that repo is kept read-only).

History: commit `d475752` had only the 64-bit binaries (32-bit was removed); the 32-bit
ones were added back later the same day so that the repo is complete.

## Commands used (Windows)

```bat
cd mff
fbc64.exe -b "mff.bi" "mff.rc" -dll -gen gcc -Wc -O2
fbc64.exe -target win32 -b "mff.bi" "mff.rc" -dll -gen gcc -Wc -O2
```

Note: `mff.bi` contains `#cmdline "-x ../mff64.dll"` / `"-x ../mff32.dll"`, which overrides any
`-x` given on the command line — the output always goes to `../mff64.dll` / `../mff32.dll`.
