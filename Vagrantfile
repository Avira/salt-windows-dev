# Fetch values from environment or use defaults
confMap = {
    # the vagrant box that should be used
    "SALT_VAGRANT_BOX" => "obestwalter/salt-windows-test-2k8_r2",
    # path to your salt checkout on the host
    "SALT_VAGRANT_SOURCES_PATH" => "~/work/salt/salt",
    "SALT_VAGRANT_BOX_IP" => "10.10.60.12",
}
puts "settings (can be changed in environemnt)"
confMap.each do |key, value|
    if ENV[key]
        confMap[key] = ENV[key]
    end
    puts "#{key}: #{confMap[key]}"
end

Vagrant.require_version ">= 1.7.2"
Vagrant.configure('2') do |config|
    config.ssh.insert_key = false
    config.vm.provider "virtualbox" do |vb|
        vb.memory = 2048
        vb.cpus = 2
        # might be useful to limit host cpu use, if vagrant is greedy
        # vb.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
    end

    config.vm.define :win2k8r2 do |c|
        c.vm.box = confMap["SALT_VAGRANT_BOX"]
        c.vm.hostname = "salt-vagrant-dev"
        c.vm.guest = :windows
        c.vm.communicator = "winrm"
        c.winrm.username = "vagrant"
        c.winrm.password = "vagrant"
        c.vm.network :private_network, ip: confMap["SALT_VAGRANT_BOX_IP"]
        c.vm.network "forwarded_port", guest: 3389, host: 33389, id: "rdp", auto_correct: true
        c.vm.synced_folder confMap["SALT_VAGRANT_SOURCES_PATH"],
            "c:/salt-dev", :mount_options => ["ro"]
        c.vm.provider :virtualbox do |vb|
            vb.name = c.vm.hostname
        end
        c.vm.provision "shell", path: "vagrant_dev_env.ps1"
    end
end
