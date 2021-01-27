VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "freebsd/FreeBSD-12.2-RELEASE"
  config.vm.box_version = "2020.10.23"
  datadisk = "./datadisk.vdi"

  config.vm.network "forwarded_port", guest: 8079, host: 8079, host_ip: "127.0.0.1"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 8192
    vb.cpus = 4
    if not File.exists?(datadisk)
      vb.customize ['createhd', '--filename', datadisk, '--variant', 'Standard', '--size', 80 * 1024] # 80G
    end
    vb.customize ['storageattach', :id,  '--storagectl', 'IDE Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', datadisk]
  end

  config.vm.provision "shell",
    inline: "pkg install -y python"

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.extra_vars = {
      ansible_python_interpreter: "/usr/local/bin/python",
    }
  end
end
