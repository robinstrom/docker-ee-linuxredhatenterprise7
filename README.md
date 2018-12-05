# docker-ee-linuxredhatenterprise7
A quick walkthrough on how to setup Docker EE on a Linux Red Hat Enterprise 7 server.

Installing Docker EE on Red Hat Enterprise 7 server

Get your Docker EE repository URL:
Go to https://store.docker.com/my-content and click the Setup button for Docker Enterprise Edition for Red Hat Enterprise Linux. There you copy the URL to download you edition.

Create yum-repository:

Store a temporary environment variable with the URL that you copied from before:

	$ export DOCKERURL="<DOCKER-EE-URL>"
  
Take the value of the DOCKERURL variable and store it in a yum variable in /etc/yum/vars:

	$ sudo -E sh -c 'echo "$DOCKERURL/rhel" > /etc/yum/vars/dockerurl'

Store your OS version strin in /etc/yum/vars/dockerosversion:

	$ sudo sh -c 'echo "7" > /etc/yum/vars/dockerosversion'

Install required packages:

	$ sudo yum install -y yum-utils \
	  device-mapper-persistent-data \
	  lvm2

To ensure access to the container-selinux package, you have to enable extras:

	$ sudo yum-configer-manager –enable rhel-7-server-extras-rpms

Add Docker EE stable repository

	$ sudo -E yum-config-manager \
	  --add-repo \
	  "$DOCKERURL/rhel/docker-ee.repo"

Installing from the Repository:

	$ sudo yum -y install docker-ee

Start Docker:

	$ sudo systemctl start docker

By running the hello-world image we can verify that the Docker EE installation is done correctly:

	$ sudo docker run hello-world

To be able to run docker commands without having to write sudo before every command, simply create a docker group and add your user:

Creating the group:

	$ sudo groupadd docker

Adding your user:

	$ sudo usermod -aG docker $USER

You have to log out and back in for your membership to be re-evaluated. On virtual machine you might have to restart, and in that case you could just type:

	$ reboot

Verify that you now can run docker commands without using sudo by running the hello-world image again:

	$ docker run hello-world

