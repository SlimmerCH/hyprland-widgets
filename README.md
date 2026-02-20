# Hyprland Desktop Widgets

Desktop widgets for **Hyprland**. Based on AGS and Astal.

![Screenshot](./screenshots/screenshot1.png)

## Usage

Run the following.

```bash
desktop
```

You might want to execute this automatically on login.

When changing the theme color using this app, the file `~/.conf/desktop/hypr.conf` is created.

```conf
$primaryColor = rgba(179,165,231,0.6)
```

You may want to include this in your hyprland config to match the border color of active windows.

### Commands

You can display volume and brightness indicators with

```bash
desktop-ctl show volume
```

```bash
desktop-ctl show brightness
```

This will display the volume and brightness for a few seconds in a small widget at the bottom.
These commands should be binded to the corresponding buttons on the keyboard.

## System Requirements

This app requires a standard **Hyprland** setup and assumes the following services running.

- NetworkManager
- BlueZ (Bluetooth)
- Power Profiles Daemon
- WirePlumber
- SWWW (for wallpapers)
- UPower (Even on Desktops)

## Installation

### NixOS & Home Manager

If you are on NixOS or have the Nix package manager installed with **Flakes enabled**, you do not need to manually install dependencies.

#### Step 1: Add the Input

In your system's flake.nix, add this repository to the inputs block.

```nix
{
  inputs = {
    desktop-widgets.url = "github:selimbucher/hyprland-widgets";
  };
}
```

#### Step 2: Append the Package List

In your Home Manager configuration file (usually home.nix), add the package to your home.packages list. You will need to pass the inputs argument to your module.

```nix
{ inputs, pkgs, ... }:

{
  home.packages = [
    inputs.desktop-widgets.packages.${pkgs.system}.default
  ];
}
```

#### Step 3: Add glassify blurer to hyprland

In your Hyprland config add this layerrule and a blur config similar to this (just ensure xray is off):

```nix
wayland.windowManager.hyprland.settings.decoration.blur = {
  enabled = true;
  size = 1;
  passes = 2;
  brightness = 1;
  contrast = 1.4;
  noise = 0;
  new_optimizations = true;
};

wayland.windowManager.hyprland.settings.layerrule = {
  name = "glassify-selimbar";
  blur = true;
  blur_popups = true;
  ignore_alpha = 0.1;
  "match:namespace" = "gtk4-layer-shell";
};
```

(if this was too easy, don't panic: the hyprland maintainer will probably break this config next week to ensure you are not out of work)

### Generic Linux (Arch, Fedora, etc.)

#### Step 1

Clone this repository to a convinient location.

#### Step 2

Install the following dependencies:

- zenity
- sox
  nd install the following [Astal](https://github.com/aylur/astal) libraries.
- io
- astal4
- battery
- network
- hyprland
- wireplumber
- mpris
- powerprofiles
- bluetooth
- tray
  For installing astal packages, refer to the [Wiki](https://aylur.github.io/astal/).

#### Step 3

Start the app by running

```bash
ags run /path/to/project/app.ts
```
