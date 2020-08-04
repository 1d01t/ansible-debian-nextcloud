# ansible-debian-nextcloud


this ansible script installs nextcloud on debian buster, with:
  - PortGreSQL
  - Reddis
  - Nginx from external repo: http://nginx.org/packages/mainline/debian
  - php 7.4 from external repo: https://packages.sury.org/php/
  - letsencrypt with nginx plugin

# Nextcloud APPS this project is using

disable:
  - survey_client
  - firstrunwizard
  - activity
  
enable:
  - admin_audit
  - files_pdfviewer
  
add:
  - rainloop
  - keeweb
  - talk
  - contacts
  - calendar
  - music
  - notes
  - tasks
  - groupfolders
  - files_ebookreader
  - group_everyone
  - previewgenerator
  - Collabora Online - Built-in CODE Server
  - Collabora Online

# Prepare plain debian buster instance
  - install tasksel
  - add your ssh key to the instance
  - allow ssh root login
  - start ssh service

# Modify the project to fit your needs

Nginx is configured to work behind an nginx SNI proxy server.
If you don't want that edit:
  templates/nginx_nextcloud.conf
and remove:
  - in line 17 and 18: "proxy_protocol"
  - the whole line 26, 27 and 28
  
  Furthermore nextcloud is configured to store all data to en external NFS mount.
  If you don't want that edit:
    nextcloud.yaml
and remove:
  - the whole lines: 433 to 439

Generally all nextcloud data are stored in "/var/nc_data_mount"

Before you can use this project, you habe to set your configs in:
  - hosts
  - group_vars/homeserver/vars.yml
  - group_vars/homeserver/vault.yml

secure yourself and encrypt the vault.yml:
  - ansible-vault encrypt group_vars/homeserver/vault.yml

# Build your own Nextcloud server

finaly run:
  - ansible-playbook -i hosts nextcloud.yaml --ask-vault-pass

