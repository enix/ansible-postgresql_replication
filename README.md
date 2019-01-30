enix.postgresql_replication
=================

A role for deploying and configuring [postgresql](http://www.postgresql.org) replication cluster on unix hosts using [Ansible](http://www.ansible.com/).


Requirements
------------

Supported targets:

- Debian 8 "Jessie"
- Debian 9 "Stretch"


Role Variables
--------------

This roles comes preloaded with almost every available default. You can override each one in your hosts/group vars, in your inventory, or in your play. See the annotated defaults in `defaults/main.yml` for help in configuration. All provided variables start with `postgresql_replication__`.

This role work in a different way than several others in a way that in order to make different cluster hosts roles independent we use ansible groups. There is so 3 different groups to define in your inventory. to make this usable with different clusters in the same inventory they are configurable.
- `postgresql_replication__group` - the main group of hosts for this cluster, default: `postgresql`
- `postgresql_replication__group_master` - the master group that must include only *one host*, default: `postgresql-master`
- `postgresql_replication__group_replicas` - the group including the replicas hosts, default: `postgresql-replicas`
Your inventory must have in final this form (yaml format exemple):
```
all:
  children:
    postgresql:
      children:
        postgresql-master:
          hosts:
            db-01:
              ansible_host: 185.145.251.12
        postgresql-replicas:
          hosts:
            db-02:
              ansible_host: 185.145.251.243
```
- `postgresql_replication__user` - the username to use for authentification of replication nodes. default : `replicate`.
- `postgresql_replication__password` - the postgresql_replication__user password. *It is important to change default on production clusters*. default: `replicate`.
- `postgresql_replication__waldir` - Directory used to store WAL file sent from the master node to the replication nodes.

Sending servers configuration. See (https://www.postgresql.org/docs/11/runtime-config-replication.html#RUNTIME-CONFIG-REPLICATION-SENDER) for details of correct values for your setup.
- `postgresql_replication__walsenders` - Number of wal sender processes to enable. default : `3`.
- `postgresql_replication__walsegments` - Number of wal segments to keep (16MB each). default: `64`.

Dependencies
------------

- [enix.postgresql](https://galaxy.ansible.com/enix/postgresql)

Usage
-----

Use Ansible galaxy requirements.yml

    - src: enix.postgresql_replication

And add it to your play's roles:

    - hosts: all
      roles:
        - role enix.postgresql_replication:
            postgresql_replication__var: true

You can also use the role as a playbook. You will be asked which hosts to provision, and you can further configure the play by using `--extra-vars`.

    $ ansible-playbook -i inventory --extra-vars='{...}' main.yml

Still to do
-----------

- Write the role itself, for one


Changelog
---------

### 0.1

Initial version.

License
-------

GPLv2

Author Information
------------------

Laurent CORBES <laurent.corbes@enix.fr> - http://www.enix.io
