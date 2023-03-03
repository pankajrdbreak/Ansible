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


##### Debug Module
To print content in ansible 
```console
---
- name: Debug module demo
  hosts: all
  tasks:
    - name: testing debug module
      debug:
        msg: "This is PankajVare from Mumbai/n"
```
###### Output
```console
[root@ip-172-31-39-199 angel]# ansible-playbook 3rd-plyabook.yml 

PLAY [Debug module demo] *********************************************************************************************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************************************************************************************
ok: [client1]

TASK [testing debug module] ******************************************************************************************************************************************************************************************
ok: [client1] => {
    "msg": "This is PankajVare from Mumbai/n"
}

PLAY RECAP ***********************************************************************************************************************************************************************************************************
client1                    : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

##### Register keyword
Its used to store the output of task 

```console
---
- name: User creation on the Linux based server
  hosts: websrv
  tasks:
    - name: angleone user creations
      user:
        name: angel6
        state: present
        uid: 6012
        group: root
      register: output
- name: This is my output
  debug:
    msg: "{{ output }}"
 ```


##### Ansible_facts
It will store all information about remote hosts as a built in variable.
Setup module is running to gather the info

```console
---
- name: gather facts demo
  hosts: websrv
  tasks:
    - name: displya info
      debug:
        msg: "{{ ansible_hostname }}"
 ```

##### Custom Facts : To fetch application information
1. Create folder in remote host /etc/ansible/facts.d
2. Create web.fact inside /etc/ansible/facts.d
3. cat /etc/ansible/facts.d/web.fact
    [webdetails]
    web_pkg=nginx
    web_port=9000
4. save and close
5. On ansible server give below command 
6. ansible websrv -m setup -a 'filter=ansible_local'
7. Also we can create playbook
```console
---
- name: Get Web-app details from customize facts
  hosts: websrv
  tasks:
    - name: This is collection of customize facts
      debug:
        msg: "{{ ansible_local.web.webdetails.web_port }} {{ ansible_local.web.webdetails.web_pkg }}"
```
