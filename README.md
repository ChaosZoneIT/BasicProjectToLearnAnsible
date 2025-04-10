# This is a basic project to LEARN how to automate system configuration and management with Ansible (by default it is installed on one machine - main / primary / host container).

## Basic images with only ssh, net-tools, nano are in the BasicImage directory.

They are from Ubuntu 22, Ubuntu 24, Debian Bookworm, fedora 41, Alma Linux 9.5, Alma Linux 9.5 (min) and Rocky Linux 9, which are very simple and different, but can be learned (maybe in the future the evolution to better).

Can build all images use docker-compose in BasicImage directory:

`
cd BasicImage
`

`
docker compose build
`

this command create all images with default name:

`
vps-[SYSTEM_NAME]-[VERSION]-image
`

Such names are used to build the basic structure for learning

## Directory: vps-host
Dockerfile to create image with primary system to managment other and had installed Ansible and python

## Directory: vps
Dockerfile to create images to symulation network with different systems. They have  only python and user ansible (password: ansible) to connect with ssh

## Primary project
Primary docker compose create full project (systems and network with static IP)

Primary system to managment has address: <b>172.16.128.2</b> and volumens:

- directory: <b>host</b> with groups and address all machines
- directory: <b>playbooks</b> with basic ansible playbooks (in future)

Other systems have address:
- Ubuntu 22 - 172.16.128.4
- Ubuntu 24 - 172.16.128.5
- Debian Bookworm - 172.16.128.6
- Fedora 41- 172.16.128.7
- AlmaLinux 9.5 - 172.16.128.8
- AlmaLinux 9.5 min - 172.16.128.9
- RockyLinux 9 - 172.16.128.10

### Primary project run
In the primary directory build or run all project:

`
docker compose build
`

`
docker compose up -d
`

### Cleaning after use project (in computer):
Stop docker containers:

`
docker compose down
`

Remove images:

Lates when use to learn:

`
docker rmi $(docker images | grep server-vps-)
`

`
docker rmi server-ansible-host:latest
`

Base images:

`
docker rmi $(docker images | grep -oE 'vps-.*-image')
`

Remove address from the list of known hosts:

`
ssh-keygen -f '/home/username/.ssh/known_hosts' -R '172.16.128.2'
`

`
ssh-keygen -f '/home/username/.ssh/known_hosts' -R '172.16.128.4'
`

`
ssh-keygen -f '/home/username/.ssh/known_hosts' -R '172.16.128.5'
`

`
ssh-keygen -f '/home/username/.ssh/known_hosts' -R '172.16.128.6'
`

`
ssh-keygen -f '/home/username/.ssh/known_hosts' -R '172.16.128.7'
`

`
ssh-keygen -f '/home/username/.ssh/known_hosts' -R '172.16.128.8'
`

`
ssh-keygen -f '/home/username/.ssh/known_hosts' -R '172.16.128.9'
`

`
ssh-keygen -f '/home/username/.ssh/known_hosts' -R '172.16.128.10'
`

## Use project
### It is Base project to learn.
Login to primary system:

`
ssh ansible@172.16.128.2
`
(password ansible)

Generate ssh key:

`
ssh-keygen 
`
(accept all default settings)

Copy this key to all machines:
|  System |  command |  description |
|---|---|---|
| Ubuntu 22 | ssh-copy-id ansible@172.16.128.4 | confirm and give password to user in machine ubuntu 22 (default passwort to user ansible is ansible) |
| Ubuntu 24 | ssh-copy-id ansible@172.16.128.5 | confirm and give password to user in machine ubuntu 24 (default passwort to user ansible is ansible) |
| Debian Bookworm | ssh-copy-id ansible@172.16.128.6 | confirm and give password to user in machine Debian Bookworm (default passwort to user ansible is ansible) |
| Fedora 41 | ssh-copy-id ansible@172.16.128.7 | confirm and give password to user in machine Fedora 41 (default passwort to user ansible is ansible) |
| AlmaLinux 9.5 | ssh-copy-id ansible@172.16.128.8 | confirm and give password to user in machine AlmaLinux 9.5 (default passwort to user ansible is ansible) |
| AlmaLinux 9.5 min | ssh-copy-id ansible@172.16.128.9 | confirm and give password to user in machine AlmaLinux 9.5 min (default passwort to user ansible is ansible) |
| RockyLinux 9 | ssh-copy-id ansible@172.16.128.10 | confirm and give password to user in machine RockyLinux 9 (default passwort to user ansible is ansible) |


### Login to others system (e.q. Ubintu 22):
`
ssh ansible@172.16.128.4 
`
(password ansible)

Generate ssh key:

`
ssh-keygen
`
(accept all default settings)

Copy this key to host machines:

`
ssh-copy-id ansible@172.16.128.2
`
confirm and give password to user in machine host (default passwort to user ansible is ansible)

Repeat to all containers

## Learning Ansible

Login to primary machine and can to learning

`
ssh ansible@172.16.128.2
`
(password: ansible)

Test connected primary machine with all hosts

`
ansible all -m ping
`

Result should be green:

```
172.16.128.6 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.11"
    },
    "changed": false,
    "ping": "pong"
}
172.16.128.4 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.10"
    },
    "changed": false,
    "ping": "pong"
}
172.16.128.5 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.12"
    },
    "changed": false,
    "ping": "pong"
}
172.16.128.8 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.9"
    },
    "changed": false,
    "ping": "pong"
}
172.16.128.7 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
172.16.128.9 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.9"
    },
    "changed": false,
    "ping": "pong"
}
172.16.128.10 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.9"
    },
    "changed": false,
    "ping": "pong"
}
````

There is still a warning

```
[WARNING]: Platform linux on host 172.16.128.6 is using the discovered Python interpreter at /usr/bin/python3.11, but
future installation of another Python interpreter could change the meaning of that path. See
https://docs.ansible.com/ansible-core/2.17/reference_appendices/interpreter_discovery.html for more information.
```
