Network Configuration Backup to Git
=========

Network configuration backups are collected and then added to a git repository, this will require the ansible_user to have credentials (SSH key) setup to write to the backup target git repo.

Supported network equipment:

 - Cisco IOS
 - Cisco NXOS
 - Vyos

Example:

```
$ git diff 8a7a573d7f341e51e57a284336097041edfdf199
diff --git a/switch.error1.local.ios b/switch.error1.local.ios
index f9cd86e..c543ca8 100644
--- a/switch.error1.local.ios
+++ b/switch.error1.local.ios
@@ -1,9 +1,9 @@
 Building configuration...

-Current configuration : 6330 bytes
+Current configuration : 6366 bytes
 !
-! Last configuration change at 17:30:20 GMT Sat Dec 27 2025 by olly
-! NVRAM config last updated at 17:30:21 GMT Sat Dec 27 2025 by olly
+! Last configuration change at 23:24:44 GMT Sat Dec 27 2025 by olly
+! NVRAM config last updated at 23:24:45 GMT Sat Dec 27 2025 by olly
 !
 version 15.2
 no service pad
@@ -221,6 +221,7 @@ ip ssh pubkey-chain
    key-hash ssh-rsa 7FB6EC05EAAD562E290308DD538DEA02 ansible-worker@error1.local
 ip scp server enable
 !
+ip access-list extended anothertest
 ip access-list extended change
  permit ip any any
 ip access-list extended lkasdjfasdf
$
```

Requirements
------------

Git needs to be installed on the controller.

Role Variables
--------------

Defaults:

tmp_backup_dir: /tmp/network_backups/backups # tmp dir where we store backups.
git_repo_dir: /tmp/network_backups/repo # tmp dir where we will store the git repo.
git_branch: main
git_commit_message: "Ansible automated backup commit at {{ now(utc=true, fmt='%Y-%m-%d %H:%M:%S') }} UTC"

Required: examples used.

git_repo_url: 'git@gitserver.local:naemspace/network-config-archive.git'
git_user_name: ansible-worker
git_user_email: 'ansible-worker@domain.local'


A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

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

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

MIT
