# Bluepad32

Bluetooth "host" library that lets microcontrollers accept input from gamepads, mice, and keyboards. Targets ESP32 family (ESP32, S3, C3, C6, H2), Raspberry Pi Pico W / Pico 2 W, Arduino (IDE + ESP-IDF), and Posix (Linux, macOS) for Bluetooth packet analysis.

Written in C. Built on top of [BTstack](https://github.com/bluekitchen/btstack). Platform code lives behind a thin abstraction so the same core handles every target.

## Architecture

@docs/architecture.md

Platforms (`Unijoysticle`, `NINA`, `AirLift`, `Arduino`, `MightyMiggy`, custom) plug into the Bluepad32 core, which uses BTstack on top of the OS-specific SDK (ESP-IDF, Pico SDK, or libusb on Posix). Adding a new platform: `docs/adding_new_platform.md`.

## Repo layout

| Path | Contents |
|------|----------|
| `src/components/bluepad32/` | Core library + platform implementations |
| `src/main/` | ESP-IDF entry point for the default firmware |
| `examples/esp32/` | ESP-IDF example app |
| `examples/pico_w/` | Pico SDK example app |
| `examples/arduino/` | Arduino sketches |
| `examples/posix/` | Linux/macOS example (BTstack + libusb) |
| `external/btstack/` | BTstack submodule |
| `tools/fw/` | Firmware build scripts (`build.py`) |

## Build

Each target has its own toolchain. Pick the one for the platform you're working on:

- **ESP-IDF** (ESP32 family, default firmware): `idf.py -C src build` — requires ESP-IDF v4.4 or v5.x
- **Pico SDK** (Pico W / Pico 2 W): `cmake -B build && make -C build -j` from `examples/pico_w`
- **Posix** (packet capture / protocol work): `cmake -B build && make -C build -j` from `examples/posix` — requires libusb

Upstream CI configs live at `src/sdkconfig.ci.plat_*` for the ESP-IDF platform variants.

## Developer workflows

@docs/developer_notes.md

Covers release flow (main↔develop merge), core-dump analysis, packet capture via the Posix platform + Wireshark, and OpenOCD setup for Pico W.

## Conventions

- C style follows the existing sources — don't reformat unrelated code while making a change
- Commit messages are imperative, scoped where it helps (`fix esp32 example build issue`, `Update CHANGELOG`)
- Release bumps touch `src/version.txt`, `uni_version.h`, `CHANGELOG.md`, `AUTHORS` — see developer_notes.md
- This fork's remotes: `origin` = `laurigates/bluepad32`, `upstream` = `ricardoquesada/bluepad32`. Pull upstream changes via `git fetch upstream && git merge upstream/main`.

## Contributing back

@CONTRIBUTING.md

Upstream is active (GitLab + GitHub). For fixes worth sending back, target `ricardoquesada/bluepad32` via the upstream PR flow rather than this fork.
