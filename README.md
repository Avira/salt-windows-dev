# salt-windows-dev-vagrant

**Proof of concept for a vagrant enabled windows devbox.**

This is a vagrant setup to bootstrap a windows development and testing environment with minimal effort. This can be used by developers and by test automation systems.

## Pererquisites

Host environment with [VirtualBox](https://www.virtualbox.org/) and [Vagrant](https://docs.vagrantup.com) installed.

### Vagrant windows box

This box might be provided at some point in the future, when some legal questions are clarified.

For now you have to package a VirtualBox image yourself like described in [this Tutorial](https://dennypc.wordpress.com/2014/06/09/creating-a-windows-box-with-vagrant-1-6/)


Additionally you have to

* switch off [password complexity](http://stackoverflow.com/questions/23260656/modify-local-security-policy-using-powershell) 
* create a user 'vagrant' with password 'vagrant' that is member in 'Administrators' group


## Usage

1. You have to tell vagrant a bit about your environment.

    export SALT_VAGRANT_BOX="name of your vagrant windows box"
    
    export SALT_VAGRANT_SOURCES_PATH="path to your local salt checkout"
    
2. Change into your clone or download of this repository and type ``vagrant up``.

3. Done

With ``vagrant rdp`` you can open a remote desktop connection (you can also use a normal rdesktop client to connect to the IP that is configured in the ``Vagrantfile``).

