# Agents

## Cursor Cloud specific instructions

This is an **Arduino C++ library** (Adafruit L3GD20 gyroscope sensor driver). It is not a web application — there are no services to start. Development consists of compiling sketches and running lint checks.

### Key Commands

| Task | Command |
|------|---------|
| **Compile example** | `arduino-cli compile --warnings all --fqbn arduino:avr:uno /workspace/examples/sensorapi` |
| **Lint (clang-format)** | `python3 ~/ci-arduino/run-clang-format.py -e "ci/*" -e "bin/*" -r .` |
| **Install additional platforms** | `arduino-cli core install <platform> --additional-urls "<BSP_URLS>"` |

### Important Gotchas

- **NEVER run `arduino-cli lib uninstall` on the development library.** The library is symlinked from `/workspace` into `~/Arduino/libraries/Adafruit_L3GD20_U`. Running uninstall follows the symlink and **deletes the actual workspace files** (destructive!).
- **Do NOT run `ci/build_platform.py` directly from a workspace-cloned `ci/` directory.** That script calls `arduino-cli lib uninstall` on the library under development, which destroys the workspace (see above). Instead, compile examples directly with `arduino-cli compile`.
- The CI toolchain (`adafruit/ci-arduino`) is cloned to `~/ci-arduino` (outside the workspace) to avoid polluting the repo.
- The library symlink must exist at `~/Arduino/libraries/Adafruit_L3GD20_U -> /workspace` for compilation to find it.
- Warnings about unused parameters in `new.cpp` are from the Arduino AVR core itself, not from this library.
- The `library.properties` file declares `depends=Adafruit Unified Sensor` — install it via `arduino-cli lib install "Adafruit Unified Sensor"`.
- BSP URLs for additional board support: see `~/ci-arduino/all_platforms.py` for the full list of supported platforms and FQBNs.
