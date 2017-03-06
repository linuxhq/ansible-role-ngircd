# ansible-role-ngircd

[![Build Status](https://travis-ci.org/linuxhq/ansible-role-ngircd.svg?branch=master)](https://travis-ci.org/linuxhq/ansible-role-ngircd)

RHEL/CentOS - Next Generation IRC Daemon

## Requirements

None

## Role Variables

Available variables are listed below, along with default values:

    ngircd_adminemail: ngircd@localhost
    ngircd_admininfo1: ngircd
    ngircd_admininfo2: localhost
    ngircd_allowedchanneltypes: '#&+'
    ngircd_allowremoteoper: False
    ngircd_connectipv4: True
    ngircd_connectipv6: True
    ngircd_connectretry: 60
    ngircd_defaultusermodes: i
    ngircd_dns: True
    ngircd_helpfile: /usr/share/doc/ngircd/Commands.txt
    ngircd_ident: True
    ngircd_idletimeout: 0
    ngircd_info: 'Server Info Text'
    ngircd_listen: [ '127.0.0.1' ]
    ngircd_maxconnections: 0
    ngircd_maxconnectionsip: 5
    ngircd_maxjoins: 10
    ngircd_maxlistsize: 100
    ngircd_maxnicklength: 9
    ngircd_moreprivacy: False
    ngircd_name: "{{ inventory_hostname }}"
    ngircd_network: localnet
    ngircd_noticebeforeregistration: False
    ngircd_opercanusemode: False
    ngircd_opercanpautoop: True
    ngircd_operservermode: False
    ngircd_pam: True
    ngircd_pamisoptional: False
    ngircd_pingtimeout: 120
    ngircd_pongtimeout: 20
    ngircd_requireauthping: False
    ngircd_scrubctcp: False
    ngircd_servergid: ngircd
    ngircd_serveruid: ngircd
    ngircd_syslogfacility: local5

Additional configruation variables not defined by default:

    ngircd_chrootdir: /var/empty
    ngircd_cloakhost: cloaked.host
    ngircd_cloakhostmodex: cloaked.user
    ngircd_cloakhostsalt: abcdefghijklmnopqrstuvwxyz
    ngircd_cloakusertonick: True
    ngircd_incluedir: /etc/ngircd.d
    ngircd_motdfile: /etc/ngircd.motd
    ngircd_motdphrase: 'Hello world!'
    ngircd_password: xyz
    ngircd_pidfile: /var/run/ngircd/ngircd.pid
    ngircd_ports: [ '6667' ]
    ngircd_webircpassword: xyz

Additional ssl variables not defined by default:

    ngircd_certfile: /etc/ssl/server-cert.pem
    ngircd_cipherlist: [ 'SECURE128', '-VERS-SSL3.0' ]
    ngircd_dhfile: /etc/ssl/dhparams.pem
    ngircd_keyfile: /etc/ssl/server-key.pem
    ngircd_keyfilepassword: secret
    ngircd_ssl_ports: [ '6697', '9999' ]

Additional channel variable not defined by default:

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

Additional operator variable not defined by default:

    ngircd_operators:
      - name: nick1
        password: pass1 (plaintext)
        mask: *!*user1@host1.com
      - name: nick2
        password: pass2 (plaintext)
        mask: *!*user2@host2.com

Additional server variable not defined by default:

    ngircd_server:
      hub:
        name: hub.inventory.hostname.com
        mypassword: QKYt88uqiICVF3KPTMgL9PQm (plaintext)
      leafs:
        - name: leaf1.inventory.hostname.com
          mypassword: mFyu7YRJAhcStWBmZINDiu4c (plaintext)
        - name: leaf2.inventory.hostname.com
          mypassword: yw8OOBO6IVkFi2XlKCFJRamd (plaintext)

## Dependencies

 * https://galaxy.ansible.com/linuxhq/epel/

## Example Playbooks

#### Configure a standalone IRC server (binded to localhost)

    - hosts: servers
      roles:
        - role: linuxhq.ngircd
          ngircd_ports:
            - 6667

#### Configure a standalone IRC server with channels and operators (binded to ansible_default_ipv4)

    - hosts: servers
      roles:
        - role: linuxhq.ngircd
          ngircd_channels:
            - name: '#linuxhq'
              topic: 'http://www.linuxhq.org'
              modes: nt
          ngircd_listen:
            - "{{ ansible_default_ipv4.address }}"
          ngircd_operators:
            - name: tkimball
              password: QA@#h$$LysnSwGIppY
              mask: '*!*tkimball@*'
          ngircd_ports:
            - 6667

## License

BSD

## Author Information

This role was created by [Taylor Kimball](http://www.linuxhq.org).
