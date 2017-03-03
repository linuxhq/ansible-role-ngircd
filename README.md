# ansible-role-ngircd

[![Build Status](https://travis-ci.org/linuxhq/ansible-role-ngircd.svg?branch=master)](https://travis-ci.org/linuxhq/ansible-role-ngircd)

RHEL/CentOS - Next Generation IRC Daemon

## Requirements

None

## Role Variables

Available variables are listed below, along with default values:

    ngircd_adminemail: root@localhost
    ngircd_admininfo1: root
    ngircd_admininfo2: localhost
    ngircd_helpfile: /usr/share/doc/ngircd/Commands.txt
    ngircd_info:
      - Server
      - Info
      - Text
    ngircd_listen:
      - 127.0.0.1
    ngircd_motdfile: /etc/ngircd.motd
    ngircd_motdphrase: 'Hello world!'
    ngircd_name: "{{ inventory_hostname }}"
    ngircd_network: localnet
    ngircd_servergid: ngircd
    ngircd_serveruid: ngircd

Additional generic configruation variables not defined by default:

    ngircd_allowedchanneltypes: '#&+'
    ngircd_allowremoteoper: False
    ngircd_chrootdir: /var/empty
    ngircd_cloakhost: cloaked.host
    ngircd_cloakhostmodex: cloaked.user
    ngircd_cloakhostsalt: abcdefghijklmnopqrstuvwxyz
    ngircd_cloakusertonick: True
    ngircd_connectipv4: True
    ngircd_connectipv6: True
    ngircd_connectretry: 60
    ngircd_defaultusermodes: i
    ngircd_dns: True
    ngircd_ident: True
    ngircd_idletimeout: 0
    ngircd_incluedir: /etc/ngircd.d
    ngircd_maxconnections: 0
    ngircd_maxconnectionsip: 5
    ngircd_maxjoins: 10
    ngircd_maxlistsize: 100
    ngircd_maxnicklength: 9
    ngircd_moreprivacy: False
    ngircd_noticebeforeregistration: False
    ngircd_opercanusemode: False
    ngircd_opercanusemode: True
    ngircd_operservermode: False
    ngircd_pam: True
    ngircd_pamisoptional: False
    ngircd_password: xyz
    ngircd_pidfile: /var/run/ngircd/ngircd.pid
    ngircd_pingtimeout: 120
    ngircd_pongtimeout: 20
    ngircd_ports: [ '6667' ] 
    ngircd_requireauthping: False
    ngircd_scrubctcp: False
    ngircd_syslogfacility: local5
    ngircd_webircpassword: xyz

Additional ssl variables not defined by default:

    ngircd_certfile: /etc/ssl/server-cert.pem
    ngircd_cipherlist: [ 'SECURE128', '-VERS-SSL3.0' ]
    ngircd_dhfile: /etc/ssl/dhparams.pem
    ngircd_keyfile: /etc/ssl/server-key.pem
    ngircd_keyfilepassword: secret
    ngircd_ssl_ports: [ '6697', '9999' ]

Additional channel, operator, and server variables not defined by default:

    ngircd_channels:
      - name: channel
        topic: subject
        modes: nt
        key: secret
        keyfile: /etc/key.file
        maxusers: 25
    ngircd_operators:
      - name: nick
        password: pass
        mask: *!*user@host.com

## Dependencies

 * https://galaxy.ansible.com/linuxhq/epel/

## Example Playbook

    - hosts: servers
      roles:
        - role: linuxhq.ngircd
          ngircd_operators:
            - name: tkimball
              password: operup
              mask: *!taylor@linuxhq.org

## License

BSD

## Author Information

This role was created by [Taylor Kimball](http://www.linuxhq.org).
