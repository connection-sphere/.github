# Installation On Production Environment

This guide installs the master, slaves, workers nodes on a distributed mesh of nodes.

## Prerequisites

- Rent one Contabo VPS 10 for each node you have to install.

## Installing Master Node

Only one master node is required, so you likely will perform this task once.

1. Edit the secret file `BlackOpsFile` to add your master node.

```ruby
BlackOps.add_node({
    :name => 'master',
    :ip => '195.........21',
    :domain => 'connectionsphere.com',
    # ssh passwords
    :ssh_password => '...',
    :ssh_root_password => '...',
    # postgres password
    :postgres_password => '...',
    ...
})
```

2. Create domain `A` record to point to node's IP.

3. Install the environment, deploy source code, migrate database, start the services.

```bash
export OPSLIB=~/code1/secret/production && \
saas install --node=master --root && \
saas deploy --node=master && \
saas migrations --node=master && \
saas start --node=master --root
```

## Scaling Salve Nodes

1. Edit the secret file `BlackOpsFile` to add your master node.

```ruby
# slave nodes
#
#
{
    ...
    free02: '62.........223', 
}.map { |key, value|
    BlackOps.add_node({
        :name => key.to_s,
        :ip => value.to_s,
        :domain => "#{key.to_s}.connectionsphere.com",
        # ssh passwords
        :ssh_password => '...',
        :ssh_root_password => '...',
        # postgres password
        :postgres_password => '...',
        ...
    }.merge(
        @t[:ssh],
        @t[:postgres],
        @t[:git],
        @t[:adspower],
        @t[:commons],
        @t[:emails],
        @t[:slave_22_04],
        @t[:monitoring],
    ))
}
```

2. Create domain `A` record to point to node's IP.

3. Add your node to the Contabo merging list _(deprecated)_

```ruby
# list of slave nodes for resources allocation
:slaves => [{
    ...
}, {
    :url_port => 'free02.connectionsphere.com:443',
    ...
}],
```

4. Install the environment, deploy source code, migrate database, start the services.

```bash
export OPSLIB=~/code1/secret/production && \
saas install --node=free02 --root && \
saas deploy --node=free02 && \
saas migrations --node=free02 && \
saas start --node=free02 --root
```

## Scaling Worker Nodes

1. Edit the secret file `BlackOpsFile` to add your master node.

```ruby
# worker nodes
#
#
{
    ...
    w01e: '164.........140',
}.map { |key, value|
    BlackOps.add_node({
        :name => key.to_s,
        :ip => value.to_s,
        # ssh passwords
        :ssh_password => 'SanCristobal943',
        :ssh_root_password => 'SanCristobal943',
        ...
    }.merge(
        @t[:ssh],
        @t[:git],
        @t[:adspower],
        @t[:commons],
        @t[:worker],
        @t[:install_worker_ubuntu2204],
        @t[:monitoring],
    ))
}
```

2. Add your node to the Contabo merging list _(deprecated)_

```ruby
# list of worker nodes for resources allocation
:workers => [{
    ...
},{
    :name => 'w01e',
    :max_rpa => 3,
    :max_api_mta => 10,
}],
```

3. Install the environment, deploy source code, start the services.

```bash
export OPSLIB=~/code1/secret/production && \
saas install --node=w01e --root && \
saas deploy --node=w01e && \
saas start --node=w01e --root
```

## Refresh List of Contabo Nodes _(deprecated)_

Always refresh the array of **slave** and **worker** nodes on the **master** node - The master node will merge such a list with Contabo information (e.g.: auto-renewal).

```bash
export OPSLIB=~/code1/secret/production && \
saas deploy --node=master && \
saas stop --node=master  --root && \
saas start --node=master --root
```
