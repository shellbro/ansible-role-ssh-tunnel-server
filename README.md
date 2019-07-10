shellbro.ssh_tunnel_server
==========================

[![Build Status](https://travis-ci.org/shellbro/ansible-role-ssh-tunnel-server.svg?branch=master)](https://travis-ci.org/shellbro/ansible-role-ssh-tunnel-server)

https://galaxy.ansible.com/shellbro/ssh_tunnel_server

Ansible role for setting up SSH tunnel between two CentOS 7 machines (server
side). Uses autossh and systemd for persistance.

After applying this role to a server machine use `shellbro.ssh_tunnel_client`
role on a client machine to estabish persistent tunnel.

Requirements
------------

Ansible version >= 2.4

Role Variables
--------------

* user - name of the user whose `authorized_keys` file will be modified
(required)
* client_alive_interval - SSH connection setting (by default 60 seconds)
* client_alive_count_max - SSH connection setting (by default 3)
* ssh_public_key_file - path to SSH public key (by default `id_rsa.pub`, file is
required)
* firewall_port - if specified, open this port in firewall

Dependencies
------------

None

Example Playbook
----------------

    - name: Configure server side on AWS
      hosts: server_side
      roles:
        - role: shellbro.ssh_tunnel_server
          user: ec2-user
          ssh_public_key_file: id_rsa.pub
          firewall_port: 12222

    - name: >-
        Set up persistent SSH tunnel for accessing SSH server
        (running on the client side) behind firewall
      hosts: client_side
      roles:
        - role: shellbro.ssh_tunnel_client
          server: server_side
          server_user: ec2-user
          user: devops
          remote_port_forwarding: true
          bind_address: 0.0.0.0
          forward_port: 12222
          target_host: 127.0.0.1
          target_port: 22

License
-------

BSD

Author Information
------------------

Jakub Gorczyca
