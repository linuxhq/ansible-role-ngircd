# ansible-role-ngircd

[![Build Status](https://travis-ci.org/linuxhq/ansible-role-ngircd.svg?branch=master)](https://travis-ci.org/linuxhq/ansible-role-ngircd)

RHEL/CentOS - Next Generation IRC Daemon

## Requirements

None

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
    ngircd_options_allowremoteoper: False
    ngircd_options_connectipv6: True
    ngircd_options_connectipv4: True
    ngircd_options_defaultusermodes: i
    ngircd_options_dns: True
    ngircd_options_ident: True
    ngircd_options_moreprivacy: False
    ngircd_options_noticebeforeregistration: False
    ngircd_options_opercanusemode: False
    ngircd_options_operchanpautoop: True
    ngircd_options_operservermode: False
    ngircd_options_pam: True
    ngircd_options_pamisoptional: False
    ngircd_options_requireauthping: False
    ngircd_options_scrubctcp: False
    ngircd_options_syslogfacility: local5
    ngircd_servers: []
    ngircd_ssl: False

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
    ngircd_options_cloakusertonick: True
    ngircd_options_incluedir: /etc/ngircd.d
    ngircd_options_webircpassword: xyz
    ngircd_ssl_certfile: /etc/ssl/server-cert.pem
    ngircd_ssl_cipherlist: [ 'SECURE128', '-VERS-SSL3.0' ]
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
      - name: nick1
        password: pass1 (plaintext)
        mask: *!*user1@host1.com
      - name: nick2
        password: pass2 (plaintext)
        mask: *!*user2@host2.com

Additional server variable not defined by default:

    ngircd_servers
      - name: server1.linuxhq.org
        passive: False
        peerpassword: mFyu7YRJAhcStWBmZINDiu4c
        port: 6667
        mypassword: QKYt88uqiICVF3KPTMgL9PQm
        sslconnect: False
        servicemask: '*Serv,Global'
      - name: server2.linuxhq.org
        passive: False
        peerpassword: QKYt88uqiICVF3KPTMgL9PQm
        port: 6667
        mypassword: mFyu7YRJAhcStWBmZINDiu4c
        sslconnect: False
        servicemask: '*Serv,Global'

## Dependencies

 * https://galaxy.ansible.com/linuxhq/epel/

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
            - name: server1.linuxhq.org
              mypassword: I6slKBEBkMOhz793KYz79A7j
              passive: False
              peerpassword: 4vPxc9VyQuMBJe98x3ZMNY9B
              port: 6667
              sslconnect: False
            - name: server2.linuxhq.org
              mypassword: 4vPxc9VyQuMBJe98x3ZMNY9B
              passive: False
              peerpassword: I6slKBEBkMOhz793KYz79A7j
              port: 6667
              sslconnect: False

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
            - name: server1.linuxhq.org
              mypassword: I6slKBEBkMOhz793KYz79A7j
              passive: False
              peerpassword: 4vPxc9VyQuMBJe98x3ZMNY9B
              port: 6697
              sslconnect: True
            - name: server2.linuxhq.org
              mypassword: 4vPxc9VyQuMBJe98x3ZMNY9B
              passive: False
              peerpassword: I6slKBEBkMOhz793KYz79A7j
              port: 6697
              sslconnect: True
          ngircd_ssl: True
          ngircd_ssl_certfile: "/etc/letsencrypt/live/{{ inventory_hostname }}/fullchain.pem"
          ngircd_ssl_cipherlist: 'SECURE128:-VERS-SSL3.0'
          ngircd_ssl_keyfile: "/etc/letsencrypt/live/{{ inventory_hostname }}/privkey.pem"
          ngircd_ssl_ports: 6697

## License

BSD

## Author Information

This role was created by [Taylor Kimball](http://www.linuxhq.org).
