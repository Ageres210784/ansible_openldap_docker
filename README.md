openldap_docker
=========

The role for install and configure openldap in docker container.

Requirements
------------

You need to install `jmespath`
```
pip install jmespath
```

Role Variables
--------------

All variables you can see in defaults.

To change:
- `ldap_root` - domain/organisation
- `ldap_mdb_olcAccess` - olcAccess. Example:
```yaml
ldap_mdb_olcAccess:
  - >-
    {0}to *
    by dn.exact=gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth manage
    by * break
  - >-
    {1}to attrs=userPassword,shadowLastChange
    by self write
    by dn="cn={{ ldap_admin_username }},{{ ldap_root }}" write
    by anonymous auth
    by * none
  - >-
    {2}to *
    by self read
    by dn="cn={{ ldap_admin_username }},{{ ldap_root }}" write
    by dn="cn={{ ldap_readonly_username }},{{ ldap_root }}" read
    by * none
  # - >-
  #   to attrs=userPassword
  #   by self write
  #   by anonymous auth
  #   by * none
  # - >-
  #   to *
  #   by self write
  #   by * read
```
- `group_list` - list of groups. Example:
```yaml
groups_list:
  - name: sudo
    description: "sudo group"
    gidNumber: 27
  - name: group1
    description: "The group for test_user1"
```
- `users_list` - list of users and groups in which the user is included. Example:
```yaml
users_list:
  - firstName: UserName1
    lastName: UserLastName1
    passwordHash: "{SSHA}2EQP9Ki9O2hbc/n0F3hSG7P2YOEHpmnz" # slappasswd -s password
    ssh_key: "ssh-rsa SOME_RSA_PUBLICK_KEY=="
    groups:
      - group1
      - sudo
```

Example Playbook
----------------

```yaml
- hosts: ldap_hosts
  become: yes
  roles:
      - ageres210784.openldap_docker
```

Example run
-----------

Run in server
```
ansible-playbook run-openldap.yml -l ldap-server -t server
```
Run in client
```
ansible-playbook run-openldap.yml -l ldap-client -t client
```

License
-------

Apache 2.0

Author Information
------------------

[Evseev Sergey](https://github.com/Ageres210784)
