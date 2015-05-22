# salt-windows-dev-vagrant

**Proof of concept for a vagrant enabled windows devbox.**

This is a vagrant setup to bootstrap a windows development and testing environment with minimal effort. This can be used by developers and by test automation systems.

## Pererquisites

Host environment with [VirtualBox](https://www.virtualbox.org/) and [Vagrant](https://docs.vagrantup.com) installed.

### Vagrant windows box

This box will provided in the vagrant cloud as soon as somebody gets around to resolve [this isse](https://github.com/obestwalter/salt-windows-dev/issues/5). for now you can follow the steps described [here](https://github.com/obestwalter/salt-windows-dev/issues/5).

## Usage

1. You have to tell vagrant a bit about your environment.

    export SALT_VAGRANT_BOX="name of your vagrant windows box"
    
    export SALT_VAGRANT_SOURCES_PATH="path to your local salt checkout"
    
2. Change into your clone or download of this repository and type ``vagrant up``.

3. Done

With ``vagrant rdp`` you can open a remote desktop connection (you can also use a normal rdesktop client to connect to the IP that is configured in the ``Vagrantfile``).

