# ssh

[![Build Status](https://drone.osshelp.ru/api/badges/ansible/ssh/status.svg)](https://drone.osshelp.ru/ansible/ssh)

Role for installing openssh-server and managing sshd configuration.

## Usage (example)

```yaml
    - role: ssh
      sshd_install_openssh_server: true
      sshd_keys_source_dir: 'files/ssh/server_keys'
      sshd_params:
        - { key: PermitRootLogin, value: 'without-password' }
```

## Available parameters

### Main

| Param | Default | Description |
| -------- | -------- | -------- |
| `sshd_install_openssh_server` | `false` | Whether to install openssh-server via apt. |
| `sshd_default_params` | `[{ key: 'DebianBanner', value: 'no' }]` | List of default sshd parameters |
| `sshd_params` | `[]` | List of custom sshd parameters |
| `sshd_conf_path` | `/etc/ssh/sshd_config` | Path to sshd configuration file |
| `sshd_keys_source_dir` | `''` | If source path given, keys from this directory will be copied to target host. Override for cloud-init will be created to disable automatic key recreation. All keys must be encrypted with Ansible-vault. |

## FAQ

### Static ssh keys

You can build container via molecule, take the generated keys from there, encrypt them and put in your build repository (e.g. in `files/ssh/server_keys`). Then, use `sshd_keys_source_dir` param to give the role the source path to operate with. Make sure to keep original file names while transfering keys to repository.

### Managing HostKey

There could be problems if you'll try to set custom value for HostKey param via `sshd_params`. Сurrent HostKey values in the config will be overwritte and only the last passed will be kept. For example:

```yaml
      sshd_params:
        - { key: HostKey, value: '/etc/ssh/ssh_host_ecdsa_key' }
        - { key: HostKey, value: '/etc/ssh/ssh_host_ed25519_key' }
```

Will result in a single param:

```conf
HostKey /etc/ssh/ssh_host_ed25519_key
```

## Useful links

- [Official documentation](https://www.ssh.com/ssh/sshd_config/)

## TODO

- ...

## License

GPL3

## Author

OSSHelp Team, see <https://oss.help>
