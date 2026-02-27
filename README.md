# dragonshark-helpers
A list of helper commands. The directory `dragonshark` with all its scripts must be put inside `/opt/Hawa`,
and then symlinked into `/usr/local/bin`.

Different groups of helper commands are given:

1. Network-related commands: Those related to enumerating IP addresses, wireless interfaces, and connecting/disconnecting.
2. Games-related commands: Those related to directories (e.g. saves and games/roms).
3. Experience-related commands: Those related to stuff like sound (set and get the volume).

## Installation

Stepping on this directory, the command to install these commands is:

```shell
sudo cp -R ./dragonshark /opt/Hawa/dragonshark
ls /opt/Hawa/dragonshark | grep dragonshark | xargs -I {} sudo ln -s /opt/Hawa/dragonshark/{} /usr/local/bin/{}
```

## Usage

These commands are meant to be used from the Dragonshark UI apps or indirectly at bootstrap and setup.

They come like this:

### Games and Saves

#### dragonshark-games-enumerate-external-device-dirs

Lists the available external storage device directories (USB sticks or Mini SD cards).
The output is a list (one per line) of allowed directories to store games or data into.

Run it like this:

```shell
dragonshark-games-enumerate-external-device-dirs
```

It returns something like:

```
/media/pi/FOO
/media/pi/BAR
...
```

where those are the actually per-unit mounted directories, for external storages like USB sticks.

#### dragonshark-games-roms-setup

Ensures that, inside a chosen directory, all the 21 directories will exist to store the ROMs.
There's no meaningful output from this command.

Run it like this:

```shell
dragonshark-games-roms-setup /media/pi/SOMETHING-HERE
```

It creates 21 special directories inside that directory. Check the following command to understand
what are those directories' names.

#### dragonshark-games-get-roms-dir

Tells the directory where the games (emulated/ROMs and native/ARM64 ones) are located.
The output is a single line telling the games' storage directory.

Run it like this:

```shell
dragonshark-games-get-roms-dir
```

It returns a single line with something like:

```
/media/pi/GAMES
```

This directory is inferred by inspection of `/home/pi/.emulationstation/es_systems.cfg`, which
will have a perhaps-wrong default vault (e.g. `/media/pi/DATA` - might fail to exist). It must
be checked on setup by the user or a Dragonshark UI application.

The directory must exist for the console to be successful. Typically, it should be (and, as per
architecture it WILL be) a directory among the listed ones in the previous command. It also must
have the following subdirectories:

1. `mame` (MAME-related games, multiple vendors).
2. `atari2600` (ATARI 2600 Video Computer System).
3. `atari5200` (ATARI 5200).
4. `atari7800` (ATARI 7800 ProSystem).
5. `atarijaguar` (ATARI Jaguar).
6. `atarilynx` (ATARI Lynx).
7. `dosbox` (DOSBOX engine built as libretro emulator).
8. `nes` (Nintendo Entertainment System).
9. `snes` (Super Nintendo Entertainment System).
10. `gbcolor` (Nintendo Gameboy Color - also accepts regular Gameboy games).
11. `gba` (Nintendo Gameboy Advance).
12. `genesis` (SEGA Genesis).
13. `mastersystem` (SEGA MasterSystem).
14. `gamegear` (SEGA Game Gear).
15. `segacd` (SEGA CD).
16. `neogeopocket` (NeoGeo Pocket - separate format than MAME packed ones).
17. `neogeocd` (NeoGeo CD - separate format than MAME packed ones).
18. `ps1` (PlayStation 1).
19. `nds` (Nintendo DS).
20. `n64` (Nintendo 64).
21. `dragonshark` (Dragonshark-format Linux ARM64 packed games).

#### dragonshark-games-set-roms-dir

Sets the directory where the games (emulated/ROMs and native/ARM64 ones) will be located.

The specified directory **must** be a directory among the directories returned by the
`dragonshark-games-enumerate-external-device-dirs` command, or it will be an error.

Again: The stated 21 subdirectories (in the previous command) must exist in this new
directory for this command to be effective. Also, for the sake of EmulationStation to
be nice, each subdirectory should have a `downloaded_images` subdirectory itself, where
a `.png` image must exist with its name matching the corresponding ROM's name (without
extension) of the game it represents. Otherwise, the game will not have a portrait image
when using EmulationStation.

#### dragonshark-games-saves-backup

Performs a backup of the current saves, as a ZIP file.
There's no meaningful output from this command.

Run it like this:

```shell
dragonshark-games-saves-backup /media/pi/SOME-UNIT
```

It creates a backup of the current saves (from the directory `/mnt/SAVES`, which will exist
as per setup) as a ZIP file named `backup.zip` into the specified directory. Typically, the
directory will be selected from one of the directories returned by running the command
`dragonshark-games-enumerate-external-device-dirs`.

#### dragonshark-games-saves-restore

Performs a restore of a saves' backup (a ZIP file named `backup.zip`).
There's no meaningful output from this command.

Run it like this:

