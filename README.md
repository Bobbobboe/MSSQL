# Ansible Collection - bobbobboe.mssql

# General
This is a collection of Ansible roles used to configure MSSQL servers using Ludus based and to introduce some common misconfigurations into them.

# Vulnerabilities incuded
 - All domain users granted public role
 - Password reuseage
 - Kerberoastable User granted sysadmin role
 - LA permissions granted to kerberoastable user

# TODO
 - Insecure Link Configuration
 - Impersonation

# Sample Deployment file

```yml
ludus:
  - vm_name: "{{ range_id }}-DC01"
    hostname: "DC01"
    template: win2022-server-x64-template
    vlan: 10
    ip_last_octet: 10
    ram_gb: 4
    ram_min_gb: 1
    cpus: 2
    windows:
      sysprep: true
    domain:
      fqdn: ludus.domain
      role: primary-dc
    roles:
      #- synzack.ludus_sccm.install_adcs
      - synzack.ludus_sccm.disable_firewall

  - vm_name: "{{ range_id }}-sql-01"
    hostname: "sql-01"
    template: win2022-server-x64-template
    vlan: 10
    ip_last_octet: 11
    ram_gb: 4
    ram_min_gb: 1
    cpus: 4
    windows:
      sysprep: true
    domain:
      fqdn: ludus.domain
      role: member
    roles:
      - bobbobboe.mssql.mssql_install_as_user
      - bobbobboe.mssql.mssql_common_misconfigurations
    role_vars:
      ludus_sql_svc_account_username: srv_sql
    
  - vm_name: "{{ range_id }}-sql-02"
    hostname: "sql-02"
    template: win2022-server-x64-template
    vlan: 10
    ip_last_octet: 12
    ram_gb: 4
    ram_min_gb: 1
    cpus: 4
    windows:
      sysprep: true
    domain:
      fqdn: ludus.domain
      role: member
    roles:
      - bobbobboe.mssql.mssql_install_as_gmsa

  - vm_name: "{{ range_id }}-kali"
    hostname: "kali"
    template: kali-x64-desktop-template
    vlan: 10
    ip_last_octet: 100
    ram_gb: 4
    ram_min_gb: 1
    cpus: 4
    linux: true
    testing:
      snapshot: false
      block_internet: false

```
