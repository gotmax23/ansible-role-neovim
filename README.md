# Ansible Role: neovim
[![Role gotmax23.neovim][badge-role]][link-galaxy]
[![Github Repo][badge-github-repo]][link-github-repo]
[![SourceHut Repo][badge-srht-repo]][link-srht-repo]
[![MIT Licensed][badge-license]][link-license]
[![Github Open Issues][badge-github-issues]][link-github-issues]
[![Github Open PRs][badge-github-prs]][link-github-prs]
[![Role Version][badge-version]][link-version]
[![Commits since last version][badge-commits-since]][link-version]
[![Galaxy Role Quality][badge-quality]][link-galaxy]
[![Galaxy Role Downloads][badge-downloads]][link-galaxy]
[![Github Actions Molecule workflow status][badge-molecule-workflow]][link-molecule-workflow]
[![Github Actions Galaxy workflow status][badge-galaxy-workflow]][link-galaxy-workflow]

Ansible role that install the neovim text editor.

## Beta Warning
**This role is currently in beta and is not intended for production use. Breaking changes may occur between releases, so please make sure to read the release notes.**
## Requirements

This role depends on certain collections that are not included in ansible-core.

This role's [example playbook](#example-playbook) requires another role to prepare the target system.

To install this role's requirements, create a `requirements.yml` file with the following contents:

``` yaml
# SPDX-FileCopyrightText: 2022 Maxwell G (@gotmax23)
# SPDX-License-Identifier: CC0-1.0
---
roles:
  - name: robertdebock.epel

collections:
  - name: community.general

```

Then, if you are using ansible-base/ansible-core 2.10 or later, run this command.

``` shell
ansible-galaxy install -r requirements.yml
```


If you are still using Ansible 2.9, run these commands, instead.

``` shell
ansible-galaxy collection install -r requirements.yml
ansible-galaxy role install -r requirements.yml
```

## Role Variables

Here are this role's variables and their default values, as set in [`defaults/main.yml`][link-defaults]. If you'd like, you may change them to customize this role's behavior.

``` yaml
# SPDX-FileCopyrightText: 2022 Maxwell G (@gotmax23)
# SPDX-License-Identifier: CC0-1.0
---
# Options:
# - `present` ensures that Neovim is installed.
# - `absent` ensures that Neovim is not installed.
neovim_state: present


# Whether to create a symlink from /usr/bin/nvim to neovim_symlink_dest.
neovim_symlink_create: true

neovim_symlink_dest: /usr/local/bin/vim


# Whether to install a neovim config file
neovim_install_config: true

# Which template to use for the neovim config file.
# The default template makes neovim more compatible with vim by sourcing the
# ~/.vimrc and plugins stored in ~/.vim/pack/*/start.
neovim_config_template: "init.vim.j2"

neovim_config_dir: "/home/{{ neovim_config_owner }}/.config/nvim"

neovim_config_owner: "{{ ansible_user_id }}"

neovim_config_group: "{{ neovim_config_owner }}"

neovim_config_dest: "{{ neovim_config_dir }}/init.vim"

```

## Example Playbook
``` yaml
# SPDX-FileCopyrightText: 2022 Maxwell G (@gotmax23)
# SPDX-License-Identifier: CC0-1.0
---
- name: Install Neovim
  hosts: all
  become: true

  tasks:
    - name: Update apt cache
      when: ansible_pkg_mgr == "apt"
      ansible.builtin.apt:
        update_cache: true
        cache_valid_time: 3600

    - name: Install EPEL Repo (will only run on EL-based distros)
      ansible.builtin.include_role:
        name: robertdebock.epel

    - name: Install Neovim
      ansible.builtin.include_role:
        name: "gotmax23.neovim"

```

## Compatibility
This role is tested using the latest version of ansible-core and the latest version of the collections from Ansible Galaxy. This is the only version of Ansible that this role officially supports. Best effort support is provided for other versions.

This role is compatible with the following distros:

|distro|versions|
|------|--------|
|Archlinux|any|
|Debian|buster, bullseye|
|EL|7, 8|
|Fedora|34, 35, 36|
|opensuse|15.3, tumbleweed|
|Ubuntu|bionic, focal|

## License
[MIT][link-license]

## Author
Maxwell G (@gotmax23)

