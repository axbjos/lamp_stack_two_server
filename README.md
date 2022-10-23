# Vagrant Automation for the PHP Crude CRUD App

## Background

This Vagrantfile will create a single server running the following (LAMP Stack):

- Ubuntu 20.04 Linux
- MariaDB distribution of MySQL
- Apache 2 Webserver
- PHP Programming Language and Modules

A sample 300,000 record "Employee" database will be uploaded to the DB.  

A sample PHP application that implements all CRUD operations in very Crude fashion: aka the PHP Crude CRUD App.

Please review the Vagrantfile for specific configuration details.

The end result should be a running server with everything working.  Simply navigate to the IP address in the Vagrantfile

---

## Prerequistites

### Intel Based Windows and Macs:

- Vagrant http://vagrantup.com
- Virtualbox http://virtualbox.org

### M1/M2 aka Apple Silicon Macs:

- VMware Fusion for Apple Silicon
- Vagrant
- Vagrant VMware Utility
- Vagrant VMware Provider

Consult the Vagrant Documentation

As of Oct 18, 2022, an unlimited free version of VMware Fusion for Apple Silicon is available.

Also as of Oct 18, 2022, there are all sorts of "gotchas" running Vagrant and VMware Fusion for Apple Silicon together.

---

## Manual Install Alternative

Create your own Ubuntu 18/20/22 Virtual Machine manually using whatever hypervisor you wish.

Then just run all the shell commands shown in the Vagrant file on your VM

## Docker

Look at my phpcrudecruddocker repo for an example of how to containerize the app.

A ready to go app is also available on Dockhub.  Will add the pull link soon.
