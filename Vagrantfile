# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.require_version ">= 2.3.7"
Vagrant.configure("2") do |config|
  script_choice = ENV['VAGRANT_SETUP_CHOICE'] || 'none'
  config.vm.box = "ubuntu/jammy64"
  config.disksize.size = '100GB'

  config.vm.network "forwarded_port", guest: 9000, host: 9000

  # NIC 2: Promiscuous mode
  # config.vm.network "private_network", type: "dhcp", virtualbox__intnet: "promiscuous", auto_config: false

  config.vm.provider "virtualbox" do |vb|
    vb.gui = true
    # Customize the amount of memory on the VM:
    vb.name = "CRIBL-Playground"
    vb.memory = "4096"
    vb.cpus = 4
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get upgrade -y

    # Turn off password authentication to make it easier to login
    sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
    sudo systemctl restart sshd
    sudo useradd -m -p '$1$0mYmh0yB$UI7zJJZ4jHjAnF/O7IjeC0' cribl

    cd /opt
    sudo curl -Lso - $(curl -s https://cdn.cribl.io/dl/latest) | sudo tar zxvf -
    sudo chown -R cribl:cribl /opt/cribl
    sudo /opt/cribl/bin/cribl boot-start enable -u cribl
    # sudo systemctl start cribl
    # sudo /opt/cribl/bin/cribl status

  SHELL

  config.vm.provision "reload"

end
