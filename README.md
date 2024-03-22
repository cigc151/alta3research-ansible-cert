# Certification Challenge

## Project Structure

```python
certification_challenge/
├── alta3research-ansiblecert01.yml
├── ansible.cfg
├── inventory
│   └── hosts
└── roles
    └── host_details
        ├── README.md
        ├── tasks
        │   └── main.yml
        └── templates
            ├── basic.j2
            └── detailed.j2
```

## Pre steps to run the playbook

In order to run this playbook we need to verify the following:

1. Make sure you have the **ansible.cfg** file under **certification_challenge** folder.
2. Make sure the inventory file called **hosts** is under the inventory folder.
3. Make sure we have a list of hosts defined as planetexpress.

You can verify which ansible configuration file location using:

```python
$ ansible -- version
	ansible [core 2.16.4]
  **config file = /home/student/certification_challenge/ansible.cfg**
  configured module search path = ['/home/student/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /home/student/.local/lib/python3.10/site-packages/ansible
  ansible collection location = /home/student/.ansible/collections:/usr/share/ansible/collections
  executable location = /home/student/.local/bin/ansible
  python version = 3.10.12 (main, Nov 20 2023, 15:14:05) [GCC 11.4.0] (/usr/bin/python3)
  jinja version = 3.0.3
  libyaml = True
```

Make sure this config file is using the correct inventory

```python
~/certification_challenge$ cat ansible.cfg 
[defaults]
# default location of inventory
# this can be a file or a directory
inventory = /home/student/certification_challenge/inventory

# prevents playbook from hanging on new connections
host_key_checking = False
```

## How to run the playbook

```python
---
# tasks file for host_details/roles/task/main.yml

    - name: show fact keys
      debug:
        msg: "{{ ansible_facts.keys() }}"
      tags:
        - show_facts
    - name: Create basic information file from template
      template:
        src: basic.j2
        dest: "~/basic_info_{{ inventory_hostname }}.txt"
      tags:
        - basic_info

    - name: Create detailed information file from template
      template:
        src: detailed.j2
        dest: "~/detailed_info_{{ inventory_hostname }}.txt"
      tags:
        - detailed_info
```

As we can see in the above playbook we have three task defined using three different tags, that mean we can run the three tasks at the same time or run each task individually using the following commands.

```python
#to run all task at the same time 
~/certification_challenge$ ansible-playbook alta3research-ansiblecert01.yml

#to run only the show fact keys
~/certification_challenge$ ansible-playbook alta3research-ansiblecert01.yml --tags "show_facts"

#to run basic and detailed tasks
~/certification_challenge$ ansible-playbook alta3research-ansiblecert01.yml --skip-tags "show_facts"

```
