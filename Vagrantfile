# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

# This Vagrantfile will create a new VirtualBox VM with the following configuration:
#
# * Operating system: CentOS 7
# * Memory: 2GB
# * CPUs: 2
# * Disk size: 10GB

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # The name of the VM
  config.vm.hostname = "centos-vm"

  # The operating system to use
  config.vm.box = "centos/7"
  # The amount of memory to allocate to the VM
  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
    v.cpus = 2
  end

  # The size of the virtual disk
  config.vm.disk :disk, size: "10GB", primary: true

  # Set the network adapter to bridge mode
  config.vm.network "public_network", bridge: "wlp2s0"
  config.vm.provision "shell", inline: <<-'SHELL'
    sed -i 's/^#* *\(PermitRootLogin\)\(.*\)$/\1 yes/' /etc/ssh/sshd_config
    sed -i 's/^#* *\(PasswordAuthentication\)\(.*\)$/\1 yes/' /etc/ssh/sshd_config
    systemctl restart sshd.service
    echo -e "vagrant\nvagrant" | (passwd vagrant)
    echo -e "root\nroot" | (passwd root)
  SHELL
  # Provision the VM with a shell script that will output the IP address
  config.vm.provision "shell", inline: <<-SHELL
    ip a
  SHELL
end