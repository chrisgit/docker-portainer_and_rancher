Portainer and Rancher
=====================

Putting Docker into production has been a really interesting exercise, we looked at some orchestration tools but decided to pick a solution native to our cloud provider. Whilst virtually everything is automated I can't help thinking we could have saved a LOT of time if there was a proper console to control, scale, monitor and capture the logs from containers. Sure using the console is likely to be a manual task but it would make the whole process of pushing out new containers simpler.

With that in mind I have played with Rancher and Portainer.

This repo has vagrantfiles for both.

Portainer is simple to use and leverages a single host or swarm mode hosts.

Rancher requires a master server and hosts running an agent.

I like both products but while using Rancher I also started to use the excellent RancherOS (I ran this in VirtualBox).

RancherOS is a base operating system that literally has to run one thing, Docker. All system services run in containers, the concept and ease use for RancherOS is brilliant, I love it!

Thank you to the teams maintaining Portainer, Rancher and RancherOS.

If you want more details they are here

 Product       | Site                         | Github
---------------|------------------------------|------------------------------------------------
Portainer      | https://portainer.io/        | https://github.com/portainer/portainer
Rancher        | http://rancher.com/          | https://github.com/rancher/rancher
RancherOS      | http://rancher.com/docs/os/  | https://github.com/rancher/os

# Requirements

To see these demonstrations you will need
* VirtualBox which can be downloaded from here: https://www.virtualbox.org/wiki/Downloads
* Vagrant which can be downloaded from here: https://www.vagrantup.com/downloads.html

Tested with VirtualBox 5.1.22
Tested with Vagrant 1.9.5

Contributing
------------
Please see [CONTRIBUTING.md][contributor] and read the [CODE_OF_CONDUCT.md][conduct]

License and Authors
-------------------
Authors: Chris Sullivan

[contributor]: https://github.com/chrisgit/docker-portainer_and_rancher/blob/master/CONTRIBUTING.md
[conduct]: https://github.com/chrisgit/docker-portainer_and_rancher/blob/master/CODE_OF_CONDUCT.md
