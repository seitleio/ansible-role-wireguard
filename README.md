Ansible Role: wireguard
=========
[![Ansible Lint](https://github.com/seitleio/ansible-role-wireguard/actions/workflows/ansible-lint.yaml/badge.svg)](https://github.com/seitleio/ansible-role-wireguard/actions/workflows/ansible-lint.yaml)

This role creates a [Wireguard](https://hub.docker.com/r/linuxserver/wireguard). The container can be configured as server or client using the ansible variables.

Requirements
------------

As a prerequisite Python 3, Python Pip and Python docker module are required on the target host. You can install the packages manually or via ansible (see [Example Playbook](#example-playbook)).

```bash
# Manually install the packages with apt
apt install python3-full python3-pip python3-docker
```

Role Variables
--------------

| name                               | purpose                                               | default value                                                            | note |
| ---------------------------------- | ----------------------------------------------------- | ------------------------------------------------------------------------ | ---- |
| service_name                       | Used for the docker container name and the data path. | "wireguard"                                                              |      |
| service_data_location              | All data created by this service will be stored here. | "/data/services/{{ service_name }}"                                      |      |
| wireguard_network_prefix           | Network prefix for the Wireguard docker network. | "172.0.20"                                                               |      |
| wireguard_network_gateway_ip       | The host IP of the gateway (the docker host).                                     | "{{ wireguard_network_prefix }}.1"                                       |      |
| wireguard_network_subnet           | Wireguard network subnet IP adresses.  | "{{ wireguard_network_prefix }}.0/24"                                    |      |
| wireguard_container_ip             | Host IP of the Wireguard container. | "{{ wireguard_network_prefix }}.2"                                       |      |
| wireguard_server_mode              | Decide if the container should accept client connections. | false                                                                    |      |
| wireguard_server_url               | The external address of the wireguard server. | ""                                                                       |      |
| wireguard_server_peers             | When the container is used as server, this can  | "" # eg. iPhone, notebook - Will automatically create peer configuration |      |
| wireguard_server_internal_subnet   | Subnet used by the wireguard container.| "172.9.9.0/24"                                                           |      |
| wireguard_server_allowed_ips       | Client allowed IPs. | "0.0.0.0/8"                                                              |      |
| wireguard_server_peerdns           | DNS server used by connected clients. | "1.1.1.1"                                                                |      |
| wireguard_client_route_subnets     | Adds routes to the host using a systemd unit | []                        |      |
| wireguard_client_configs_directory | Path to the wireguard configuration files. | "{{ service_data_path }}/config/wg_confs"                                |      |
| wireguard_client_nat_host          |  | false                                                                    |      |
| wireguard_client_configs           | Control were the container should connect to. See [Example: wireguard_client_configs](#example-wireguard_client_configs) | []                                                                       |      |

### Example: wireguard_client_configs
```yaml
- name: wg-home
  ip_address: 172.9.9.2
  dns: 1.1.1.1
  port: 51820
  client_private_key: ...
  server_public_key: ...
  endpoint: vpn.example.org
  allowed_ips: 192.168.178.0/24
  host_port_forwarding: [80,443,22]
  persistent_keepalive: 25
```

Dependencies
------------

* https://github.com/geerlingguy/ansible-role-docker

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
- name: Run wireguard on wireguard_hosts
  hosts: wireguard_hosts
  gather_facts: true
  tags:
    - setup_wireguard
  pre_tasks:
  - name: Install requirements for Docker Ansible module
    ansible.builtin.apt:
      pkg:
      - python3-full
      - python3-pip
      - python3-docker
    when: ansible_distribution == 'Debian' or ansible_distribution == 'Ubuntu'
  roles:
    - role: ansible-role-wireguard
```

License
-------
MIT

Author Information
------------------
Johannes Seitle <<johannes@seitle.io>>

<br/><br/><hr/>
<p align="center" style="font-size:24px">
<img src="https://avatars.githubusercontent.com/u/102231325?s=400&u=0c500c28b968020e0c306478e55779ed7a872a98&v=4" width="128"/><br/>
seitle.io
<p/>
