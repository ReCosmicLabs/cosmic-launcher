# cosmic-launcher (fork)

> **This is a fork of [pop-os/cosmic-launcher](https://github.com/pop-os/cosmic-launcher)**, the launcher and
> window switcher of the COSMIC desktop by [System76](https://system76.com). All credit for the launcher itself
> goes to System76 and the upstream contributors. The license is unchanged: **GPL-3.0-only** (see `LICENSE.md`).

## Changes in this fork

Maintained by [eualexandrerrr](https://github.com/eualexandrerrr) for the
[dotfiles](https://github.com/eualexandrerrr/dotfiles) setup. Everything below is a modification of the
original work, as required by section 5 of the GPL.

- **Horizontal, centered window switcher** (`src/app.rs`). In alt-tab mode the layer surface is centered on
  the screen instead of anchored to the top, and the windows are laid out as a horizontal strip of tiles
  (icon, app name, window title), like the Windows switcher. The search launcher is unchanged.

---

Original README follows.

# Cosmic Launcher

Layer Shell frontend for https://github.com/pop-os/launcher. Currently the underlying protocol being used in the plugin for managing toplevels in wayland is defined [here](https://github.com/pop-os/cosmic-protocols/blob/main/unstable/cosmic-toplevel-info-unstable-v1.xml) but it will be switched to use [wlr-foreign-toplevel-management](https://wayland.app/protocols/wlr-foreign-toplevel-management-unstable-v1) when it is ready.

# Building

Cosmic Launcher is set up to build a deb and a Nix flake, but it can be built using just.

Some Build Dependencies:
```
  cargo,
  just,
  intltool,
  appstream-util,
  desktop-file-utils,
  libxkbcommon-dev,
  pkg-config,
  desktop-file-utils,
```

## Build Commands

For a typical install from source, use `just` followed with `sudo just install`.
```sh
just
sudo just install
```

If you are packaging, run `just vendor` outside of your build chroot, then use `just build-vendored` inside the build-chroot. Then you can specify a custom root directory and prefix.
```sh
# Outside build chroot
just clean-dist
just vendor

# Inside build chroot
just build-vendored
sudo just rootdir=debian/cosmic-launcher prefix=/usr install
```

# Translators

Translation files may be found in the i18n directory. New translations may copy the English (en) localization of the project and rename `en` to the desired [ISO 639-1 language code](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes). Translations may be submitted through GitHub as an issue or pull request. Submissions by email or other means are also acceptable; with the preferred name and email to associate with the changes.

# Debugging & Profiling

## Profiling async tasks with tokio-console

To debug issues with asynchronous code, install [tokio-console](https://github.com/tokio-rs/console) and run it within a separate terminal. Then kill the **cosmic-launcher** process a couple times in quick succession to prevent **cosmic-session** from spawning it again. Then you can start **cosmic-launcher** with **tokio-console** support either by running `just tokio-console` from this repository to test code changes, or `env TOKIO_CONSOLE=1 cosmic-launcher` to enable it with the installed version of **cosmic-launcher**.