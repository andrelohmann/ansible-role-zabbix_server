zabbix_server
=============

Use this role to install zabbix-server on an ubuntu server.

Requirements
------------

This role requires ubuntu xenials or bionic.

Role Variables
--------------

The following need to be set in group_vars/host_vars

    apache_remove_default_vhost: true
    php_version: 7.2
    php_ini_settings:
    - key: post_max_size
      value: 512M
    - key: upload_max_filesize
      value: 512M
    - key: max_execution_time
      value: 600
    - key: max_input_time
      value: 600
    php_additional_packages:
    - libapache2-mod-php7.2
    - php7.2-bcmath
    - php7.2-mbstring
    - php7.2-ldap
    - php7.2-mysql
    - php7.2-xml
    - php7.2-gd
    apache_additional_modules:
    - name: rewrite
      state: present
    - name: ssl
      state: present
    percona_mysql_root_password: __MYSQL_ROOT_PASSWORD__
    mysql_zabbix_user_password: __MYSQL_ZABBIX_USER_PASSWORD__
    mysql_grants_databases:
    - name: zabbix_db
      encoding: utf8
    mysql_grants_users:
    - name: zabbix
      password: __MYSQL_ZABBIX_USER_PASSWORD__
      host: localhost
      priv: "zabbix_db.*:ALL"
      state: present
    zabbix_server_admin_password:
    zabbix_server_url: __ZABBIX_SERVER_URL__
    zabbix_server_dbhost: localhost
    zabbix_server_dbname: zabbix_db
    zabbix_server_dbuser: zabbix
    zabbix_server_dbpassword: __MYSQL_ZABBIX_USER_PASSWORD__
    zabbix_server_dbport: 3306
    #zabbix_server_tlscafile: Full pathname of a file containing the top-level CA(s) certificates for peer certificate verification.
    #zabbix_server_tlscrlfile: Full pathname of a file containing revoked certificates.
    #zabbix_server_tlscertfile: Full pathname of a file containing the agent certificate or certificate chain.
    #zabbix_server_tlskeyfile: Full pathname of a file containing the agent private key.
    #zabbix_url_aliases: A list with Aliases for the Apache Virtual Host configuration.
    zabbix_timezone: Europe/Berlin
    zabbix_vhost: True
    zabbix_apache_vhost_port: 80
    zabbix_apache_tls: True
    zabbix_apache_redirect: True
    zabbix_apache_tls_crt: __PATH_TO_CRT_FILE__
    zabbix_apache_tls_key: __PATH_TO_KEY_FILE__
    zabbix_apache_tls_chain: PATH_TO_CHAIN_FILE__
    zabbix_web_max_execution_time: 600
    zabbix_web_memory_limit: 512
    zabbix_web_post_max_size: 128M
    zabbix_web_upload_max_filesize: 128M
    zabbix_web_max_input_time: 600
    #zabbix_web_env: (Optional) A Dictionary of PHP Environments

Example Playbook
----------------

    - hosts: zabbix-server
      gather_facts: yes
      become: yes
      roles:
      - andrelohmann.sury_php
      - andrelohmann.apache_php
      - andrelohmann.percona_mysql
      - andrelohmann.mysql_grants
      - andrelohmann.zabbix_server

License
-------

MIT

Author Information
------------------

https://github.com/andrelohmann
