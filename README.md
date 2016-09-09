## Note 2016-09-08-2013:

### This is NOW a fully functional, ready to go ansible playground with vagrant AND root ssh keys already installed.



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

Now try making each host say "hello"

    ansible all -a "/bin/echo hello"

    # output:
    # three | success | rc=0 >>
    # hello
    # 
    # two | success | rc=0 >>
    # hello
    # 
    # one | success | rc=0 >>
    # hello




Now that you're all set up, you're ready to [hit the docs](http://www.ansibleworks.com/docs/)!
