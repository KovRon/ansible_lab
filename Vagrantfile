# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |al|

  al.vm.define "ansible", primary: true do |ansible|
    ansible.vm.box = "centos/7"
    ansible.vm.network "private_network", ip: "192.168.33.10"
    ansible.vm.provider "virtualbox" do |v|
      v.gui = false
      v.cpus = "1"
      v.memory = "512"
    end
    ssh_pr_key = File.readlines("#{Dir.pwd}/id_rsa").first.strip
    ansible.vm.provision "shell", inline: <<-SHELL
      rpm -Uvh http://download.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-9.noarch.rpm
      yum update -y
      yum install -y ansible git
      echo '192.168.33.10 ansible\n192.168.33.11 web\n192.168.33.12 db' >> /etc/hosts
      mkdir -p /root/.ssh
      echo #{ssh_pr_key} >> /root/.ssh/id_rsa
      chmod 700 /root/.ssh
      chmod 600 /root/.ssh/id_rsa
      chown root:root -R /root/.ssh
    SHELL
  end

  al.vm.define "web", primary: true do |web|
    web.vm.box = "centos/7"
    web.vm.network "private_network", ip: "192.168.33.11"
    web.vm.provider "virtualbox" do |v|
      v.gui = false
      v.cpus = "1"
      v.memory = "512"
    end
    ssh_pub_key = File.readlines("#{Dir.pwd}/id_rsa.pub").first.strip
    web.vm.provision "shell", inline: <<-SHELL
      echo '192.168.33.10 ansible\n192.168.33.11 web\n192.168.33.12 db' >> /etc/hosts
      mkdir -p /root/.ssh
      echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
      chmod 700 /root/.ssh
      chmod 600 /root/.ssh/authorized_keys
      chown root:root -R /root/.ssh
    SHELL
  end

  al.vm.define "db", primary: true do |db|
    db.vm.box = "centos/7"
    db.vm.network "private_network", ip: "192.168.33.12"
    db.vm.provider "virtualbox" do |v|
      v.gui = false
      v.cpus = "1"
      v.memory = "512"
    end
    ssh_pub_key = File.readlines("#{Dir.pwd}/id_rsa.pub").first.strip
    db.vm.provision "shell", inline: <<-SHELL
      echo '192.168.33.10 ansible\n192.168.33.11 web\n192.168.33.12 db' >> /etc/hosts
      mkdir -p /root/.ssh
      echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
      chmod 700 /root/.ssh
      chmod 600 /root/.ssh/authorized_keys
      chown root:root -R /root/.ssh
    SHELL
  end

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

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end
