ansible-initial-server
======================

Initial configuration of a freshly installed Debian server to be used as a template in KVM.

Requirements
------------

### Net-Install from ISO
Tested with a default install of Debian 10 ISO
- installed from debian-10.x.y-amd64-netinst.iso
- Software: Standard System Utilities and SSH Server
- Default user `deploy`
- Partitions: 1 primary ext4 root partition /dev/sda1 (no swap); Grub on MBR
- an auto-install http/preseed.cfg can be used to speed-up the initial install

### After Installation from ISO
- check networking, that the VM reachable via SSH
- no need to change `sshd_config` to `PermitRootLogin yes`, because Ansible can authenticate with the initial user password and then 'become' root using 'su'.

Role Variables
--------------

`iserver_repos`
- List of repositories which will replace the initial defaults

`iserver_packages`
- List of system utilities to install

`iserver_user`
- Username of the default user

`iserver_sshkey`
- SSH-key of the default user

`iserver_interface`
- Main network interface of VM

`iserver_users_dirs`
- Home directories of the users whose bashrc will be customized

`iserver_bashrc`

Recommended Roles
-----------------

For more detailed SSH settings use the role
- ansible-ssh-server

Example Playbook
----------------

Name of playbook: `server-template.yml`

```yml
    - name: Initial configuration of a server.
      hosts: servers
      roles:
        - role: ansible-initial-server
```

The Debian installer creates a root user with a root password, who is not permitted to SSH in to the server. Sudo is not installed by default. Therefore the default user can SSH into Debian, but has to use `su` to do roots tasks.

Only for the first run on Debian (before the SSH key has been transferred):

- `ansible-playbook --ask-pass --ask-become-pass -e iserver_become=su  -vv server-template.yml`


For subsequent runs or on installations which have sudo and SSH-key already enabled:

- `ansible-playbook --vv server-template.yml`


For the first run on Ubuntu (before the SSH key has been transferred):

- `ansible-playbook -u ubuntu --ask-pass --ask-become-pass -vv server-template.yml`

For subsequent runs:

- `ansible-playbook -u ubuntu -vv server-template.yml`


License
-------

MIT

Author Information
------------------

Christian Wagner
