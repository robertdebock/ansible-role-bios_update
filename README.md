# bios_update

Download, extract and write bootable USB image.

|Travis|GitHub|Quality|Downloads|
|------|------|-------|---------|
|[![travis](https://travis-ci.com/robertdebock/ansible-role-bios_update.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-bios_update)|[![github](https://github.com/robertdebock/ansible-role-bios_update/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-bios_update/actions)|[![quality](https://img.shields.io/ansible/quality/39155)](https://galaxy.ansible.com/robertdebock/bios_update)|[![downloads](https://img.shields.io/ansible/role/d/39155)](https://galaxy.ansible.com/robertdebock/bios_update)|

## Example Playbook

This example is taken from `molecule/resources/converge.yml` and is tested on each push, pull request and release.
```yaml
---
- name: converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: robertdebock.bios_update
      # The bios_update_url does not always need to be set, it's typically
      # "discovered". For CI however, there is not right model, so this
      # variable needs to be set manually.
      bios_update_url: "https://download.lenovo.com/pccbbs/mobiles/r02uj70d.iso"
```

The machine may need to be prepared using `molecule/resources/prepare.yml`:
```yaml
---
- name: prepare
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - role: robertdebock.bootstrap
```

For verification `molecule/resources/verify.yml` run after the role has been applied.
```yaml
---
- name: Verify
  hosts: all
  become: yes
  gather_facts: yes

  tasks:
    - name: check if connection still works
      ping:
```

Also see a [full explanation and example](https://robertdebock.nl/how-to-use-these-roles.html) on how to use these roles.

## Role Variables

These variables are set in `defaults/main.yml`:
```yaml
---
# defaults file for bios_update

# Where to store the downloaded ISO.
bios_update_temporary_directory: /tmp

# The URL to a bootable ISO containing a BIOS update.
# The URL is "discovered" in `vars/main.yml`, but can be overwritten here.
# or in any variable that takes precedence.
#
# bios_update_url: "https://download.lenovo.com/pccbbs/mobiles/r02uj70d.iso"

# The device to write the bootable image to.
#
# WARNING: THIS DEVICE WILL BE OVERWRITTEN.
#
bios_update_flash_drive: "/dev/sdCHANGEME"
```

## Requirements

- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible. (Tests run on the current, previous and next release of Ansible.)

The following roles can be installed to ensure all requirements are met, using `ansible-galaxy install -r requirements.yml`:

```yaml
---
- robertdebock.bootstrap

```

## Context

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/bios_update.png "Dependency")

## Compatibility

This role has been tested on these [container images](https://hub.docker.com/):

|container|tags|
|---------|----|
|amazon|2018.03|
|el|7, 8|
|debian|buster, bullseye|
|fedora|31, 32|
|ubuntu|focal, bionic, xenial|

The minimum version of Ansible required is 2.8 but tests have been done to:

- The previous version, on version lower.
- The current version.
- The development version.

## Exceptions

Some variarations of the build matrix do not work. These are the variations and reasons why the build won't work:

| variation                 | reason                 |
|---------------------------|------------------------|
| Alpine | required by: world[geteltorito] |
| Archlinux | required by: world[geteltorito] |
| RedHat | No package genisoimage available |
| Suse | No provider of geteltorito found |


## Testing

[Unit tests](https://travis-ci.com/robertdebock/ansible-role-bios_update) are done on every commit, pull request, release and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-bios_update/issues)

Testing is done using [Tox](https://tox.readthedocs.io/en/latest/) and [Molecule](https://github.com/ansible/molecule):

[Tox](https://tox.readthedocs.io/en/latest/) tests multiple ansible versions.
[Molecule](https://github.com/ansible/molecule) tests multiple distributions.

To test using the defaults (any installed ansible version, namespace: `robertdebock`, image: `fedora`, tag: `latest`):

```
molecule test

# Or select a specific image:
image=ubuntu molecule test
# Or select a specific image and a specific tag:
image="debian" tag="stable" tox
```

Or you can test multiple versions of Ansible, and select images:
Tox allows multiple versions of Ansible to be tested. To run the default (namespace: `robertdebock`, image: `fedora`, tag: `latest`) tests:

```
tox

# To run CentOS (namespace: `robertdebock`, tag: `latest`)
image="centos" tox
# Or customize more:
image="debian" tag="stable" tox
```

## License

Apache-2.0


## Author Information

[Robert de Bock](https://robertdebock.nl/)

Please consider [sponsoring me](https://github.com/sponsors/robertdebock).
