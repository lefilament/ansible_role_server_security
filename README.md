server_security
=========
The main repo for this role is on [Le Filament GitLab](https://sources.le-filament.com/lefilament/ansible-roles/server_security.git)
This roles installs and configures security features on server :
* auditd
* iptables
* fail2ban

Requirements
------------

This role should be executed after Docker and Traefik have been installed and configured (otherwise Fail2ban configuration may not be accepted).

Role Variables
--------------

This role is highly dependent on groups the host belongs to. In particular :
* docker : group for servers using Docker and Traefik
* docker_elk : group for servers hosting ELK stack for log collection from other hosts
* docker_nagios : group for servers hosting Nagios Docker for monitoring of other hosts
* full_maintenance : group for servers which send logs to ELK platform and perform daily backups
* odoo_server : group for deploying Odoo on server with nginx proxy (without docker)
* owncloud_server : group for deploying Owncloud on server with nginx proxy (without docker)

Other variables that are used in this role (with default values in defaults/main.yml) :
* default_maintenance_email : default maintenance e-mail where to send fail2ban action reports (defaults to maintenance@example.org)
* default_smtp_server : default smtp server to be used as relay for sending fail2ban action reports (defaults to smtp.example.org)
* default_sshd_port : default SSHD port to be used (default to 10022)
* docker_userns_remap : whether remapping of user namespace is being used for Docker (security feature defaults to true)
* dockremap_subuid : first subuid used for user namespace remap for Docker (defaults to 165536) - should be retrieved by docker_server role in host_vars
* dockremap_subgid : first subgid used for user namespace remap for Docker (defaults to 165536) - should be retrieved by docker_server role in host_vars
* logstash_port : port on which logstash server is listening for log collection (defaults to 5044)
* logstash_public_ip : logstash public ip address for log collection (defaults to 127.0.0.1)
* private_pull : whether a scheduled pulling of files via SFTP is to be performed on server (defaults to false)
* `server_security__manage_mail`: manage e-mails with `ssmtp` (default to ̀enabled`)


Dependencies
------------

This roles depends upon completion of docker_server role (for collecting correct dockremap subuid/gid) and filebeat (for provision of filebeat logs).

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:


    - hosts: all
      gather_facts: false
      become: true
      roles:
      - { role: server_security, tags: security }
      vars:
      - { default_maintenance_email: "maintenance@example.org"}
      - { default_smtp_server: "smtp.example.org" }
      - { default_sshd_port: 10022 }
      - { docker_userns_remap: true }
      - { dockremap_subuid: 165536 }
      - { dockremap_subgid: 165536 }
      - { logstash_port: 5044 }
      - { private_pull: false }


License
-------

AGPL-3

Author Information
------------------

Le Filament (https://le-filament.com)
