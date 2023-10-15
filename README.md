[![CI](https://github.com/de-it-krachten/ansible-role-postgresql/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-postgresql/actions?query=workflow%3ACI)


# ansible-role-postgresql

PostgreSQL installation
https://www.postgresql.org



## Dependencies

#### Roles
None

#### Collections
- community.general
- community.postgresql

## Platforms

Supported platforms

- Red Hat Enterprise Linux 8<sup>1</sup>
- Red Hat Enterprise Linux 9<sup>1</sup>
- RockyLinux 8
- RockyLinux 9
- OracleLinux 8
- OracleLinux 9
- AlmaLinux 8
- AlmaLinux 9
- Debian 11 (Bullseye)
- Debian 12 (Bookworm)
- Ubuntu 20.04 LTS
- Ubuntu 22.04 LTS

Note:
<sup>1</sup> : no automated testing is performed on these platforms

## Role Variables
### defaults/main.yml
<pre><code>
# Installation source to use (vendor or oss)
postgresql_type: oss

# Postgresql version
postgresql_version: 13

# List of packages
postgresql_os_packages:
  - postgresql-server

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

# List of packages
postgresql_os_packages:
  - postgresql-{{ postgresql_version }}

# binary/data/etc directory
postgresql_bin_dir: /usr/lib/postgresql/{{ postgresql_version }}/bin
postgresql_data_dir: /var/lib/postgresql/{{ postgresql_version }}/main
postgresql_etc_dir: /etc/postgresql/{{ postgresql_version }}/main
</pre></code>

### defaults/family-RedHat.yml
<pre><code>
# Repository gpg url
postgresql_repo: https://download.postgresql.org/pub/repos/yum/{{ postgresql_version }}/redhat/rhel-$releasever-$basearch

# Repository gpg key
postgresql_gpg_key: https://download.postgresql.org/pub/repos/yum/RPM-GPG-KEY-PGDG-{{ postgresql_version }}

# List of packages
postgresql_os_packages:
  - postgresql{{ postgresql_version }}-server

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
  become: "yes"
  vars:
    postgresql_db_name: test
    postgresql_db_user: test
    postgresql_db_password: test
  tasks:

    - name: Include role 'postgresql'
      ansible.builtin.include_role:
        name: postgresql
</pre></code>
