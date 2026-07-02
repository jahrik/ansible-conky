# ansible-conky

[![CICD](https://github.com/jahrik/ansible-conky/actions/workflows/cicd.yml/badge.svg)](https://github.com/jahrik/ansible-conky/actions/workflows/cicd.yml)
[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-jahrik.conky-blue?logo=ansible)](https://galaxy.ansible.com/ui/standalone/roles/jahrik/conky/)

Installs [Conky](https://github.com/brndnmtthws/conky) system monitor and deploys a templated `~/.conkyrc` config file. Supports Arch Linux and Debian/Ubuntu. Font rendering requires [DejaVu Sans Mono Nerd Font](https://github.com/ryanoasis/nerd-fonts), installed automatically via the `jahrik.nerd_fonts` dependency.

## OS Support

| Platform | Install method |
|---|---|
| Arch Linux | `pacman` installs `conky` |
| Debian / Ubuntu | `apt` installs `conky-all` (all protocols and features) |

## Role Variables

| Variable | Default | Description |
|---|---|---|
| `install` | `true` | Set to `false` to uninstall Conky and remove `~/.conkyrc` |
| `conky.font` | `DejaVuSansMono Nerd Font Mono` | Font used in `~/.conkyrc` |
| `conky.update_int` | `2` | Conky update interval in seconds |
| `conky.color1` | `FFCC99` | Primary highlight color (hex, no `#`) |
| `conky.color2` | `33CC33` | Secondary highlight color (hex, no `#`) |
| `conky.color3` | `33CC99` | Tertiary highlight color (hex, no `#`) |
| `conky.interface` | `ansible_default_ipv4.interface` | Network interface shown in bandwidth stats |

## Example Playbook

```yaml
- hosts: all
  roles:
    - jahrik.conky
```

To uninstall:

```yaml
- hosts: all
  vars:
    install: false
  roles:
    - jahrik.conky
```

## Tags

Run or skip parts of the role with tags:

```bash
ansible-playbook playbook.yml --tags conky:install
ansible-playbook playbook.yml --skip-tags conky:uninstall
```

| Tag | Scope |
|---|---|
| `conky` | All role tasks |
| `conky:install` | Install path only |
| `conky:uninstall` | Uninstall path only |

## Testing

```bash
uv sync
source .venv/bin/activate
yamllint .
ansible-lint
molecule test
```

## License

GPLv2

## Author Information

jahrik@gmail.com
