# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |al|

  al.vm.define "web" do |web|
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

  al.vm.define "db" do |db|
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

  al.vm.define "ansible", primary: true do |ansible|
    ansible.vm.box = "centos/7"
    ansible.vm.network "private_network", ip: "192.168.33.10"
    ansible.vm.provider "virtualbox" do |v|
      v.gui = false
      v.cpus = "1"
      v.memory = "512"
    end
    ssh_prv_key = File.read("#{Dir.pwd}/id_rsa")
    ansible.vm.provision "shell", inline: <<-SHELL
      rpm -Uvh http://download.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-10.noarch.rpm
      yum update -y
      yum install -y ansible git
      cd /root && git clone https://github.com/KovRon/ansible_lab.git
      echo '192.168.33.10 ansible\n192.168.33.11 web\n192.168.33.12 db' >> /etc/hosts
      mkdir -p /root/.ssh
      echo "#{ssh_prv_key}" > /root/.ssh/id_rsa
      chmod 700 /root/.ssh
      chmod 600 /root/.ssh/id_rsa
      chown root:root -R /root/.ssh
      sed -i.bak 's/#host_key_checking = False/host_key_checking = False/' /etc/ansible/ansible.cfg
      cd /root/ansible_lab && ansible-playbook -i hosts all.yml
    SHELL
  end
end