[badge-role]: https://img.shields.io/ansible/role/58064.svg?logo=ansible
[link-galaxy]: https://galaxy.ansible.com/gotmax23/neovim
[badge-github-repo]: https://img.shields.io/static/v1?label=GitHub&message=repo&color=blue&logo=github
[link-github-repo]: https://github.com/gotmax23/ansible-role-neovim
[badge-srht-repo]: https://img.shields.io/static/v1?label=SourceHut&message=repo&color=blue&logo=data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiIHN0YW5kYWxvbmU9Im5vIj8+CjxzdmcKICAgdmlld0JveD0iMCAwIDUxMiA1MTIiCiAgIHZlcnNpb249IjEuMSIKICAgaWQ9InN2ZzUxIgogICBzb2RpcG9kaTpkb2NuYW1lPSJzb3VyY2VodXQtd2hpdGUuc3ZnIgogICBpbmtzY2FwZTp2ZXJzaW9uPSIxLjEgKGM2OGUyMmMzODcsIDIwMjEtMDUtMjMpIgogICB4bWxuczppbmtzY2FwZT0iaHR0cDovL3d3dy5pbmtzY2FwZS5vcmcvbmFtZXNwYWNlcy9pbmtzY2FwZSIKICAgeG1sbnM6c29kaXBvZGk9Imh0dHA6Ly9zb2RpcG9kaS5zb3VyY2Vmb3JnZS5uZXQvRFREL3NvZGlwb2RpLTAuZHRkIgogICB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciCiAgIHhtbG5zOnN2Zz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciPgogIDxkZWZzCiAgICAgaWQ9ImRlZnM1NSIgLz4KICA8c29kaXBvZGk6bmFtZWR2aWV3CiAgICAgaWQ9Im5hbWVkdmlldzUzIgogICAgIHBhZ2Vjb2xvcj0iIzUwNTA1MCIKICAgICBib3JkZXJjb2xvcj0iI2ZmZmZmZiIKICAgICBib3JkZXJvcGFjaXR5PSIxIgogICAgIGlua3NjYXBlOnBhZ2VzaGFkb3c9IjAiCiAgICAgaW5rc2NhcGU6cGFnZW9wYWNpdHk9IjAiCiAgICAgaW5rc2NhcGU6cGFnZWNoZWNrZXJib2FyZD0iMSIKICAgICBzaG93Z3JpZD0iZmFsc2UiCiAgICAgaW5rc2NhcGU6em9vbT0iMS42NTQyOTY5IgogICAgIGlua3NjYXBlOmN4PSIyNTYiCiAgICAgaW5rc2NhcGU6Y3k9IjI1NiIKICAgICBpbmtzY2FwZTp3aW5kb3ctd2lkdGg9IjE5MjAiCiAgICAgaW5rc2NhcGU6d2luZG93LWhlaWdodD0iMTA1OSIKICAgICBpbmtzY2FwZTp3aW5kb3cteD0iMCIKICAgICBpbmtzY2FwZTp3aW5kb3cteT0iMCIKICAgICBpbmtzY2FwZTp3aW5kb3ctbWF4aW1pemVkPSIxIgogICAgIGlua3NjYXBlOmN1cnJlbnQtbGF5ZXI9InN2ZzUxIiAvPgogIDxwYXRoCiAgICAgZD0iTTI1NiA4QzExOSA4IDggMTE5IDggMjU2czExMSAyNDggMjQ4IDI0OCAyNDgtMTExIDI0OC0yNDhTMzkzIDggMjU2IDh6bTAgNDQ4Yy0xMTAuNSAwLTIwMC04OS41LTIwMC0yMDBTMTQ1LjUgNTYgMjU2IDU2czIwMCA4OS41IDIwMCAyMDAtODkuNSAyMDAtMjAwIDIwMHoiCiAgICAgaWQ9InBhdGg0OSIKICAgICBzdHlsZT0iZmlsbDojZmZmZmZmIiAvPgo8L3N2Zz4K
[link-srht-repo]: https://git.sr.ht/~gotmax23/ansible-role-neovim
[badge-license]: https://img.shields.io/github/license/gotmax23/ansible-role-neovim.svg?logo=github
[link-license]: https://github.com/gotmax23/ansible-role-neovim/blob/main/LICENSE
[badge-github-issues]: https://img.shields.io/github/issues/gotmax23/ansible-role-neovim.svg?logo=github
[link-github-issues]: https://github.com/gotmax23/ansible-role-neovim/issues
[badge-github-prs]: https://img.shields.io/github/issues-pr/gotmax23/ansible-role-neovim.svg?logo=github
[link-github-prs]: https://github.com/gotmax23/ansible-role-neovim/pulls
[badge-version]: https://img.shields.io/github/release/gotmax23/ansible-role-neovim.svg?logo=github
[link-version]: https://github.com/gotmax23/ansible-role-neovim/releases/latest
[badge-commits-since]: https://img.shields.io/github/commits-since/gotmax23/ansible-role-neovim/latest.svg?logo=github
[badge-quality]: https://img.shields.io/ansible/quality/58064.svg?logo=ansible
[badge-downloads]: https://img.shields.io/ansible/role/d/58064.svg?logo=ansible
[badge-molecule-workflow]: https://github.com/gotmax23/ansible-role-neovim/actions/workflows/molecule.yml/badge.svg?branch=main
[link-molecule-workflow]: https://github.com/gotmax23/ansible-role-neovim/actions/workflows/molecule.yml
[badge-galaxy-workflow]: https://github.com/gotmax23/ansible-role-neovim/actions/workflows/galaxy.yml/badge.svg
[link-galaxy-workflow]: https://github.com/gotmax23/ansible-role-neovim/actions/workflows/galaxy.yml
[link-defaults]: https://github.com/gotmax23/ansible-role-neovim/blob/main/defaults/main.yml
