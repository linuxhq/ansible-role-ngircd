# ansible-role-ngircd

[![Build Status](https://travis-ci.org/linuxhq/ansible-role-ngircd.svg?branch=master)](https://travis-ci.org/linuxhq/ansible-role-ngircd)
[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-ngircd-blue.svg?style=flat)](https://galaxy.ansible.com/linuxhq/ngircd)
[![License](https://img.shields.io/badge/license-GPLv3-brightgreen.svg?style=flat)](COPYING)

RHEL/CentOS - Next Generation IRC Daemon

## Requirements

This role requires that you have the epel repository installed.

 * https://galaxy.ansible.com/linuxhq/epel/

## Role Variables

Available variables are listed below, along with default values:

    ngircd_channels: []
    ngircd_global_adminemail: ngircd@localhost
    ngircd_global_admininfo1: ngircd
    ngircd_global_admininfo2: localhost
    ngircd_global_helpfile: /usr/share/doc/ngircd/Commands.txt
    ngircd_global_info: localhost
    ngircd_global_listen: 0.0.0.0
    ngircd_global_name: localhost
    ngircd_global_network: localnet
    ngircd_global_pidfile: /var/run/ngircd/ngircd.pid
    ngircd_global_servergid: ngircd
    ngircd_global_serveruid: ngircd
    ngircd_limits_connectretry: 60
    ngircd_limits_idletimeout: 0
    ngircd_limits_maxconnections: 0
    ngircd_limits_maxconnectionsip: 5
    ngircd_limits_maxjoins: 10
    ngircd_limits_maxnicklength: 9
    ngircd_limits_maxlistsize: 100
    ngircd_limits_pingtimeout: 120
    ngircd_limits_pongtimeout: 20
    ngircd_operators: []
    ngircd_options_allowedchanneltypes: '#&+'
    ngircd_options_allowremoteoper: false
    ngircd_options_connectipv6: true
    ngircd_options_connectipv4: true
    ngircd_options_defaultusermodes: i
    ngircd_options_dns: true
    ngircd_options_ident: true
    ngircd_options_moreprivacy: false
    ngircd_options_noticebeforeregistration: false
    ngircd_options_opercanusemode: false
    ngircd_options_operchanpautoop: true
    ngircd_options_operservermode: false
    ngircd_options_pam: true
    ngircd_options_pamisoptional: false
    ngircd_options_requireauthping: false
    ngircd_options_scrubctcp: false
    ngircd_options_syslogfacility: local5
    ngircd_servers: {}
    ngircd_ssl: false
    ngircd_systemd: {}

Additional configruation variables not defined by default:

    ngircd_global_motdfile: | 
      bW90ZGZpbGUK
    ngircd_global_motdphrase: 'Hello world!'
    ngircd_global_password: xyz
    ngircd_global_ports:
      - 6667
      - 6668
    ngircd_options_chrootdir: /var/empty
    ngircd_options_cloakhost: cloaked.host
    ngircd_options_cloakhostmodex: cloaked.user
    ngircd_options_cloakhostsalt: abcdefghijklmnopqrstuvwxyz
    ngircd_options_cloakusertonick: true
    ngircd_options_incluedir: /etc/ngircd.d
    ngircd_options_webircpassword: xyz
    ngircd_ssl_certfile: /etc/ssl/server-cert.pem
    ngircd_ssl_cipherlist: 'SECURE128:-VERS-SSL3.0'
    ngircd_ssl_dhfile: /etc/ssl/dhparams.pem
    ngircd_ssl_keyfile: /etc/ssl/server-key.pem
    ngircd_ssl_keyfilepassword: secret
    ngircd_ssl_ports:
      - 6697
      - 9999

Example configuration for defining channels:

    ngircd_channels:
      - name: #chan1
        topic: subject1
        modes: nt
        key: secret1
        keyfile: /etc/key1.file
        maxusers: 25
      - name: #chan2
        topic: subject2
        modes: npst
        key: secret2
        keyfile: /etc/key2.file
        maxusers: 50

Example configuration for defining operators:

    ngircd_operators:
      - name: linuxhq
        password: sha5shooc8we8Seiseer
        mask: *@linuxhq.org
      - name: tkimball
        password: zajee7ahshoth6faCefu
        mask: *!tkimball@linuxhq.org

Example configuration for defining servers:

    ngircd_servers:
      hub:
        host: hub.linuxhq.org
        mypassword: QKYt88uqiICVF3KPTMgL9PQm
        name: hub.linuxhq.org
        passive: false
        port: 6667
        servicemask: '*Serv,Global'
        sslconnect: false
      leaf1:
        mypassword: mFyu7YRJAhcStWBmZINDiu4c
        name: leaf1.linuxhq.org
        passive: false
        port: 6667
        servicemask: '*Serv,Global'
        sslconnect: false

## Dependencies

None

## Example Playbooks

#### Configure a standalone IRC server (all ipv4 interfaces)

    - hosts: servers
      roles:
        - role: linuxhq.ngircd
          ngircd_ports:
            - 6667

#### Configure a standalone IRC server with channels and operators (all ipv4/ipv6 interfaces)

    - hosts: servers
      roles:
        - role: linuxhq.ngircd
          ngircd_channels:
            - name: '#linuxhq'
              topic: 'http://www.linuxhq.org'
              modes: nt
          ngircd_listen:
            - '::'
            - '0.0.0.0'
          ngircd_operators:
            - name: tkimball
              password: QA@#h$$LysnSwGIppY
              mask: '*!*tkimball@*.*'
          ngircd_ports:
            - 6667

#### Configure a IRC network with channels, operators, and servers (non-ssl only)

    - hosts: servers
      roles:
        - role: linuxhq.ngircd
          ngircd_channels:
            - name: '#linuxhq'
              topic: 'http://www.linuxhq.org'
              modes: nt
          ngircd_operators:
            - name: tkimball
              password: QA@#h$$LysnSwGIppY
              mask: '*!*tkimball@*'
          ngircd_ports:
            - 6667
          ngircd_servers:
            hub:
              host: hub.linuxhq.org
              mypassword: I6slKBEBkMOhz793KYz79A7j
              name: hub.linuxhq.org
              passive: false
              port: 6667
              sslconnect: false
            leaf1:
              name: leaf1.linuxhq.org
              mypassword: 4vPxc9VyQuMBJe98x3ZMNY9B
              passive: false
              port: 6667
              sslconnect: false

#### Configure a IRC network with channels, operators, and servers (ssl only)

    - hosts: servers
      roles:
        - role: linuxhq.ngircd
          ngircd_channels:
            - name: '#linuxhq'
              topic: 'http://www.linuxhq.org'
              modes: nt
          ngircd_operators:
            - name: tkimball
              password: QA@#h$$LysnSwGIppY
              mask: '*!*tkimball@*'
          ngircd_servers:
            hub:
              host: hub.linuxhq.org
              mypassword: I6slKBEBkMOhz793KYz79A7j
              name: hub.linuxhq.org
              passive: false
              port: 6697
              sslconnect: true
            leaf1:
              name: leaf1.linuxhq.org
              mypassword: 4vPxc9VyQuMBJe98x3ZMNY9B
              passive: false
              port: 6697
              sslconnect: true
          ngircd_ssl: true
          ngircd_ssl_certfile: "/etc/letsencrypt/live/{{ inventory_hostname }}/fullchain.pem"
          ngircd_ssl_cipherlist: 'SECURE128:-VERS-SSL3.0'
          ngircd_ssl_keyfile: "/etc/letsencrypt/live/{{ inventory_hostname }}/privkey.pem"
          ngircd_ssl_ports: 6697

## License

Copyright (C) 2018 Taylor Kimball <tkimball@linuxhq.org>

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program. If not, see <http://www.gnu.org/licenses/>.
