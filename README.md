# Patroni

An Ansible role which installs and configures [Patroni](https://github.com/zalando/patroni/) - HA solution for PostgreSQL.

## Requirements

This role requires root privileges, so tell ansible to use `become: true` in any [convenient way](http://docs.ansible.com/ansible/latest/become.html) for you.

## Role Variables

    patroni_postgresql_version                      # [default: 11]
    patroni_postgresql_exists:                      # [true | false] skip postgresql installation
    patroni_cluster_name:                           # [required] name of cluster (and kv if dcs is consul) 
    
    patroni_dcs:                                    # [etcd | consul | zookeeper | exhibitor]
    patroni_dcs_exists:                             # [default: true] for preinstall dcs
    
    patroni_postgresql_create_replica_methods:      # [default: basebackup] pgbackrest & wal_e & basebackup
    patroni_backup_on_copy:                         # [true | false] create a backup file before copying files on hosts
    
    patroni_replication_username:                   # [default: replicator]
    patroni_replication_password:                   # [default: repuserpasswd]
    patroni_superuser_username:                     # [default: postgres]
    patroni_superuser_password:                     # [default: supersecretpostgrespasswd]
    
    patroni_watchdog_mode: automatic                # use quotes for 'off' value

    patroni_encvolume: false                        # [true | false] encrypt second volume fore data store
    patroni_encvolume_key:                          # if not set, will be generated and store to {{ patroni_encvolume_keyfile_path }}
    patroni_encvolume_keyfile_path:                 # [default: /opt/keyfile] path to store patroni_encvolume_key


## Dependencies

There are no dependencies for the role, but Patroni itself needs a DCS (Etcd, Consul, ZooKeeper or Exhibitor) to be installed and configured properly and it's your responsibility to make it up and running before using this role.
Currently, it is supposed that a DCS is prepared. Otherwise, you can try one of the following roles (just uncomment respective section [here](https://github.com/kostiantyn-nemchenko/ansible-role-patroni/blob/master/meta/main.yml#L28) and set `patroni_dcs_exists` variable to false):

* [**andrewrothstein.etcd-cluster**](https://github.com/andrewrothstein/ansible-etcd-cluster)
* [**brianshumate.consul**](https://github.com/brianshumate/ansible-consul)
* [**AnsibleShipyard.ansible-zookeeper**](https://github.com/AnsibleShipyard/ansible-zookeeper)
Currently, it is supposed that a DCS is prepared.

## Example Playbook

tests/consul-patroni.yml

## Example Run

ansible-playbook tests/consul-patroni.yml -i tests/consul-patroni

## License

MIT
