# -*- mode: ruby -*-
# vi: set ft=ruby :

box = 'btexpress/ubuntu64-14.04'
#box = 'ubuntu/precise64'

$install_hosts = <<-eos
cat <<HERE > /etc/hosts
127.0.0.1        localhost
192.168.33.10    ansible
192.168.33.20    one
192.168.33.21    two
192.168.33.22    three
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
one
two
three
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

  # Take this line out eventuall
#  config.vbguest.auto_update = false
  #

  config.vm.provision "shell", inline: $install_hosts
  

  config.vm.define "ansible" do |ansible|
    ansible.vm.hostname = "ansible"
    ansible.vm.network "private_network", ip: "192.168.33.10"
    ansible.vm.provision "shell", inline: $skeleton_key_install
    ansible.vm.provision "shell", inline: $ansible_install
    ansible.vm.provision "shell", inline: $ansible_vagrant_key_install
    ansible.vm.provision "shell", inline: $ansible_root_key_install
  end

  config.vm.define "slave1" do |one|
    one.vm.hostname = "one"
    one.vm.network :private_network, ip: "192.168.33.20"
    one.vm.provision "shell", inline: $slaves_vagrant_key_install
    one.vm.provision "shell", inline: $slaves_root_key_install
  end

  config.vm.define "slave2" do |two|
    two.vm.hostname = "two"
    two.vm.network :private_network, ip: "192.168.33.21"
    two.vm.provision "shell", inline: $slaves_vagrant_key_install
    two.vm.provision "shell", inline: $slaves_root_key_install
  end

  config.vm.define "slave3" do |three|
    three.vm.hostname = "three"
    three.vm.network :private_network, ip: "192.168.33.22"
    three.vm.provision "shell", inline: $slaves_vagrant_key_install
    three.vm.provision "shell", inline: $slaves_root_key_install
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
