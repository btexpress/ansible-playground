# -*- mode: ruby -*-
# vi: set ft=ruby :

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
#ansible config
mkdir -p /etc/ansible
cat <<HERE > /etc/ansible/hosts
one
two
three
HERE
eos


Vagrant.configure("2") do |config|

  config.vm.box = "precise64"

  config.vm.provision "shell", inline: $install_hosts
  

  config.vm.define "ansible" do |ansible|
    ansible.vm.hostname = "ansible"
    ansible.vm.network "private_network", ip: "192.168.33.10"
    ansible.vm.provision "shell", inline: $skeleton_key_install
    ansible.vm.provision "shell", inline: $ansible_install
  end

  config.vm.define "slave1" do |one|
    one.vm.hostname = "one"
    one.vm.network :private_network, ip: "192.168.33.20"
  end

  config.vm.define "slave2" do |two|
    two.vm.hostname = "two"
    two.vm.network :private_network, ip: "192.168.33.21"
  end

  config.vm.define "slave3" do |three|
    three.vm.hostname = "three"
    three.vm.network :private_network, ip: "192.168.33.22"
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
