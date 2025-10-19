# Repository Guidelines

## Project Structure & Module Organization
The emulator entry point is `jzintv.c`, while hardware subsystems live in focused directories: `cp1600/` (CPU core), `stic/` (video), `ay8910/` and `snd/` (audio), `periph/`, `pads/`, `joy/`, `mouse/` (I/O), and `serializer/` (state). `mem/`, `cfg/`, and `bincfg/` handle ROM/RAM layout and cartridge metadata. Developer tooling sits in `asm/`, `imasm/`, and `dasm/`; command-line helpers in `util/`; sample ROM sources in `demo/`; platform glue in `plat/` and `event/`. Each module exposes a `subMakefile` that `Makefile.common` includes—follow that pattern for new code.

## Build, Test, and Development Commands
- `make`: Build the emulator binary at `../bin/jzintv` (create `../bin`, `../lib`, and `../rom` first if absent).
- `make build`: Build the emulator plus SDK utilities and diagnostic ROMs into `../bin` and `../rom`.
- `make clean`: Remove intermediates enumerated by the module `subMakefile`s.
Keep `sdl-config` on PATH so SDL include and link flags resolve.

## Coding Style & Naming Conventions
Match the existing C style: 4-space indentation, braces on their own line, lower_snake_case names for functions/files, and wide block comments (`/* ==== */`) for section banners. There is no formatter in-tree, so mirror nearby code and keep preprocessor directives flush left. When adding tools or ROM generators, pair them with a `subMakefile` entry so they join the aggregate build.

## Testing Guidelines
No automated harness ships with the tree. After building, run the diagnostics emitted to `../rom` (e.g. `../bin/jzintv ../rom/event_diag.rom`) and verify audio/video/input paths with representative Intellivision ROMs. When adding features, document the manual validation steps and drop helper ROMs or scripts into `demo/` if they aid regression coverage.

## Commit & Pull Request Guidelines
History favors short, imperative subjects (“Add gitignore file”); keep new commit subjects under ~50 characters, add a blank line, then optional detail. In pull requests, cover the subsystems touched, build/test commands run, impacted platforms (macOS notes are in `notes.osx`), and link issues. Include logs or captures when audio/video behavior changes.

## Platform & Dependency Notes
SDL is the primary external dependency; install the development headers so `sdl-config` is present. Builds assume this tree sits inside an SDK root with sibling `bin/`, `lib/`, and `rom/` directories—create them when working standalone. Platform-specific makefiles (`Makefile.osx_unix`, `Makefile.w32`, etc.) document extra flags; use them as references when tuning toolchains or adding targets.
