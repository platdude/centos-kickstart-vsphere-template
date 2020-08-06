Project Title
-------------
Minimal Centos VSphere template automation

Getting Started
---------------
These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

Prerequisites
-------------
1. Packer from Hashicorp installed on your Linux or MacOS device
2. Access to a Vcenter

Installation
------------
1. Download and install a precompiled binary for your OS from https://www.packer.io/downloads/
2. Clone this repository

You should end up with something like the below, meaning Packer has been successfully installed:

```
$ alberto-mac:vm-building alberto$ packer -h
Usage: packer [--version] [--help] <command> [<args>]   

Available commands are:
    build       build image(s) from template
    console     creates a console for testing variable interpolation
    fix         fixes templates from old versions of packer
    inspect     see components of a template
    validate    check that a template is valid
    version     Prints the Packer version
```

Running the tests
-----------------
First set environment variables on your current session for (mind the capitals):
```
export VCENTER_SERVER=<your_vcenter>
export VCENTER_USERNAME=<your_vcenter_username>
export VCENTER_PASSWORD=<your_vcenter_password>
export VCENTER_HOST=<your_vcenter_host>
export VCENTER_DATASTORE=<your_vcenter_datastore>
export VCENTER_NETWORK=<your_vcenter_network>
```

Revise centos-vsphere-base.json and adjust parameters to suit your needs: disk_size , ssh_user, etc

Adjust a few things on file anaconda-ks.cfg base on your own needs. Some guidance below:

- Insert your own cncrypted password for the bootloader
- Adjust the partitioning as per your needs
- Arrange the repos you want configured by default on your template
- Adjust the provision user's ssh key (this user should be stripped later on by your configuration management tool: Ansible, Chef...)
- Adjust the name servers you want the image to use during the post installation script

Bear in mind the Kickstart file also disables newer interface naming convention by rolling back to the more traditional ethX nomenclature.

Worth noting as well this setup does not work for Centos8, since they have disabled floppy support on it.

Deployment
----------
packer build centos-vsphere-base.json

Built With
----------
- Centos Kickstart file
- Packer vsphere-iso builder

Authors
-------
Alberto Padrones Lopez - Initial work - dnsandfunops

License
-------
This project is licensed under the MIT License - see the LICENSE.md file for details

Acknowledgments
---------------
www.ispcolohost.com for inspiration to disable interface naming convention during build
