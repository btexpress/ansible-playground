## Ansible Playground

The [Getting Started](http://www.ansibleworks.com/docs/intro_getting_started.html) documentation
for Ansible assumes you have a couple of servers to play with.
This Vagrantfile sets up three servers and a master, and installs the master
with Ansible and all proper networking config stuff.

## Usage

    vagrant up
    # The master server is called 'ansible'
    vagrant ssh ansible
    ansible all -m ping

This will ping all known hosts.
Since it's the first time connecting to each host with ssh,
you'll have to manually accept each host's key for it to be
saved in `known_hosts`.

Now that you're logged in, continue onto 
[Getting Started](http://www.ansibleworks.com/docs/intro_getting_started.html)!
