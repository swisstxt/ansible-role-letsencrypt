Let's Encrypt Ansible Role
==========================

Sets up Let's Encrypt certificates for a web server or web proxy.

Certificate creation is done using certbot, and the internal web server of certbot is used for verification.
This package is in the EPEL repository on CentOS/RHEL (not enabled by default).

You must disable any server listening on port 80 before running this role.
Alternatively, you can specify a list of services to stop before and restart after creating the certificates.

It is also possible to specify a different port. See below.

A cron job is created to re-verify the certificate. It runs at a random minute every 12 hours by default.

See https://certbot.eff.org/ for more information.


Output
------

The certificate files for each domain entry will sit at <letsencrpyt_config_dir>/live/<main_domain>/ and are named:
* cert.pem: the host certificate
* privkey.pem: the private key
* chain.pem: the certificate chain only
* fullchain.pem: the full certificate chain including the host certificate
* privkey-fullchain.pem: the private key concatenated with the full chain


Role Variables
--------------

* letsencrypt_config_dir: The destination folder for config, certificates and keys. Default: /etc/letsencrypt
* letsencrypt_work_dir: The certificate generator temp directory. Default: /var/lib/letsencrypt
* letsencrypt_log_dir: Where to put log files. Default: /var/log/letsencrypt
* letsencrypt_listen: Specify the builtin web server port. Required for domain verification. Can also be overriden per certificate. Default: 443
* letsencrypt_cron: Set to false to disable creation of a cron job. Default: true
* letsencrypt_email: Specify the admin contact email address for this domain. Default: none; must be specified
* letsencrypt_services: An optional list of services that should be stopped and restarted while certificates are created or renewed. Default: none
* letsencrpyt_certificates: List of certificates to generate. Each definition must contain a name (usually the main domain) and all domains to sign for.
  Note that the target host must be reachable under all these domains.
  It is also possible to specify a separate listen port per certificate. Default: [ { main: ansible_fqdn, listen: <undefined>, domains: [ ] } ]


Example Playbook
----------------

    - hosts: ssl
      roles:
       - role: letsencrypt
         letsencrypt_cron: no
         letsencrypt_listen: 8080
         letsencrypt_domains:
           - main: www.example.com
             domains: example.com
           - main: www.example2.com
             listen: 8081


Copyright
---------

Copyright © 2017 Swiss TXT AG.
All rights reserved.

Licensed under the [ISC license](https://opensource.org/licenses/ISC).
