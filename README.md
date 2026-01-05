# dragonshark-helpers
A list of helper commands. The directory `dragonshark` with all its scripts must be put inside `/opt/Hawa`,
and then symlinked into `/usr/local/bin`.

Different groups of helper commands are given:

1. Network-related commands: Those related to enumerating IP addresses, wireless interfaces, and connecting/disconnecting.
2. Games-related commands: Those related to directories (e.g. saves and games/roms).
3. Sound-related commands: Those related to sound (set and get the volume).

## Installation

Stepping on this directory, the command to install these commands is:

```shell
sudo cp -R ./dragonshark /opt/Hawa/dragonshark
ls /opt/Hawa/dragonshark | grep dragonshark | xargs -I {} sudo ln -s /opt/Hawa/dragonshark/{} /usr/local/bin/{}
```

## Usage

These commands are meant to be used from the Dragonshark UI apps or indirectly at bootstrap and setup.

They come like this:

### dragonshark-games-enumerate-external-device-dirs

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

### dragonshark-games-get-roms-dir

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

### dragonshark-games-set-roms-dir

Sets the directory where the games (emulated/ROMs and native/ARM64 ones) will be located.

The specified directory **must** be a directory among the directories returned by the
`dragonshark-games-enumerate-external-device-dirs` command, or it will be an error.

Again: The stated 21 subdirectories (in the previous command) must exist in this new
directory for this command to be effective. Also, for the sake of EmulationStation to
be nice, each subdirectory should have a `downloaded_images` subdirectory itself, where
a `.png` image must exist with its name matching the corresponding ROM's name (without
extension) of the game it represents. Otherwise, the game will not have a portrait image
when using EmulationStation.

### dragonshark-games-saves-backup

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

### dragonshark-games-saves-restore

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

### dragonshark-games-saves-setup

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
