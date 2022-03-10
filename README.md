# Ansible Role: Black Mesa

An Ansible role that installs and configures a Black Mesa dedicated server.

The game server is downloaded through Steam and exposed as a systemd service for easier management.
Using this role it is possible to publish a minimalist deathmatch server.

Automatic testing is provided using molecule's delegated driver and <https://builds.sr.ht>.

## Requirements

An ansible role dedicated to the installation of SteamCMD such as [tleguern.steamcmd](https://github.com/tleguern/ansible-steamcmd).
This role should provide the `steam_home` variable, pointing to such a folder as `/home/steam/Steam` or `/home/steam/.steam` depending on your operating system.

## Role Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `steamcmd_user` | User name for steamcmd | `steam` |
| `steamcmd_bin` | Path to the steamcmd executable | `/usr/games/steamcmd` |
| `black_mesa_mapcycle` | List of maps for the rotation | See below |
| `black_mesa_server_cfg` | Server configuration | See below |
| `black_mesa_port` | Network port | `27015` |
| `black_mesa_ip` | IP address to listen on | `0.0.0.0` |
| `black_mesa_maxplayers` | Max number of players | `24` |
| `black_mesa_start_map` | First map to use on server start, without prefix | `gasworks` |

### `black_mesa_mapcycle`

The mapcycle file is a simple list of maps loaded on the server.

There is no default value.

Example:

```
dm_gazworks
dm_chopper
dm_power
```

### `black_mesa_server_cfg`

The server.cfg file is the main configuration file for the server.
It holds the server name, the administrator password as well as various rules.

There is no default value.

Example:

```
hostname "My Black Mesa Deathmatch Server"
rcon_password mypassword
mp_fraglimit 30
sv_always_run 1
```

## Dependencies

The `acl` package should be installed on the server.

## Example Playbook

```yaml
- hosts: game
  vars:
    black_mesa_server_cfg: |
      hostname "[BMS] Chopper only"
      rcon_password rconpassword
    black_mesa_start_map: chopper
    black_mesa_mapcycle: |
      dm_chopper
  pre_tasks:
    - package:
        name: acl
        state: present
  roles:
    - role: tleguern.steamcmd
    - role: tleguern.black_mesa
```

## License

ISC

## Contributing

Either send [send GitHub pull requests](https://github.com/tleguern/ansible-role-black-mesa) or [send patches on SourceHut](https://lists.sr.ht/~tleguern/misc).

## Author Information

Tristan Le Guern <tleguern@bouledef.eu>
