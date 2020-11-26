Ansible Role Windows OpenSSH Server
=========

This role enables the Windows native capability OpenSSH Server.  
Optionally enables powershell as default shell.  
Optionally copies id_rsa.pub to administrators_authorized_keys.

Requirements
------------

Windows 10 or Windows Server 2019

Role Variables
--------------
**win_open_ssh_server_shell_powershell**   
Default: `false`  
Set the default shell. If not set, default shell is CMD.   
If set to **true**, default shell is powershell.  

**win_open_ssh_server_set_administrators_authorized_keys**   
Default: `false`    
if set to **true**, id_rsa.pub is copied to administrators_authorized_key file  

**win_open_ssh_server_administrators_authorized_keys**   
Default: `"{{ lookup('env','HOME') + '/.ssh/id_rsa.pub' }}"`  
path to your authorized_keys file


Example Playbook
----------------

```yaml
---
- hosts: windows
  become: yes
  vars:
    win_open_ssh_server_shell_powershell: true
    win_open_ssh_server_set_administrators_authorized_keys: true

  roles:
    - { role: ansible-role-win-openssh-server }
```

Finally using Ansible with SSH-enabled Windows
---------------------------------------
**Example Inventory**

```
[windows_ssh]
winsshbox ansible_host=192.168.222.126 ansible_user=somelocaladmin
```

**Example Playbook**

```yaml
---
- hosts: windows_ssh
  vars:
    ansible_connection: ssh
    ansible_port: 22    
    ansible_shell_type: powershell
    ansible_ssh_common_args: '-o StrictHostKeyChecking=no'
    ansible_become_method: runas
    ansible_become_user: "{{ ansible_user }}"

  tasks:
    - name: test powershell
      win_shell: |
        get-host
```
