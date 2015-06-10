# salt-windows-dev-vagrant

**Proof of concept for a self installing, vagrant enabled Salt test and development environment.**

This is a vagrant setup to bootstrap a windows development and testing environment with minimal effort. This can be used by developers and by test automation systems.

## Prerequisites

Linux Host environment with [VirtualBox](https://www.virtualbox.org/) and [Vagrant](https://docs.vagrantup.com) installed.

### Vagrant windows box

The [public vagrant box](https://atlas.hashicorp.com/obestwalter/boxes/salt-windows-test-2k8_r2) that is preconfigured in the Vagrantfile will be working till 2015/12/6 when the windows evaluation period will run out. Until then the box building will hopefully be automated alread like described [in this issue](https://github.com/obestwalter/salt-windows-dev/issues/9).

## Usage

1. Tell vagrant where your local checkout of the salt sources are (deafult is ``~/work/salt/salt``): ``export SALT_VAGRANT_SOURCES_PATH="path to your local salt checkout"``.

2. Change into your clone or download of this repository and type ``vagrant up``.

3. Done

Note: There are more configuration options, that you can overwrite with you own environment setting if you need to. See ``confMap`` in Vagrantile for what you can do.

## Testing

It's not fully working and fully automated yet, so bare with me ...

* spin up box

    * $ cd [your clone of this repo]
    * $ SALT_VAGRANT_BOX="[name of vagrant box]" vagrant up

* Open a remote desktop connection With ``vagrant rdp`` or any rdp client to SALT_VAGRANT_BOX_IP and vagrant/vagrant credential.
* be aware of [#6](https://github.com/obestwalter/salt-windows-dev/issues/6 )
* set paths (see [#1](https://github.com/obestwalter/salt-windows-dev/issues/1))
* install salt (see [#2](https://github.com/obestwalter/salt-windows-dev/issues/2))
* problems? Call ``VAGRANT_LOG=debug vagrant up`` to see what's going on.

if ``salt-call --version`` does not bork you have a winner :)
