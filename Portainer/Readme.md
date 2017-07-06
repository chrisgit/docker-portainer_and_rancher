Portainer
=========

This demo uses runs Portainer, Wordpress and Mariadb in containers.

## Requirements
* VirtualBox
* Vagrant

## Usage
From a command shell type 

```
vagrant up
```

Vagrant will download an Ubuntu box file, unwrap it, start a VM with VirtualBox and start mariadb and wordpress containers.

You can access Portainer from your host using http://127.0.0.1:9000
Select manage the instance where Docker is running and click connect.

You can access Wordpress from your host using http://127.0.0.1:9001
On the home screen select your language and complete the setup Wizard.