```shell
dragonshark-games-saves-restore /media/pi/SOME-UNIT
```

It restores a backup of saves from the specified directory (which must contain a fila named
`backup.zip`, which will be the backup already generated with the previous command) into the
`/mnt/SAVES` directory, overwriting any existing save file. *It does not remove previous
contents, but writes on top*.

#### dragonshark-games-saves-setup

Ensures that, inside `/mnt/setup`, all the 21 directories will exist to store the saves.
There's no meaningful output from this command.

Run it like this:

```shell
dragonshark-games-saves-setup
```

It creates these 21 directories inside /mnt/SAVES. They match, just for pure convenience, the
previous 21 names of ROMs directories:

1. `mame` (MAME-related games, multiple vendors).
2. `atari2600` (ATARI 2600 Video Computer System).
3. `atari5200` (ATARI 5200).
4. `atari7800` (ATARI 7800 ProSystem).
5. `atarijaguar` (ATARI Jaguar).
6. `atarilynx` (ATARI Lynx).
7. `dosbox` (DOSBOX engine built as libretro emulator).
8. `nes` (Nintendo Entertainment System).
9. `snes` (Super Nintendo Entertainment System).
10. `gbcolor` (Nintendo Gameboy Color - also accepts regular Gameboy games).
11. `gba` (Nintendo Gameboy Advance).
12. `genesis` (SEGA Genesis).
13. `mastersystem` (SEGA MasterSystem).
14. `gamegear` (SEGA Game Gear).
15. `segacd` (SEGA CD).
16. `neogeopocket` (NeoGeo Pocket - separate format than MAME packed ones).
17. `neogeocd` (NeoGeo CD - separate format than MAME packed ones).
18. `ps1` (PlayStation 1).
19. `nds` (Nintendo DS).
20. `n64` (Nintendo 64).
21. `dragonshark` (Dragonshark-format Linux ARM64 packed games).

### Network commands

Wi-Fi is supported by this console, although typically via an external dongle.

While emulated games seldom use Wi-Fi, Dragonshark / Native games might make use of it.
Standard Debian commands apply here.

#### dragonshark-network-list-wlan-interfaces

Lists the available Wi-Fi network interfaces in the console or computer. If it's in
the console and does not have a Wi-Fi dongle installed, this command will not show
any network interface. Otherwise, it's typically `wlan0`. In Wi-Fi-enabled devices
with a modern Ubuntu version, the built-in supported Wi-Fi interface will have the
name `wlo1` instead.

Run it like this:

```shell
dragonshark-network-list-wlan-interfaces
```

It returns, on each line, the name of a supported and available Wi-Fi interface.

#### dragonshark-network-connect

Attempts to connect to a Wi-Fi network.

Run it like this:

```shell
# An open network.
dragonshark-network-connect SOME-NETWORK wlan0 
```

Or perhaps:

```shell
# A private network.
dragonshark-network-connect SOME-NETWORK "some-password" wlan0.
```

The first argument is a network SSID.

The last argument is a network interface (in this case wlan0) and **must** be one
returned by `dragonshark-network-list-wlan-interfaces`

The middle argument, if provided, is the network's password.

The underlying command being used is `nmcli`.

#### dragonshark-network-disconnect

Attempts to disconnect from the current Wi-Fi network.

Run it like this:

```shell
dragonshark-network-disconnect wlan0
```

The interface (in this case wlan0) **must** be one returned by the command
`dragonshark-network-list-wlan-interfaces`.

#### dragonshark-network-list-wireless-networks

Lists the available public networks, and which one is an active network, if any.

Run it like this:

```shell
dragonshark-network-list-wireless-networks
```

It will return a result where each line will have several colon-separated fields, in order:

1. Active field: `no` (not connected) or `yes`. If `yes`, then the console or computer is
   connected to that network (many rows with `yes`) can exist if there are many Wi-Fi
   interfaces, and they happen to be connected to some network.
2. The SSID (public name, say) of the network.
3. The strength (1-100) of the signal to that network.
4. The name of the interface (e.g. `wlan0` that detected that network entry).
5. The security schemes supported by that network. This is a list of codenames separated by a space.

Example output:

```
no:MY_NETWORK:97:wlan0:WPA1 WPA2
yes:MY_NETWORK:74:wlan0:WPA1 WPA2
```

#### dragonshark-network-list-ipv4-interfaces

This command is used to tell which IPv4 interfaces are available in this computer. This
typically serves the purpose of telling, in the same Wi-Fi network, what's the IP other
devices must connect to (e.g. the VirtualPad server UI uses this command as a hint to
the players to successfully connect client pads from their mobile devices to the console).

Run it like this:

```shell
dragonshark-network-list-ipv4-interfaces
```

It will return a result where each line will have several comma-separated fields, in order:

1. The IP address.
2. Whether it's a `loopback` address, a `lan` (LAN-reachable) address, or an unknown
   IPv4 address type.
