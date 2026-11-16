# Contributing

Thank you for helping. This project is small and was tried on **one watch** (Galaxy Fit3, SM-R390, firmware **R390XXU0AZA3**), so the most valuable contributions are often *facts*: what you saw on your own watch, with the version and the steps. Please read [docs/legal.md](docs/legal.md) first.

## Reporting a problem

Open an issue with:
- the watch model and the **firmware version** (see [docs/howto.md](docs/howto.md#check-your-firmware-version));
- the exact command you ran and the package you flashed (for example `--hub tennis,counter,dice`, with or without `--no-bluetooth-patches`);
- what you expected and what happened, and whether the same happens on the stock package;
- the log from `local/logs/` if a tool of ours wrote one (it never contains the package's bytes).

Never post firmware images, packages built from them, or files pulled from your watch.

## What we cannot accept

- **Anything of Samsung's:** firmware, packages, fonts, images or code decompiled from the firmware. Notes, addresses and function names found for interoperability are fine; quoted code is not.
- Code of other projects that have no license (see the table in [docs/legal.md](docs/legal.md)). Facts about a format or protocol are fine; your own implementation of it is what we want.
- Build outputs. `*.bin`, `*.apk`, `local/` and the like are git-ignored on purpose.

By opening a pull request you agree that your contribution is under the project's license, [Apache-2.0](LICENSE).

## Safety on a real watch

- **Do not flash anything that did not pass `validate_fwd.py`** (the build runs it). A watch that does not boot cannot be flashed over Bluetooth.
- **Do not send commands we have not documented.** The firmware has `FORMAT`, `RM` and `RMDIR` ([docs/protocol.md](docs/protocol.md)).
- A change that touches firmware code (a patch or a hook) must say which callers it affects and how each user path was tested: look for every `bl`, `b.w` and pointer to the function, not only the one you meant.
- Say what you tried on a watch and what you did not. [docs/status.md](docs/status.md) separates **watch**, **emulator**, **PC** and **open**; a statement that is not there is a hypothesis, and please mark it as one in comments and notes.

## Making a change

Build and the tools need Python 3 (numpy, Pillow) and `arm-none-eabi-gcc`; run everything with `python3 -I`. The full picture is in [src/README.md](src/README.md).

- **An app:** copy the smallest one and follow [src/watch/apps/README.md](src/watch/apps/README.md) (no globals, no libc, rare and small file writes). One C file in `src/watch/apps/`.
- **Documentation:** in English, short, with the addresses and what was confirmed. New firmware addresses go to [docs/symbols.csv](docs/symbols.csv) as `address,name,kind,confidence,evidence`.
- **Tests:** every directory with code has `test_*.py` next to it; run the ones for what you touched, for example `python3 -I src/firmware/test_build_package.py` (needs the stock package in `local/firmware`, see [docs/howto.md](docs/howto.md)) or the emulator tests ([src/emu/README.md](src/emu/README.md)). Tell us in the pull request which ones you could not run.
- **Commits:** one logical change each, a subject in English in the form `[area] Imperative sentence` (look at `git log`: `[hub]`, `[docs]`, `[flash]`, `[bluetooth]`, ...). Reference an issue with `Fixes #N` in the body.
- **Do not add dependencies** for what the standard library does, and keep the tools that write to the watch (`f3_flash.py`, `f3x_install.py`) the only ones that do.

## Pull requests

Keep them small and say what you tested and on which firmware. If a change alters what a package does on the watch, include the size of the free flash left (`build_firmware.py` prints it) and what you saw.
