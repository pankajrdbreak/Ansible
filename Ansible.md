# Ansible Training - Day 3

## Ansible Playbooks


```console
---
- name: User creation on the Linux based server
  hosts: websrv
  tasks:
    - name: angleone user creations
      user: name=angelone state=present uid=6002 group=root

    - name: app folder creation tasks
      command: "mkdir -p /tmp/angeloneapp"

- name: Database user creation
  hosts: database
  tasks:
    - name: database user creation
      user: name=angeldata state=present uid=7002 group=root
 ```


Line Folding Teq 1
```console
---
- name: User creation on the Linux based server
  hosts: websrv
  tasks:
    - name: angleone user creations
      user: |
        name=angel3
        state=present
        uid=6009
        group=root
```

Line folding teq 2
```console
---
- name: User creation on the Linux based server
  hosts: websrv
  tasks:
    - name: angleone user creations
      user: >
        name=angel4
        state=present
        uid=6010
        group=root
```

Line folding teq 3
```console
---
- name: User creation on the Linux based server
  hosts: websrv
  tasks:
    - name: angleone user creations
      user:
        name: angel5
        state: present
        uid: 6011
        group: root
 ```
 
 
#### Commands
```console
 ansible-playbook test.yml --syntax-check             --> Work for indentation like error. It doesnt work for typo mistake
 ansible-playbook test.yml --check                    --> dry run 
 ansible-playbook test.yml --list-hosts               --> how many hosts available in the playbook
 ansible-playbook test.yml --list-tags--list-tasks    --> how many tasks going to run
 ansible-playbook test.yml --list-tags                --> gets tags on tasks
 ansible-playbook test.yml --step                     --> interactive action. It will ask yes/no while executing each task
```
