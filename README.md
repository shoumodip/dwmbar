# Dwmbar
A modular status bar for the [dwm](https://dwm.suckless.org) written in POSIX compliant shell.

## Installation
```console
$ git clone https://git@github.com:shoumodip/dwmbar
$ cd dwmbar
$ sudo mv dwmbar /usr/bin/
$ sudo mv config.dwm ~/.config/
```

## Configuration
Dwmbar is configured through editing the `~/.config/config.dwm` file. There are three options which can be customized.

### `status_cache_file`
The cache file for the statusbar text. It defaults to `~/.cache/dwmstatus`

It is where `dwmbar` stores the results of the modules defined in the statusbar. This is a major performance booster and is the sole reason this script is described as being *modular*. Each and every single module can be updated independently because the results of the other modules is cached in a single file.

### `status_modules`
The list of modules which constitute the statusbar. It is a newline separated series of commands which get executed. The commands can only be a *single* command, no shell scripting or piping is allowed here.

Here's an example:
```sh
status_modules="
volume
clock
"
```

This configuration defines a statusbar with two modules: `volume` and `clock`. Both of these are scripts located in `$PATH`.

You might me questioning as to why pipes are not allowed? I mean they *are* the holy grail of shell scripting, right? And there's a simple reason for that: **Timers**

Here's an example:
```sh
status_modules="
volume
clock|60
"
```

The syntax for a timed module is `<command>|<seconds>`. So in the example above, the `clock` module will be refreseh every 60 seconds, which makes perfect sense. A clock is *supposed* to update, that's what it's there for in the first place!

## Actions
There are a grand total of 4 actions which `dwmbar` exposes to you. These are useful in scripts, where you might want to control the statusbar.

The syntax for executing an action:
```console
$ dwmbar [action] [...]
```

### `display`
Render the status text in the DWM statusbar.

Command:
```console
$ dwmbar display
```

Requires:
- `xsetroot`
- `cut`
- `tr`
- `sed`

### `refresh`
Refresh a module. Takes the name of the module as the argument.

Command:
```console
$ dwmbar refresh [module]
```

Let's take an example from the configuration shown above. The task is to update the `volume` module:
```console
$ dwmbar refresh volume
```

Requires:
- `sed`
- `eval`
- `echo`

### `reload`
Reload the statusbar

Command:
```console
$ dwmbar reload
```

Requires:
- `cut`
- `echo`
- `eval`

Constructs used:
- `for ... in; do ... done`

### `loop`
Start the loop of the statusbar. Use this if you have some modules which are to repeated after a specific interval.

Command:
```console
$ dwmbar loop
```

Requires:
- `cut`
- `echo`
- `eval`
- `expr`
- `sleep`

Constructs used:
- `for ... in; do ... done`
- `while ... ; do ... done`
- `case ... esac`
- `if [ ... ] ; then ... fi`

## Example
Here's an example configuration

```sh
#!/bin/sh
# File: ~/.config/config.dwm

status_cache_file=~/.cache/dwmstatus

status_modules="
network
cpu
ram
kernel
updates
bitcoin
weather
volume
clock|1
"

modules_separator=" "
```
