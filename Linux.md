# Table of Contents
1. [BASH](#bash)
1. [SSH](#ssh)
1. [GIT](#git)
1. [JS](#javascript)
1. [Salt](#saltstack)

# <a name="bash">BASH</a>

## ˜/.bashrc

### git
The following executes every time to alter the current shell location to add the branch on current git repo folder

    _git_branch='`git branch 2> /dev/null | grep -e ^* | sed -E s/^\\\\\*\ \(.+\)$/\(\\\\\1\)\/`'
    export PS1="$PS1$_git_branch"
- [Other handy bashrc settings](https://github.com/magicmonty/bash-git-prompt)

# <a name="ssh">SSH</a>

## TUNNEL

SSH forward port

- D: Bind port
- C: Compress
- N: No command executed

`ssh -D 8080 -C -N username@example.com`

## PROXY with identities

```bash
BASTION_IP=1.2.3.4
INTERNAL_IP=$IPYOUWANTTOCONNECTTO; ssh -o IdentitiesOnly=yes -o ProxyCommand="ssh ${USER}@${BASTION_IP} nc ${INTERNAL_IP} 22 ${USER}" ${USER}@${INTERNAL_IP}
```

# <a name="git">GIT</a>
- [Cheat sheet](https://gist.github.com/akras14/3d242d80af8388ebca60)
- [bisect](https://git-scm.com/docs/git-bisect): handy for binary searching commit that broke things

# <a name="javascript">JavaScript</a>

## NPM
- [Tutorial](https://www.sitepoint.com/beginners-guide-node-package-manager/)

### Set global install environment to some local folder (similar to venv)
`cd && mkdir .node_modules_global`

`npm config set prefix=$HOME/.node_modules_global`

This also creates a `.npmrc` file in our home directory. Then install npm in the local path.

`npm install npm --global`

Finally add new npm to path in .bashrc

`$HOME/.node_modules_global/bin:$PATH`

#  <a name="saltstack">Saltstack</a>
  Salt - ZeroMQ or RAET transport - Upgrade master always first

## Basic https://docs.saltstack.com/en/latest/topics/tutorials/walkthrough.html

## States: https://docs.saltstack.com/en/latest/topics/tutorials/walkthrough.html#salt-states

simple srv/client encrypted small payload
https://docs.saltstack.com/en/latest/ref/configuration/index.html
`salt <server> state.highstate test=True`

### config - https://docs.saltstack.com/en/latest/ref/configuration/master.html

`/etc/salt/[master|minion]`

#### for minion

set master, egs:
`master: bla.com`
`master: 10.1.1.1`

- default port for master 4505 and 4506

### Keys should be verified

- on master

`salt-key -F master`

- copy master.pub into the minion config master_finger
restart minion

### Start master:

- to daemonize pass -d

`salt-master [-d]`

### Start minion:

`salt-minion [-d]`

### debug

`salt-master [--log-level=debug]`

log_file: udp://loghost:10514
external log: log stash!
https://docs.saltstack.com/en/latest/ref/configuration/logging/handlers/index.html

### Test - will dry run update

`salt '*' state.highstate --state-output=changes test=True`

### Check

`salt-call -l trace --local state.highstate`

### Check versions

`salt-run manage.versions`

#### run without root

set <user> in master config file


## States application

The states that will be applied to a minion in a given environment can be viewed using the

`state.show_top` 

`salt <minion> state.show_top`

### Pin environment

`salt <minion> state.highstate saltenv=prod`

### Functions 

<https://docs.saltstack.com/en/latest/ref/modules/index.html>

#### Checks

##### Check connection

`salt <target> test.ping`

`salt '*' test.ping`

functions in states: https://docs.saltstack.com/en/latest/ref/states/all/salt.states.module.html

```
state-name:
  module.run:
    - name: network.interfaces
```

With kwargs

```
state-name:
  module.run:
    - name: network.ip_addrs
    - func: network.ip_addrs
    - kwargs: {
      interface: eth0
    }
```

kwargs is a dict

#### Targets: regex / minion names, grains, pillar, IP, compound, node group

Run an arbitrary shell command:

`salt '*' cmd.run 'uname -a'`

Better output

`salt myminion grains.item pythonpath --out=pprint`

## Salt fileserver!!!!

<https://docs.saltstack.com/en/latest/ref/file_server/index.html>


### States

basic = salt/*.sls

`/srv/salt/my_state.sls`

`salt '*' state.apply my_state`

#### high state

`salt '*' state.apply state,high state`

no need for argument since salt 2015.5.0

`salt '*' state.apply`

Better = salt/my_state/init.sls

`salt '*' state.apply my_state`

Even Better = salt/my_higher_level_state/my_substate.sls

`salt '*' state.apply my_higher_level_state`

or

`salt '*' state.apply my_higher_level_state.my_substate`

##### Run a single ID

state.sls_id ID MOD
eg:

`salt '*' state.sls_id my_id apps.my_app.module`


## Pillar

Ensure minions have the new pillar data

`salt '*' saltutil.refresh_pillar`

Retrieve pillar

`salt '*' pillar.items`

## Fileserver

Configure master
Remove lock

`salt-run fileserver.clear_lock gitfs`

## Services

in an .sls file
eg:

```
module-started:
  service.running:
    - name: mod_name
    - enable: True
    - sig: 'cmd'
```

watch: can be used with a service.running to restart a service when another state changes
(example a file.managed state)
