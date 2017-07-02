Ansible with vagrant playground
===============================

This repository presents an example of ansible playbook in combination with
Vagrant Docker provider.  In development of ansible playbooks, write, fix and
run playbooks over and over again.  This iteration not only delivery
inessential files, but also makes difficult to guarantee the playbooks
configure the host correctly from initial state.

A container technology is a good solution of the problems described previously
since the construction and destruction of the environment is easy and fast.
Vagrant accelerate the container management as ansible playground.  This
repository contains `Vagrantfile` and ansible playbools for an Elasticsearch
clustiner.

Vagrantfile in this repository creates the following hosts:

- `apt-cacher`: an apt cache server
- `elasticsearch-client`: a client node and endpoint of the cluster.
- `elasticsearch-master-{n}`: master nodes in the cluster.
- `elasticsearch-data-{n}`: data nodes in the cluster.

Usage
-----

### Construct containers

Close the repository to local disk:

```sh
git clone https://github.com/ueokande/ansible-vagrant-playground
```
Launch containers with `vagrant up`:

```sh
vagrant up
vagrant status
```

You can check the status of the containers:

```sh
vagrant status
```

### Run ansible playbooks

Since `vagrant ssh-config` with no hosts fails, generate ssh config to login containers by following:

```sh
vagrant status | \
  awk '/running/ { print $1 }' | \
  grep -v 'apt-cacher' | \
  xargs -n1 vagrant ssh-config >ssh_config
```

Make an ansible settings to load `ssh_config`:

```sh
cat >ansible.cfg <<EOF
[ssh_connection]
ssh_args = -F ssh_config
EOF
```

Run ansible-playbook:

```sh
ansible-playbook --inventory-file=ansible/inventories/hosts --sudo ansible/site.yml
```

You can observe Elasticsearch cluster state with `number_of_nodes` of 11 and
`number_of_data_nodes` of 6 via client node:

```sh
client_ip=$(docker inspect elasticsearch-client  | \
  jq -r '.[].NetworkSettings.Networks.elasticsearch.IPAddress')
curl http://${client_ip}:9200/_cluster/health | jq '.'
```

Licence
-------

MIT