3. Whether it's a WLAN-reachable address, in particular. In this case, it will be also a
   `lan` address, but not all the LAN-reachable addresses are WLAN-reachable addresses,
   because computers and consoles also support wired LAN adapters, which do not serve any
   purpose to services like VirtualPad server.

### Bluetooth commands

Bluetooth is supported in this console, although typically via an external dongle.

This is typically intended for Gamepad bluetooth devices, but this typically works with
any kind of bluetooth device supported by the console.

#### dragonshark-bluetooth-list-unpaired-devices

Lists the unpaired devices. This is done by scanning nearby the available bluetooth
devices attempting a scan on their side, and returning them. This scan takes some time
in seconds, specified by the user.

Run it like this:

```shell
# These two commands are equivalent.
dragonshark-bluetooth-list-unpaired-devices
dragonshark-bluetooth-list-unpaired-devices 6
# Or choose a different amount of seconds.
dragonshark-bluetooth-list-unpaired-devices 10
```

It returns a list like this:

```
01:11:21:31:41:51:61 GamePad
02:12:22:32:42:52:62 SomethingElse
...
```

The device gets paired, trusted and connected.

#### dragonshark-bluetooth-pair-device

Attempts to pair a device. It returns a success or failure status code from the process.

Run it like this:

```shell
# These two alternatives.
dragonshark-bluetooth-pair-device 01:11:21:31:41:51
dragonshark-bluetooth-pair-device GamePad
# These ones are equivalent to the previous ones.
dragonshark-bluetooth-pair-device 01:11:21:31:41:51 6
dragonshark-bluetooth-pair-device GamePad 6
# Or choose a different amount of seconds.
dragonshark-bluetooth-pair-device 01:11:21:31:41:51 10
dragonshark-bluetooth-pair-device GamePad 10
```

#### dragonshark-bluetooth-list-paired-devices

Lists the unpaired devices. This is done by just telling the paired
devices in the console.

Run it like this:

```shell
dragonshark-bluetooth-list-paired-devices
```

It returns a list like this:

```
# The yes and no in third column state whether they're connected or not.
01:11:21:31:41:51:61 GamePad yes
02:12:22:32:42:52:62 SomethingElse no
...
```

#### dragonshark-bluetooth-unpair-device

Attempts to unpair a device. It returns a success or failure status code from the process.

Run it like this:

```shell
# These two alternatives.
dragonshark-bluetooth-unpair-device 01:11:21:31:41:51
dragonshark-bluetooth-unpair-device GamePad
# These ones are equivalent to the previous ones.
dragonshark-bluetooth-unpair-device 01:11:21:31:41:51 6
dragonshark-bluetooth-unpair-device GamePad 6
# Or choose a different amount of seconds.
dragonshark-bluetooth-unpair-device 01:11:21:31:41:51 10
dragonshark-bluetooth-unpair-device GamePad 10
```

The device gets unpaired, untrusted and disconnected.

#### dragonshark-bluetooth-connect-device

Attempts to connect to a paired, but disconnected, device. It returns a success or failure status code from the process.

Run it like this:

```shell
# These two alternatives.
dragonshark-bluetooth-connect-device 01:11:21:31:41:51
dragonshark-bluetooth-connect-device GamePad
# These ones are equivalent to the previous ones.
dragonshark-bluetooth-connect-device 01:11:21:31:41:51 6
dragonshark-bluetooth-connect-device GamePad 6
# Or choose a different amount of seconds.
dragonshark-bluetooth-connect-device 01:11:21:31:41:51 10
dragonshark-bluetooth-connect-device GamePad 10
```

It's still a success if the device is already trusted and/or already connected.

### Experience-related commands

Miscellaneous commands are included here (e.g. to manage the system's sound).

For the sound, these commands work if ALSA Mixer is available and also an entry named
`Master` is supported (which is true in DragonShark consoles, but not necessarily true
for regular Ubuntu systems).

#### dragonshark-start

Starts the DragonShark experience on the device, with an LXDE profile.

Run it like this:

```shell
# Thees two are equivalent:
dragonshark-start
dragonshark-start LDXE-pi
# General case:
dragonshark-start SOME-LXDE-PROFILE
```

If the ~/.run-in-debug-mode file is present (it doesn't matter what it contains), then
it will be deleted and the startup will be regular (for a linux user: desktop, background
and all the other elements).

Otherwise, the regular experience will occur: No desktop, no background, no applications
bar and only the trigger of `dragonshark-ui` app, full-screen.

In order to start the device in debug mode, run this command in a shell:

```shell
touch ~/.run-in-debug-mode && reboot
```

#### dragonshark-sound-set-volume

Sets the Master volume. This command does not return a meaningful output.

Run it like this:

```shell
# Set a value 0 to 100 here.
dragonshark-sound-set-volume 50
```

#### dragonshark-sound-get-volume

Retrieves the Master volume.

Run it like this:

```shell
dragonshark-sound-get-volume
```

The result is a single number 0 to 100.

#### dragonshark-video-fix-resolution

Sets the resolution to 1920x1080.

Run it like this:

```shell
dragonshark-video-fix-resolution
```