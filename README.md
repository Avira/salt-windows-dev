# salt-windows-dev 

**Proof of concept for a vagrant enabled windows devbox.**

This is a vagrant setup to bootstrap a windows development and testing environment with minimal effort. This can be used by developers and by test automation system.

## Pererquisites

[Vagrant](https://docs.vagrantup.com)

### Vagrant windows box

This box might be provided at some point in the future, when some legal questions are clarified.

For now you have to package a VirtualBox image yourself like described in [this Tutorial](https://dennypc.wordpress.com/2014/06/09/creating-a-windows-box-with-vagrant-1-6/)


Additionally you have to

* switch of [password complexity](http://stackoverflow.com/questions/23260656/modify-local-security-policy-using-powershell) 
* create a user 'vagrant' with password 'vagrant' that is member in 'Administrators' group


## Usage

1. You have to tell vagrant a bit about your environment.

    export SALT_VAGRANT_BOX="<name of your vagrant windows box>"
    
    export SALT_VAGRANT_SOURCES_PATH="<path to your local salt checkout>"
    
2. Change into your clone or download of this repository and type ``vagrant up``.

3. Done

With ``vagrant rdp`` you can open a remote desktop connection (you can also use a normal rdesktop client to connect to the IP that is configured in the ``Vagrantfile``).


This has only been tested in a 64bit environment... although it should work in either.

## Problems
* If using Write-Output the following error occurs: "Error on The OS handle's position is not what FileStream expected. Do not use a handle simultaneously in one FileStream and in Win32 code or another FileStream. This may cause data loss". So this is commented out till a PowerShell Guru can tell me how to prevent this.


## Build Salt (2015.2 development branch and later)
Building salt has additional requirements not installed by this script. You'll need to install the NullSoft installer. That can be downloaded here: http://sourceforge.net/projects/nsis/files/NSIS%203%20Pre-release/3.0b1/nsis-3.0b1-setup.exe/download

### Build Steps
The build folders for windows are in the Salt repository as follows: ```<parent folder>\salt\pkg\windows```

The ```buildenv``` folder contains files that will be installed along with salt. The ```installer``` folder contains the files needed to create the .exe.
- Go into the ```installer``` folder and edit the file named: ```Salt-Minion-Setup.nsi```
- Change the ```PRODUCT_VERSION``` variable to reflect the branch you pulled from GitHub and save the file
- Go up one directory to ```.\salt\pkg\windows```
- Run the ```BuildSalt.bat``` file

This file does the following:
- Copies ```C:\Python27``` to ```buildenv\bin```
- Deletes unused files from ```buildenv\bin```
- Runs ```makensis``` to build the installer
- Installer is placed in the ```installer``` directory
