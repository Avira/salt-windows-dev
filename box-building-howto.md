# Box Building Howto

This particular howto is for Windows Server 2008 R2 but should be easily adaptable to different versions of Windows (fingers crossed),

Inpired by: https://dennypc.wordpress.com/2014/06/09/creating-a-windows-box-with-vagrant-1-6/

VirtualBox Version: 4.3.12

Used Windows ISO: https://www.microsoft.com/en-us/download/details.aspx?id=11093

# VM creation

    mkdir output
    export VROOT=$(pwd)/output
    VBoxManage createvm --name "w2k8_r2_base" --ostype Windows2008_64 --basefolder $VROOT --register
    VBoxManage modifyvm "w2k8_r2_base" --memory 2048 --cpus 2 --vram 128 --acpi on --boot1 dvd --nic1 nat
    cd $VROOT/w2k8_r2_base
    VBoxManage createhd --filename ./w2k8_r2_base.vdi --size 30000
    VBoxManage storagectl "w2k8_r2_base" --name "SATA" --add sata
    VBoxManage storageattach "w2k8_r2_base" --storagectl "SATA" --port 0 --device 0 --type hdd --medium ./w2k8_r2_base.vdi
    VBoxManage createhd --filename ./w2k8_r2_base_d.vdi --size 20000
    VBoxManage storageattach "w2k8_r2_base" --storagectl "SATA" --port 1 --device 0 --type hdd --medium ./w2k8_r2_base_d.vdi
    VBoxManage storagectl "w2k8_r2_base" --name "IDE" --add ide
    VBoxManage storageattach "w2k8_r2_base" --storagectl "IDE" --port 0 --device 0 --type dvddrive --medium ../../en_windows_server_2008_r2_with_sp1_x64_dvd_617601.iso

# Installation (in GUI mode)

* Language to install: English
* Time and currency format: English
* Keyboard or input method: English (United States)
* Windows Server 2008 R2 Standard (Full Installation)
* Custom Installation: Disk 0

# After Installation (in GUI mode)

* Install Guest Additions (VirtualBoxMenu: Devices -> Insert guest additions image)
* Eject Guest Additions CD image
* VirtualBoxMenu: Devices Activate Bidirectional Clipboard and Drag'nDrop
* Activate Windows (just click next without entering Product Key)
* set computer name to `w2k8-r2-eval`
* Change Drive letter of DVD drive to `E` and initialize second harddisk at `D`
* gpedit.msc
    * computer configuration -> windows settings -> security settings -> account policies
        * maximum password age: 0
        * password mus meet complecxity requirements: disabled
    * computer configuration -> administrative templates -> system
        * display shutdown event tracker: Disabled

* set Administrator pw to vagrant
* add user vagrant / vagrant (as Administrator)
* switch to user vagrant
* Run Powershell as Administrator (right click menu): C:\> Set-ExecutionPolicy Unrestricted)

## Activate WinRM (in cmd.exe NOT Powershell)

    winrm quickconfig -q
    winrm set winrm/config/winrs @{MaxMemoryPerShellMB="300"}
    winrm set winrm/config @{MaxTimeoutms="1800000"}
    winrm set winrm/config/service @{AllowUnencrypted="true"}
    winrm set winrm/config/service/auth @{Basic="true"}

## Configure RDP
    
"My computer” -> "Remote Settings” -> "Allow connections from computers running any versions for Remote Desktop”
    
"Windows Firewall with Advance Security” check Inbound Rules for "Windows Remote Management (HTTP-In)” and "Remote Desktop (TCP-in)” are enabled.


# Vagrant packaging

    cd $VROOT
    vagrant package --base w2k8_r2_base --output salt-windows-test-2k8_r2.box

# Misc info

* No windows updates installed
* resulting new user: vagrant / vagrant
* use vagrant winrm plugin to run remote commands: https://github.com/criteo/vagrant-winrm
* 180 days usage restriction (last manual build was done 2015/9/20)
