# ansible-conky

Ansible role that installs [Conky](https://github.com/brndnmtthws/conky) system monitor and deploys a templated `~/.conkyrc` config. Supports Arch Linux and Debian/Ubuntu. Depends on `jahrik.nerd_fonts` for [DejaVu Sans Mono Nerd Font](https://github.com/ryanoasis/nerd-fonts).

## Key Variables

| Variable | Default | Description |
|---|---|---|
| `install` | `true` | Set to `false` to uninstall Conky and remove `~/.conkyrc` |
| `conky.font` | `DejaVuSansMono Nerd Font Mono` | Font used in `~/.conkyrc` |
| `conky.update_int` | `2` | Conky update interval in seconds |
| `conky.color1` | `FFCC99` | Primary highlight color (hex, no `#`) |
| `conky.color2` | `33CC33` | Secondary highlight color (hex, no `#`) |
| `conky.color3` | `33CC99` | Tertiary highlight color (hex, no `#`) |
| `conky.interface` | `ansible_default_ipv4.interface` | Network interface shown in bandwidth stats |

## Task Flow

`tasks/main.yml` -> `install.yml` or `uninstall.yml` based on `install | bool`

**install.yml:**
- Arch: `community.general.pacman` installs `conky` with `update_cache: true`
- Debian: `ansible.builtin.apt` installs `conky-all` (the full-featured package) with `cache_valid_time: 3600`
- All: templates `conkyrc.j2` -> `~/.conkyrc` (`become: false`)

**uninstall.yml:** removes `conky` (Arch) or `conky-all` (Debian) package and deletes `~/.conkyrc`

The `conkyrc.j2` template renders uptime, CPU/RAM/swap usage, filesystem usage for `/` and `/home`, network bandwidth on `conky.interface`, and a top-3 process list. Colors and font are fully templated from `defaults/main.yml`.

## Testing

```bash
yamllint .
ansible-lint
molecule test
molecule converge
molecule destroy
```

## CI

- **Lint**: yamllint + ansible-lint
- **Molecule**: Ubuntu + Arch Linux via Docker (`molecule/default`)
- **Release**: publishes to Ansible Galaxy on merge to `main`
