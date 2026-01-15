# Copilot Instructions for Pandaria 5.4.8

## Project Overview
- This is a World of Warcraft Mists of Pandaria 5.4.8 server emulator, supporting Linux, Windows, and Mac.
- Major components are in `src/`:
  - `server/`: Core server logic, including worldserver, authserver, game, collision, scripts, and shared modules.
  - `updater/`: SQL/database update tool, built optionally via CMake flag `-DUPDATER=1`.
  - `tools/`: Utilities for data extraction and other tasks.
  - `genrev/`: Version/revision management.
- External dependencies: ACE, MySQL 5.7, OpenSSL 3.x.x, Boost ≥ 1.70, Windows SDK 10, MSVC ≥ 2019, GCC ≥ 9, Clang ≥ 11.

## Build & Developer Workflow
- Use CMake for all builds. Example (Linux):
  ```bash
  cmake ../ -DCMAKE_INSTALL_PREFIX=$HOME/yourUser/folder -DCMAKE_C_COMPILER=/usr/bin/clang-XX -DCMAKE_CXX_COMPILER=/usr/bin/clang++-XX -DSCRIPTS=static
  make -j $(nproc)
  make install
  ```
- On Windows, use Visual Studio (Community 2019+). Set flags in CMake GUI or command line.
- To build the updater tool:
  - Add `-DUPDATER=1` to CMake flags.
  - Copy built binaries and config files (`updater.exe`, `updater.conf.dist`, `ace.dll`, `libmysql.dll`) as needed.
  - Rename `updater.conf.dist` to `updater.conf` and configure database connection info.
- Source code is organized for modular builds; enable/disable components via CMake flags (`TOOLS`, `UPDATER`, `SERVERS`, `AUTH_SERVER`).

## Project-Specific Patterns & Conventions
- CMake scripts in `cmake/` and `dep/` manage platform-specific options and dependencies.
- SQL update scripts and config templates are in `src/updater/`.
- Database connection strings use semicolon-separated values in config files.
- All source changes must be outside the source root (`CMAKE_DISABLE_SOURCE_CHANGES ON`).
- Use `make install` or Visual Studio install step to deploy binaries and configs.

## Integration Points
- Database: MySQL 5.7, configured via `updater.conf` and other config files.
- ACE library: Included for Windows, required for builds.
- Data extraction tools require additional CMake flags and dependencies.

## Key Files & Directories
- `README.md`: Build instructions, requirements, and links to client/data resources.
- `src/server/`: Main server modules and logic.
- `src/updater/`: Updater tool, config, and SQL scripts.
- `cmake/`: Build system scripts and platform macros.
- `dep/`: Third-party dependencies (ACE, bzip2, etc.).
- `sql/`: Database schema and update scripts.

## Example: Building Updater Tool (Windows)
1. Run CMake with `-DUPDATER=1` or set in GUI.
2. Build `updater` project in Solution Explorer.
3. Copy `updater.exe`, `updater.conf.dist`, `ace.dll`, `libmysql.dll` to target directory.
4. Rename and configure `updater.conf`.

---
For unclear or missing conventions, review `README.md`, `src/updater/README.txt`, and CMake files. Ask for feedback to improve these instructions.
