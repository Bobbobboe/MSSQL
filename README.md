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