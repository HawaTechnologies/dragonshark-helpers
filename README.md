# dragonshark-helpers
A list of helper commands. The directory `dragonshark` with all its scripts must be put inside `/opt/Hawa`,
and then symlinked into `/usr/local/bin`.

Two groups of helper commands are given:

1. Network-related commands: Those related to enumerating IP addresses, wireless interfaces, and connecting/disconnecting.
2. Games-related commands: Those related to directories (e.g. saves and games/roms).

Stepping on this directory, the command to install these commands is:

```shell
sudo cp -R ./dragonshark /opt/Hawa/dragonshark
ls /opt/Hawa/dragonshark | grep dragonshark | xargs -I {} sudo ln -s /opt/Hawa/dragonshark/{} /usr/local/bin/{}
```