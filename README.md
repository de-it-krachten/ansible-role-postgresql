[![CI](https://github.com/de-it-krachten/ansible-role-postgresql/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-postgresql/actions?query=workflow%3ACI)


# ansible-role-postgresql

<basic role description>



## Dependencies

#### Roles
None

#### Collections
- community.general

## Platforms

Supported platforms

- Red Hat Enterprise Linux 7<sup>1</sup>
- Red Hat Enterprise Linux 8<sup>1</sup>
- Red Hat Enterprise Linux 9<sup>1</sup>
- CentOS 7
- RockyLinux 8
- RockyLinux 9
- OracleLinux 8
- OracleLinux 9
- AlmaLinux 8
- AlmaLinux 9
- SUSE Linux Enterprise 15<sup>1</sup>
- openSUSE Leap 15
- Debian 11 (Bullseye)
- Debian 12 (Bookworm)
- Ubuntu 20.04 LTS
- Ubuntu 22.04 LTS
- Fedora 37
- Fedora 38

Note:
<sup>1</sup> : no automated testing is performed on these platforms

## Role Variables
### defaults/main.yml
<pre><code>
# Postgresql version
postgresql_version: 13

# List of packages
postgresql_packages:
  - postgresql-server

# Password encryption to use
postgresql_password_encryption_scheme: scram-sha-256

# postgresql_host_entries:
#   - "host all all 127.0.0.1/8 {{ postgresql_password_encryption_scheme }}"
postgresql_host_entries: []

#
postgresql_service: postgresql

postgresql_data_dir: /var/lib/pgsql/data

#postgresql_encryption_scheme: md5
postgresql_encryption_scheme: scram-sha-256
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
