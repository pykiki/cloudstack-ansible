# Ansible CloudStack Playbook


## Summary

The aim of these roles are to build a single host which is ready for a zone to be created and for hosts to be added. The UI will be running and the templates for the System VMs ready to go.

The included roles will build the following elements on a single host:

1. A CloudStack Management Server
2. MySQL Server
3. An NFS server
4. KVM, XenServer, ESXi System VM templates installed
5. Cloudmonkey CLI

By default this will install CloudStack, MySQL & NFS services on the same host as you install Ansible on.


## Prerequisites

### Hardware

- The target host can be physical or virtual. 
- 2 CPUs/vCPUs are recommended 
- 2GB RAM or greater is required.
- 100GB disk minimum is recommended as this host will provide primary and secondary storage.
- As this host is acting as an NFS server, high performance is required from the underlying disks.
- 1Gb or better network connectivity


### Software

These plays are (currently) written for CentOS 7 and are tested against Ansible v2.6.1 and Cloudstack 4.11

In latest versions of Ansible, sshpass & "host_key_checking = False" are required to enable the use of a password
to login to a host rather than using SSH keys.

To install Ansible, sshpass and disable the host key checking; run:
 
```
# yum install -y epel-release
# yum install -y ansible sshpass
# sed  -i "/^#host_key_checking/ c\host_key_checking = False" /etc/ansible/ansible.cfg
```

## Running the Play

To install CloudStack, ```cd``` to where you have placed the plays and run:

```
ansible-playbook deploy-cloudstack.yml -i hosts -k
```
You will be prompted for the root password of the target host.

## Cleaning if errors occur

To cleanup CloudStack after a failed attempt, ```cd``` to where you have placed the plays and run:

```
ansible-playbook clean-cloudstack.yml -i hosts -k
```
You will be prompted for the root password of the target host.

Note, it is highly recommended to run the clean after every failed attempt, unless you know what you are doing.
Not doing the clean after an error, could prevent Cloudstack to install properly.

## Changing Default Behaviour

By default this will install CloudStack on the same host as Ansible is installed on.  To 'point' the installation at an alternate host edit
the hosts file. Change 127.0.0.1 to the IP address or resolveable FQDN of the host you wish to use.

All variables are held in deploy-cloudstack.yml. To change the installed version and/or systemvm template sources edit deploy-cloudstack.yml

## After running the play

After you run the play, you will be able to use the IP your cloudstack host and port 8080 in your browser.  (http://<cloudserver-ip>:8080)
Login with `admin` for user and `password` for password.  Follow the prompts.  Note, it will take awhile to add your host
when you are launching.  This is expected.  

After you are complete, you should have a fully working Cloudstack 1 host install!

## TODOs

- Add Ubuntu based support
