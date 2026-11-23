# Cutefish

Cutefish is a Linux distribution built on Debian (`amd64` and `arm64`) for people at home on a Mac. Its desktop is written from scratch in C++ and Skia, from the Wayland compositor to the built-in apps.

[Website](https://cutefish.org) · [Download](https://download.cutefish.org/) · [Documentation](https://cutefish.org/docs) · [Blog](https://cutefish.org/blog) · [Discord](https://discord.gg/mhUVu4dPH8) · [X](https://x.com/cutefishos) · [Sponsor](https://github.com/sponsors/cutefishos)

## Highlights

- **Wayland compositor and shell** — `cutefish-compositor` handles window management, effects and animations, and draws the menu bar, Dock, launcher and lock screen directly. `cutefish-dm` manages login and starts the session.
- **Native C++ and Skia toolkit** — `cutefish-toolkit` is a lightweight native UI toolkit shared by the desktop and every built-in app: Files, Settings, Terminal, Text Editor, Preview, Video Player, Calendar, Calculator, Clock, Disk Utility and System Monitor.
- **Compositor-level agent control** — With [`cutefish-computer-use`](https://github.com/cutefishos/cutefish-computer-use), MCP-compatible agents such as Claude Code and Codex can see the screen, click and type through the compositor itself. Cutefish asks permission first, keeps a Stop button in the menu bar while an agent is active, and pauses the moment you touch the pointer or keyboard.

## Getting Cutefish

- **ISO images (`amd64` & `arm64`)**: Download from [download.cutefish.org](https://download.cutefish.org/).
- **Build from source**: Follow [docs/building.md](docs/building.md) to build and install the Cutefish desktop on an existing Linux distribution.

## Documentation

The documentation lives in [`docs/`](docs/README.md) and is published at [cutefish.org/docs](https://cutefish.org/docs).

## Feedback & Contributing

- **Source code**: All public repositories live under [github.com/cutefishos](https://github.com/cutefishos).
- **Issues & feedback**: Open an issue in this repository for general questions, feedback or cross-component bugs, or in the specific component repository.
- **Translations**: See [docs/translating.md](docs/translating.md).
- **Support the project**: [GitHub Sponsors](https://github.com/sponsors/cutefishos).

## License

This repository is licensed under the [BSD-3-Clause License](LICENSE). Individual component repositories may use their own licenses; see each repository's `LICENSE` file for details.
