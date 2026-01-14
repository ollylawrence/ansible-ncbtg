Network Configuration Backup to Git
=========

Network configuration backups are collected and then added to a git repository, this will require the ansible_user to have credentials (SSH key) setup to write to the backup target git repo.

Supported network equipment:

 - Cisco IOS
 - Cisco NXOS
 - Vyos

Example:

```
$ git log --oneline
bb6a7fa (HEAD -> main, origin/main, origin/HEAD) Ansible automated backup commit at 2026-01-14 03:30:48 UTC
35327b9 Ansible automated backup commit at 2026-01-14 02:39:55 UTC
818b2c3 Ansible automated backup commit at 2026-01-12 02:14:58 UTC

$ git diff 35327b9
+++ b/switch.ios
@@ -1,9 +1,9 @@
 Building configuration...
 
-Current configuration : 6398 bytes
+Current configuration : 6441 bytes
 !
-! Last configuration change at 02:39:05 GMT Wed Jan 14 2026 by olly
-! NVRAM config last updated at 02:39:18 GMT Wed Jan 14 2026 by olly
+! Last configuration change at 03:30:18 GMT Wed Jan 14 2026 by olly
+! NVRAM config last updated at 03:30:18 GMT Wed Jan 14 2026 by olly
 !
 version 15.2
 no service pad
@@ -231,6 +231,11 @@ ip access-list extended yet-another-test
  remark testing 123
 !
 !
+banner motd ^C
+
+Need to make a change.
+
+^C
 !
 line con 0
 line vty 0 4
```

Requirements
------------

Git needs to be installed on the controller.

The ansible_user also has to have write access to the git repository authentication is assumed to be via ssh key.

Role Variables
--------------

Defaults:

tmp_backup_dir: /tmp/network_backups/backups # tmp dir where we store backups.
git_repo_dir: /tmp/network_backups/repo # tmp dir where we will store the git repo.
git_branch: main
git_commit_message: "Ansible automated backup commit at {{ now(utc=true, fmt='%Y-%m-%d %H:%M:%S') }} UTC"

Vars:

git_user_name: "{{ ansible_user }}"

Required: examples used.

git_repo_url: 'git@gitserver.local:naemspace/network-config-archive.git'
git_user_email: 'ansible-worker@domain.local'


Dependencies
------------

In order to backup the network equipments configuration the following collections are required:

collections:
  - name: ansible.netcommon
  - name: cisco.ios
  - name: cisco.nxos
  - name: vyos.vyos


Example Playbook
----------------

Just call the role and at least the following variables.

```
- name: Network Backup Playbook.
  hosts: ios
  gather_facts: false
  roles:
    - role: ansible-ncbtg
      vars:
        git_repo_url: 'git@gitserver:project/network-config-archive.git'
        git_user_email: 'user@domain'
```

License
-------

MIT
