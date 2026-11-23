# NEATO API

Program API environment specifications.

The NEATO API consists of the default \_G, events and handlers of the provided NEET Computers API, with changes and additions
defined by the specifications. Which parts a given operating system provides is determined by `core` and the extensions
it reports, as described in the [main ](../).

This does not require the API to explicitly behave the same as stock NEET Computers API, or that it cannot have additions
from an operating system, but rather that the expected parameters and return types are the same across all OSes.

For example, in a GUI based operating system with applications that each get their own window, it may modify the behavior
of the terminal to draw into the application's window, and the graphics API to use a specific layer for the window,
rather than the main global layer, and remain NEATO compliant.

Arguably the most important spec in the NEATO API is [globals](globals), as it defines the entirety of the NEATO environment for clarity, including things that might not have explicit API specs for whatever reason.

### Replacements for the default API

NEATO does not expose the raw NEET Computers `event` and `files` APIs, because they give programs direct access to
the "hardware" with no protection. NEATO defines its own [event](core/event) and [fs](core/fs) APIs in `core` instead, and
an OS maps them onto whatever it has underneath. The other raw NEET APIs are defined as extensions now:
`headsup` ([headsup](headsup)), `io` ([peripherals](peripherals)), `chip` ([chip](chip)),
`internet` ([internet](internet)), and the disk and partition functions of `files`
([partitions](partitions)).

### `core` (always available)

- [Globals (globals)](globals) - The complete global environment, including the program model: the arguments a
  program receives and the exit status it returns.
- [NEATO requesting (core/requestNeato)](core/requestNeato) - The function for requesting that the environment be changed to a fully NEATO compliant one.
- [Loadfile (core/loadfile)](core/loadfile) - A function for loading a lua file as a function, as in base Lua.
- [Package and require (core/package-require)](core/package-require) - The `require` function and global `package` table, as in base Lua.
- [Terminal emulation (core/term)](core/term) - Functions for a standardized terminal emulator, including reading a line typed on it.
- [System info (core/sys)](core/sys) - OS name and version, extension queries, and sleeping.
- [Events (core/event)](core/event) - Input and other events delivered to a program.
- [Files (core/fs)](core/fs) - Files and directories, using the [paths](../common/paths) format, and [filesystem points](core/fs#filesystem-points).
- [Current working directory (core/cwd)](core/cwd) - The directory the application was launched in.
- [Cryptography (core/crypto)](core/crypto) - Cryptography functions.
- [Error codes (../common/errors)](../common/errors) - The shared codes every failing API returns.

### Extensions

- [Standard streams (stdio)](stdio) - `ext.stdio`
- [Environment (env)](env) - `ext.env`
- [Time (time)](time) - `ext.time`
- [Processes (proc)](proc) - `ext.proc`
- [Job control (job)](job) - `ext.job`
- [Users (user)](user) - `ext.user`
- [Authentication (auth)](auth) - `ext.auth`
- [System (system)](system) - `ext.system`
- [Permissions (perms)](perms) - `ext.perms`
- [Screen (screen)](screen) - `ext.screen`
- [Heads-up display (headsup)](headsup) - `ext.headsup`
- [Peripherals (peripherals)](peripherals) - `ext.peripherals`
- [Partitions and disks (partitions)](partitions) - `ext.partitions`
- [Chip (chip)](chip) - `ext.chip`
- [Internet (internet)](internet) - `ext.internet`
- [Mouse events (mouse)](mouse) - `ext.mouse`
- [Compatibility events (compat)](compat) - `ext.compat`
- [Direct Payload Protocol (../network/dpp)](../network/dpp) - `ext.dpp`
- [Sequenced Payload Protocol (../network/spp)](../network/spp) - `ext.spp`
- [Routed Packet Protocol (../network/rpp)](../network/rpp) - `ext.rpp` (draft)

### Extensions that extend `fs`

Some extensions define no global table of their own and add functions to the [`fs`](core/fs) API instead. An
operating system defines those functions exactly when it reports the extension, so `sys.hasExtension` and a check
for `nil` both work.

- [Permissions (perms)](perms) - `ext.perms`, adds `fs.access`, `fs.chmod`, `fs.chown`, `fs.umask`.
- [Symbolic links (symlink)](symlink) - `ext.symlink` (draft), adds `fs.symlink`, `fs.readlink`.
- [Filesystem usage (statfs)](statfs) - `ext.statfs` (draft), adds `fs.statfs`.
- [Named pipes (fifo)](fifo) - `ext.fifo`, adds `fs.mkfifo`.
- [File times (utimes)](utimes) - `ext.utimes`, adds `fs.utimes`.
