Deploy WordPress on a Debian 9 server
========
### Introduction

Let's say, we have a fresh and minimial installed debian 9 server. The first steps are:

* ssh to your server as root and change the initial password with:

`` `passwd` ``

* update repositories and system

`` `apt update && apt upgrade` ``

* i'm using vim, therefore i have to install it first to be able editing files in a nice way.

`` `apt install vim` ``

* It's not easy to say if you need beside fail2ban a firewall as well. In my case, the firewall ufw had already failed when I installed a tool that was accessible via a port, although not released in the firewall-configuration. But it feels nicer if i have both.

`` `apt install ufw fail2ban` ``

* nice & short instructions will be available in a short time  

* update /etc/hosts that your host can be resolved. Example:

```
127.0.0.1	localhost
127.0.1.1	example.org
# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
12.134.56.789	example.org

```

* Create a new user, install sudo, assign groups like sudo, ssh to your user. Modify sudoers-file for the new user.

`` `apt install sudo apt-transport-https` ``
`` `adduser newusername` ``
`` `adduser newusername sudo` ``
`` `adduser newusername ssh` ``
`` `vi /etc/sudoers` ``

* Next step is to login via ssh with your new user. Create ssh-key:
`` `vi /etc/sudoers` ``
```ssh-keygen -t rsa -b 4096 -C your-username` ``

* from source to target server
`` `ssh-copy-id -i ~/.ssh/id_rsa.pub user@target-server` ``

###### Open Todo for Ansible, until its done, please do this steps manually on the target-server:
* Install the SURY Repository Signing Key
`` `apt install gnupg2 -y` ``
`` `wget -qO - https://packages.sury.org/php/apt.gpg | sudo apt-key add -` ``

* Install the SURY Repository and update your system
`` `echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/php7.x.list` ``
`` `apt update && apt upgrade` ``

* Check the PHP 7.4 is available for installation.
`` `apt-cache policy php7.4` ``

* Install apache2
`` ` sudo apt-get install apache2 apache2-util mailutils build-essential` ``

* nice & short instructions to install and configure apache2 will be available in a short time  

#### How to use this playbook ####

* Modify variables with your own given credentials, etc
    * roles/mysql/vars/main.yml
    * roles/wordpress/vars/main.yml
    * hosts

###### Start Deployment ######

<code>ansible-playbook playbook.yml -i hosts -u <target-host-user> -K</code>

##### Help #####
* If it's not working as expected, debug mode is automatically activated.
* Logfile is here: /var/www/WPInstallDir/wp-content/debug.log
