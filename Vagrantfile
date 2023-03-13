# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'dotenv'
Dotenv.load('.env')


# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.

  config.vm.define :uat do |uat|

    uat.vm.box = "hashicorp/bionic64"
    uat.vm.hostname = "prod"
    uat.vm.network "private_network", ip: ENV["IP_UAT"]
    uat.vm.network "forwarded_port", guest: ENV["PORT_JENKINS"], host: ENV["PORT_JENKINS"]

    uat.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.memory = 2048
      vb.cpus = 2
    end


    uat.vm.provision "shell", inline: <<-SHELL
      echo 'export PORT_JENKINS='#{ENV['PORT_JENKINS']}'' | sudo tee -a /etc/environment
    SHELL

    uat.vm.provision "ansible" do |ansible|  
      ansible.playbook = "./ansible/playbooks/Playbook_uat.yml"
    end
  end

  config.vm.define :prod do |prod|

    prod.vm.box = "hashicorp/bionic64"
    prod.vm.hostname = "dev"
    prod.vm.network "private_network", ip: ENV["IP_PROD"]
    prod.vm.network "forwarded_port", guest: ENV["PORT_WEB"], host: ENV["PORT_WEB"]
    prod.vm.network "forwarded_port", guest: ENV["PORT_PMA"], host: ENV["PORT_PMA"]

    prod.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.memory = 2048
      vb.cpus = 2
    end

    prod.vm.provision "shell", inline: <<-SHELL
      echo 'export PORT_WEB='#{ENV['PORT_WEB']}'' | sudo tee -a /etc/environment
      echo 'export PORT_PMA='#{ENV['PORT_PMA']}'' | sudo tee -a /etc/environment
      echo 'export MYSQL_ROOT_PASSWORD='#{ENV['MYSQL_ROOT_PASSWORD']}'' | sudo tee -a /etc/environment
      echo 'export MYSQL_DATABASE='#{ENV['MYSQL_DATABASE']}'' | sudo tee -a /etc/environment
      echo 'export MYSQL_USER='#{ENV['MYSQL_USER']}'' | sudo tee -a /etc/environment
      echo 'export MYSQL_PASSWORD='#{ENV['MYSQL_PASSWORD']}'' | sudo tee -a /etc/environment
    SHELL

    prod.vm.provision "ansible" do |ansible|  
      ansible.playbook = "./ansible/playbooks/Playbook_automation_projet.yml"
    end
  end

  config.vm.provision "shell", inline: <<-SHELL
    echo 'vagrant:vagrant' | chpasswd
    mkdir -p /home/vagrant/.ssh
    chmod 0700 /home/vagrant/.ssh
    ssh-keygen -t rsa -f /home/vagrant/.ssh/id_rsa -N ''
    cat /home/vagrant/.ssh/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys
    chmod 0600 /home/vagrant/.ssh/authorized_keys
    chown -R vagrant:vagrant /home/vagrant/.ssh
  SHELL

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end
