# -*- mode: ruby -*-
# vi: set ft=ruby :

box = 'btexpress/ubuntu64-14.04'
#box = 'ubuntu/precise64'

$install_hosts = <<-eos
cat <<HERE > /etc/hosts
127.0.0.1        localhost
192.168.33.10    control
192.168.33.20    lb01
192.168.33.21    app01
192.168.33.22    app02
192.168.33.23    db01
HERE
eos

$skeleton_key_install = <<-eos
cp /vagrant/vagrant_private_key /home/vagrant/.ssh/id_rsa
chmod 600 /home/vagrant/.ssh/id_rsa
chown vagrant:vagrant /home/vagrant/.ssh/id_rsa
eos

$ansible_install = <<-eos
apt-get update
apt-get install python-dev python-pip vim -y
pip install ansible
pip install markupsafe
#ansible config
mkdir -p /etc/ansible
cat <<HERE > /etc/ansible/hosts
lb01
app01
app02
db01
HERE
eos

$ansible_vagrant_key_install = <<-eos
cp /vagrant/vagrant_rsa_keys/id_rsa* /home/vagrant/.ssh
chown vagrant:vagrant /home/vagrant/.ssh/id_rsa*
chmod 700 /home/vagrant/.ssh
chmod 600 /home/vagrant/.ssh/id_rsa*
eos

$slaves_vagrant_key_install = <<-eos
echo "" >> /home/vagrant/.ssh/authorized_keys
echo "" >> /home/vagrant/.ssh/authorized_keys
cat /vagrant/vagrant_public_key/authorized_keys >> /home/vagrant/.ssh/authorized_keys
chmod 700 /home/vagrant/.ssh
chmod 600 /home/vagrant/.ssh/authorized_keys
eos

$ansible_root_key_install = <<-eos
mkdir -p /root/.ssh
cp /vagrant/root_rsa_keys/id_rsa* /root/.ssh
chown root:root /root/.ssh
chown root:root /root/.ssh/id_rsa*
chmod 700 /root/.ssh
chmod 600 /root/.ssh/id_rsa*
eos

$slaves_root_key_install = <<-eos
mkdir -p /root/.ssh
cp /vagrant/root_public_key/authorized_keys /root/.ssh
chown root:root /root/.ssh
chown root:root /root/.ssh/authorized_keys
chmod 700 /root/.ssh
chmod 600 /root/.ssh/authorized_keys
eos


Vagrant.configure("2") do |config|

  config.vm.box = box

  # Take this line out eventually
#  config.vbguest.auto_update = false
  #

  config.vm.provision "shell", inline: $install_hosts
  

  config.vm.define "control" do |control|
    control.vm.hostname = "control"
    control.vm.network "private_network", ip: "192.168.33.10"
    control.vm.provision "shell", inline: $skeleton_key_install
    control.vm.provision "shell", inline: $ansible_install
    control.vm.provision "shell", inline: $ansible_vagrant_key_install
    control.vm.provision "shell", inline: $ansible_root_key_install
  end

  config.vm.define "lb01" do |lb01|
    lb01.vm.hostname = "lb01"
    lb01.vm.network :private_network, ip: "192.168.33.20"
    lb01.vm.provision "shell", inline: $slaves_vagrant_key_install
    lb01.vm.provision "shell", inline: $slaves_root_key_install
  end

  config.vm.define "app01" do |app01|
    app01.vm.hostname = "app01"
    app01.vm.network :private_network, ip: "192.168.33.21"
    app01.vm.provision "shell", inline: $slaves_vagrant_key_install
    app01.vm.provision "shell", inline: $slaves_root_key_install
  end

  config.vm.define "app02" do |app02|
    app02.vm.hostname = "app02"
    app02.vm.network :private_network, ip: "192.168.33.22"
    app02.vm.provision "shell", inline: $slaves_vagrant_key_install
    app02.vm.provision "shell", inline: $slaves_root_key_install
  end

  config.vm.define "db01" do |db01|
    db01.vm.hostname = "db01"
    db01.vm.network :private_network, ip: "192.168.33.23"
    db01.vm.provision "shell", inline: $slaves_vagrant_key_install
    db01.vm.provision "shell", inline: $slaves_root_key_install
  end


  # config.vm.network :forwarded_port, guest: 80, host: 8080

  # config.vm.network :private_network, ip: "192.168.33.10"

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  # config.ssh.forward_agent = true

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider :virtualbox do |vb|
  #   # Don't boot with headless mode
  #   vb.gui = true
  #
  #   # Use VBoxManage to customize the VM. For example to change memory:
  #   vb.customize ["modifyvm", :id, "--memory", "1024"]
  # end
  #
  # View the documentation for the provider you're using for more
  # information on available options.

end
