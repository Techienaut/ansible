# About

This is a Cisco lab, using [GNS3](https://www.gns3.com/), Ansible, Virtualbox, and [Vagrant](https://www.vagrantup.com/).

Currently, it is able to run an Ubuntu server and install ansible on said server. From there, it is able to administer router **R1** and send individual cisco IOS commands via ansible--In this case `show version`.

# Setup

1. To setup, please install Ansible, GNS3, Vagrant, and Virtualbox on a Linux machine (preferably). In GNS3, import the C7200 Router and C3600 L3 Switch into GNS3 [download](<http://srijit.com/working-cisco-ios-gns3/>).
2. Add a router and change username to R1 and domain to geek.com.
3. Add ssh to router, including generating RSA keys
4. Add ip address 10.0.0.1 255.255.255.0 to Fa0/0
5. On the host machine, change directory to 02_cisco_ios/ and type `vagrant up`. This will create an ubuntu VM in Virtualbox, as well as provision it with `cisco_terminal.yml` via Ansible . Type `vagrant halt` after the provisioning is done.
6. On GNS3, add `02_cisco_ios` as a template. Connect the vm with a link from `Fa0/0` on R1 to `ether 1` on the VM.
7. Under `02_cisco_ios/`, type `vagrant provision`. This will set an ip address `10.0.0.3` to the machine on `ether 1`.
8. Under `02_cisco_ios/`, type `vagrant ssh` .
9. While your ssh into the VM, cd to `/media/sf_vagrant`  and type `ANSIBLE_HOST_KEY_CHECKING=false ansible-playbook -i hosts R1.yml`. This will issue the command `show version` on `R1`.