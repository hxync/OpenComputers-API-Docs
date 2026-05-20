# opencomputers

## Summary

This page is the high-level system index for the base OpenComputers distribution documented in this repository. Instead of listing raw asset paths, it explains how the bundled OpenComputers content is organized and where to look for the APIs that matter during Lua development.

## Scope

The `opencomputers` system covers the content that ships with the core OpenComputers mod, including:

- Core runtime libraries such as `computer`, `component`, and `event`
- Built-in components such as GPUs, screens, filesystems, robots, and redstone devices
- OpenOS commands and bundled userland programs
- Documentation pages for standard blocks, items, upgrades, and gameplay systems

## How To Use This Documentation

When you are writing Lua code for OpenComputers, you usually need one of these sections:

- `core-runtime/`
  Use this for machine-global runtime libraries such as `computer`.
- `components/`
  Use this for component callbacks exposed by blocks, cards, upgrades, and devices.
- `commands/`
  Use this for bundled shell commands from OpenOS, Plan9k, SecureOS, and related systems.
- `libraries/`
  Use this for reusable Lua modules bundled with the operating systems.
- `systems/`
  Use this for overview pages like this one that explain how a larger subsystem is structured.

## Main Areas In The Base Mod

### Runtime APIs

These are always the first stop when you need machine-level behavior:

- `computer`: memory, uptime, energy, users, architecture
- `component`: component lookup and proxy access
- `event`: signal queue and event handling
- `unicode`: Unicode-safe string helpers

### Core Components

These pages describe the APIs most OpenComputers programs touch regularly:

- Display and input: `gpu`, `screen`, `keyboard`
- Storage: `filesystem`, `drive`, `eeprom`
- Robotics and movement: `robot`, `navigation`, `inventory_controller`, `tractor_beam`
- Networking: `modem`, `internet`, linked and wireless cards
- Sensors and world access: `geolyzer`, `motion_sensor`, `waypoint`
- Automation: `redstone`, `transposer`, `tank_controller`

### Operating-System Userland

The base mod also ships with a large amount of userland content:

- OpenOS shell commands such as `ls`, `cp`, `find`, `ps`, and `free`
- Libraries used by those commands and by normal Lua programs
- Installer scripts and bundled examples

## Notes

- This page is intentionally an overview page, not a raw source inventory.
- Detailed API signatures belong on the specific component, command, or library pages.
- If you are looking for a specific callable interface, prefer the matching page under `components/`, `commands/`, `libraries/`, or `core-runtime/`.

## Recommended Starting Points

- New to OpenComputers scripting:
  Start with `core-runtime/computer.md`, `core-runtime/events.md`, and the main component pages for `gpu`, `screen`, and `filesystem`.
- Writing robot automation:
  Start with `components/robot.md`, `components/opencomputers-robot-proxy.md`, `components/navigation.md` related pages, and inventory or redstone helpers.
- Debugging system behavior:
  Check the relevant component page first, then the matching command or library page if the behavior is exposed through OpenOS tools.
