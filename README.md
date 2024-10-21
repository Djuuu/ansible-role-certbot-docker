Ansible Role: Certbot-docker
============================

Install Let's Encrypt Certbot Docker Compose project.

Uses Certbot container with Gandi LiveDNS authentication plugin.

- https://letsencrypt.org/
- https://eff-certbot.readthedocs.io/en/stable/
- https://github.com/obynio/certbot-plugin-gandi

Requirements
------------

Requires the following to be installed:
- docker
- docker compose

Role Variables
--------------

Common Docker projects variables:

```yaml
# Base directory for Docker projects
docker_projects_path: # /var/apps
```

Available role variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
# Docker project variables

certbot_project_name: letsencrypt


# Let's Encrypt project variables

# Certbot default authenticator (webroot | dns-gandi)
certbot_authenticator: webroot

# Email used for registration and recovery contact
certbot_email: you@example.net

# Certificates
letsencrypt_certs: []
#  - name: 'default.net'
#    domains:
#      - 'default.net'
#
#  - name: 'example.net'
#    domains:
#      - 'example.net'
#      - 'www.example.net'
#    authenticator: dns-gandi
#    gandi_key_name: key1

# Gandi access tokens
certbot_gandi_access_tokens: []
#  # single / default key
#  - xxxxxxxx-xxxxxxxx
#
#  # multiple keys
#  - { name: key1, key: xxxxxxxx-xxxxxxxy }
#  - { name: key2, key: xxxxxxxx-xxxxxxxz }

# Number of seconds to wait for DNS propagation
dns_gandi_propagation_seconds: 30
```

Run options variables:

```yaml
dry_run: false
```

Dependencies
------------

This role depends on :
- [djuuu.docker_project](https://github.com/Djuuu/ansible-role-docker-project)

Example Playbook
----------------

Install Let's Encrypt

```yaml
- hosts: all
  gather_facts: true
  gather_subset:
    - "!all"
    - "!min"
    - user_id

  roles:
    - djuuu.certbot_docker
```

Generate certificate

```yaml
- hosts: server
  gather_facts: true
  gather_subset:
    - "!all"
    - "!min"
    - user_id

  tasks:
    - name: Generate certificates
      ansible.builtin.include_role:
        name: djuuu.certbot_docker
        tasks_from: certonly
      loop: "{{ letsencrypt_certs }}"
      loop_control:
        loop_var: cert
      tags: [always]
```

Certificate names can be used as tags:
```bash
ansible-playbook generate-certificates.yml -e dry_run=true -t mydomain.net
```

License
-------

Beerware License
