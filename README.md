[![CI](https://github.com/de-it-krachten/ansible-role-postgresql/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-postgresql/actions?query=workflow%3ACI)


# ansible-role-postgresql

PostgreSQL installation (v12+)
https://www.postgresql.org



## Dependencies

#### Roles
- deitkrachten.python

#### Collections
- community.postgresql

## Platforms

Supported platforms

- Red Hat Enterprise Linux 8<sup>1</sup>
- Red Hat Enterprise Linux 9<sup>1</sup>
- Red Hat Enterprise Linux 10<sup>1</sup>
- RockyLinux 8
- RockyLinux 9
- RockyLinux 10
- OracleLinux 8
- OracleLinux 9
- OracleLinux 10
- AlmaLinux 8
- AlmaLinux 9
- AlmaLinux 10
- Debian 11 (Bullseye)
- Debian 12 (Bookworm)
- Debian 13 (Trixie)
- Ubuntu 22.04 LTS
- Ubuntu 24.04 LTS

Note:
<sup>1</sup> : no automated testing is performed on these platforms

## Role Variables
### defaults/main.yml
<pre><code>
# Type of installation (client or server)
postgresql_install_type: server

# Installation source to use (vendor or oss)
postgresql_type: oss

# Postgresql version
postgresql_version: 13

# Defines proxy to use
# postgresql_proxy: http://127.0.0.1:3128

# List of server packages
postgresql_optional_packages: []

# Location for pypi virtualenv
postgresql_venv_root: /usr/local/venv/postgresql

# Should optional packages (devel) be installed
postgresql_install_optional_packages: false

# List of packages
postgresql_pip_packages:
  - psycopg2-binary

# Password encryption to use
postgresql_password_encryption_scheme: scram-sha-256

# Service name
postgresql_service: postgresql

# Data direcotry location
postgresql_data_dir: /var/lib/pgsql/data

# Pwsotgresql encryption
# postgresql_encryption_scheme: md5
postgresql_encryption_scheme: scram-sha-256

# Postgresql OS user / group
postgresql_user: postgres
postgresql_group: postgres

# Dict of postgrewsql settings
postgresql_settings:
  listen_addresses: "127.0.0.1"
  password_encryption: "{{ postgresql_encryption_scheme }}"

# List of HBA entries (remote connections)
postgresql_hba_entries:
  - { type: local, db: all, user: all, address: '', method: peer }
  # - { type: host, db: all, user: all, address: '127.0.0.1/32', method: ident }
  # - { type: host, db: all, user: all, address: '::1/128', method: ident }
  - { type: local, db: replication, user: all, address: '', method: 'peer' }
  - { type: host, db: replication, user: all, address: '127.0.0.1/32', method: 'ident' }
  - { type: host, db: replication, user: all, address: '::1/128', method: 'ident' }
  - { type: host, db: all, user: all, address: '127.0.0.1/32', method: '{{ postgresql_password_encryption_scheme }}' }
</pre></code>

### defaults/family-Debian.yml
<pre><code>
# OSS repository url
postgresql_repo: https://apt.postgresql.org/pub/repos/apt

# OSS APT GPG key
postgresql_gpg_key: https://www.postgresql.org/media/keys/ACCC4CF8.asc

# Lists of packages
postgresql_server_packages:
  - postgresql-{{ postgresql_version }}
postgresql_client_packages:
  - postgresql-client-{{ postgresql_version }}
postgresql_optional_packages:
  - libpq-dev

# binary/data/etc directory
postgresql_bin_dir: /usr/lib/postgresql/{{ postgresql_version }}/bin
postgresql_data_dir: /var/lib/postgresql/{{ postgresql_version }}/main
postgresql_etc_dir: /etc/postgresql/{{ postgresql_version }}/main
</pre></code>

### defaults/family-RedHat-8.yml
<pre><code>
# List of optional packages
postgresql_optional_packages:
  - postgresql{{ postgresql_version }}-devel
  - python3-wheel
</pre></code>

### defaults/family-RedHat.yml
<pre><code>
# Repository gpg url
postgresql_repo: https://download.postgresql.org/pub/repos/yum/{{ postgresql_version }}/redhat/rhel-$releasever-$basearch

# Repository gpg key
postgresql_gpg_key: >-
  https://download.postgresql.org/pub/repos/yum/keys/PGDG-RPM-GPG-KEY-RHEL

# List of required packages
postgresql_server_packages:
  - postgresql{{ postgresql_version }}-server
postgresql_client_packages:
  - postgresql{{ postgresql_version }}
postgresql_optional_packages:
  - postgresql{{ postgresql_version }}-devel

# Install optional packages
postgresql_install_optional_packages: true

# Binary/data/etc directory location
postgresql_bin_dir: /usr/pgsql-{{ postgresql_version }}/bin
postgresql_data_dir: /var/lib/pgsql/{{ postgresql_version }}/data
postgresql_etc_dir: /var/lib/pgsql/{{ postgresql_version }}/data

# Service
postgresql_service: postgresql-{{ postgresql_version }}
</pre></code>




## Example Playbook
### molecule/default/converge.yml
<pre><code>
- name: sample playbook for role 'postgresql'
  hosts: all
  become: 'yes'
  vars:
    postgresql_db_name: test
    postgresql_db_user: test
    postgresql_db_password: test
  tasks:
    - name: Install postgresql client
      ansible.builtin.include_role:
        name: postgresql
      vars:
        postgresql_install_type: client
    - name: Install postgresql server
      ansible.builtin.include_role:
        name: postgresql
      vars:
        postgresql_install_type: server
</pre></code>
