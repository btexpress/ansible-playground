## Note 2016-09-08-2013:

### This is NOW a fully functional, ready to go ansible playground with vagrant AND root ssh keys already installed.



## Ansible Playground

The [documentation](http://docs.ansible.com/ansible/index.html) for Ansible assumes you have a couple of servers to play with.
This Vagrantfile sets up four servers and a master (control machine), and installs the master
with Ansible and all proper networking config stuff.

## Usage

    vagrant up
    # The master server is called 'control'
    vagrant ssh control
    ansible all -m ping

This will ping all known hosts.
Since it's the first time connecting to each host with ssh,
you'll have to manually accept each host's key for it to be
saved in `known_hosts`.

Now try making each host say "hello"

    ansible all -a "/bin/echo hello"

    # output:
    # app01 | success | rc=0 >>
    # hello
    #
    # app02 | success | rc=0 >>
    # hello
    #
    # lb01 | success | rc=0 >>
    # hello
    #
    # db01 | success | rc=0 >>
    # hello




Now that you're all set up, you're ready to [hit the docs](http://docs.ansible.com/ansible/index.html)!
