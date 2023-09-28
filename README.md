certbot
=======

An Ansible role to issue and revoke Let's Encrypt SSL certificates using OVH
and Cloudflare DNS challenges.

Requirements
------------

This role has no specific requirements.

Role Variables
--------------

```
# SSL certificates
certbot_certs:
  - domains:
      - one.domain.com
      - two.domain.com
      - three.domain.com
    challenge: dns-ovh # see supported challenges
    expand: false # true | false (default 'false')
    post_hook: systemctl reload nginx # post-hook command
    state: present # present | revoked

# E-mail address for Let's Encrypt account
certbot_email_address: me@domain.com

# OVH DNS challenge settings (see https://certbot-dns-ovh.readthedocs.io/)
certbot_dns_ovh_config_file_path: /etc/certbot-dns-ovh.ini
certbot_dns_ovh_endpoint: ovh-eu
certbot_dns_ovh_application_key: MDAwMDAwMDAwMDAw
certbot_dns_ovh_application_secret: MDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAw
certbot_dns_ovh_consumer_key: MDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAw

# DNS propagation in seconds
certbot_dns_propagation_seconds: 30

# Certbot issue command
certbot_commmand: certbot

certbot_arguments:
  - --non-interactive
  - --agree-tos
  - --no-eff-email
  - --no-redirect

certbot_challenges_arguments:
  dns-ovh:
    - --dns-ovh
    - --dns-ovh-credentials {{ certbot_dns_ovh_config_file_path }}
    - --dns-ovh-propagation-seconds {{ certbot_dns_propagation_seconds }}
  dns-cloudflare: # FIXME: to be completed
    - --dns-cloudflare
```

### Supported challenges

This role supports the following DNS challenges:

* OVH: https://certbot-dns-ovh.readthedocs.io/ (`challenge=dns-ovh`)
* Cloudflare: https://certbot-dns-cloudflare.readthedocs.io
(`challenge=dns-cloudflare`)

You can specify a different challenge for each domains, with the `challenge`
key into the `certbot_certs` element.

Dependencies
------------

This role has no dependecies.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
        - role: mmartinello.certbot
          vars:
            certbot_certs:
              - domains:
                  - foo.domain.com
                  - bar.domain.com
                challenge: dns-ovh
                post_hook: systemctl reload nginx
                state: present
            certbot_email_address: mail@domain.com
            certbot_dns_ovh_application_key: <my_application_key>
            certbot_dns_ovh_application_secret: <my_application_secret>
            certbot_dns_ovh_consumer_key: <my_consumer_key>

License
-------

MIT

Author Information
------------------

Mattia Martinello 🇮🇹 🗺️

Feedbacks, bug reports, requests & more
---------------------------------------

Are very welcomed [here](https://github.com/mmartinello/ansible-certbot/issues)!