# Puppet-configuration-and-working
Puppet configuration
----------------------------------------------------------------------------
**************Prerequisits************

Install putty from putty.org

Open AWS account
Go to EC2
Open instances (2)
provide a key in ppt ( if you save it in pem then convert it to ppk by using putty gen)
Edit network settings 
Add a new network group
Make ALL TCP in the connections 
Make ANYWHERE in the network ip

Launch the instances
Change the name to Master and slaves

Connect to master and slave separately from putty using PUBLIC IP address.
Change the font and color for master and slaves. 
**********************************************************
----------------------------------------------------------------------------
Master

Update servers 
sudo apt-get update

sudo apt-get install wget 

change the host
sudo nano /etc/hosts
<public ip of master> puppet

Download files (Debian package)
wget https://apt.puppetlabs.com/puppet-release-bionic.deb

Unzip the file
sudo dpkg -i puppet-release-bionic.deb

Install puppet master
sudo apt-get install puppet-master

Check puppet master policy
apt policy puppet-master

Check the puppet master active status
sudo systemctl status puppet-master.service
ctrl+c

Goto file to change args
sudo vim /etc/default/puppet-master

Change java args for priamry memory
Xmx specifies the maximum memory allocation pool for a Java virtual machine 
(JVM), while Xms specifies the initial memory allocation pool
JAVA_ARGS="-Xms512m -Xmx512m"

Restart Puppet master
sudo systemctl restart puppet-master.service


Open port to communicate with slave
sudo ufw allow 8140/tcp

Configuration of the master is done.
-----------------------------------------------------------------------
Slave Configuration

sudo apt-get update
sudo apt-get install wget 
sudo nano /etc/hosts
<public ip of master> puppet
wget https://apt.puppetlabs.com/puppet-release-bionic.deb
sudo dpkg -i puppet-release-bionic.deb
sudo apt-get install puppet
sudo systemctl start puppet
sudo systemctl enable puppet

Configuration of the slave is done.
------------------------------------------------------------------------
Master

Need to check the certificate request
sudo puppet cert list

If there is any certificate then sign them
sudo puppet cert sign --all


***********Create manifest********
sudo mkdir -p /etc/puppet/code/environments/production/manifests/
sudo nano /etc/puppet/code/environments/production/manifests/site.pp
file{'/tmp/puppet_test.txt':					#resource type file and filename
	ensure => present,					#if it exists
	mode => '0644',						#permissions
	content => "Working on ${ipaddress_eth0}!\n",	#Print IP add
}
***********Create manifest********

Restart puppet master
sudo systemctl restart puppet-master
---------------------------------------------------------------------------
Slave

Check if any file exist on in temp starts with i
cd /tmp
ls

Slave asked the master if there is any changes required for this server
sudo puppet agent --test

Check if any file is created
ls

Check the contents
cat puppet_test.txt

Configuration of master-slave communication is over.
----------------------------------------------------------------------------
Master

***********Create manifest********
cd /etc/puppet
ls
cd code
ls
sudo mkdir -p environments/production/manifests/
cd environments/production/manifests/

Create a file new_site1.pp
sudo nano new_site.pp


node default{			#any of the agent wanna update master can do
# 1 resource
package {'nginx':			#resource type{ 'resource name
ensure => installed,		#attribute
}

#2 resource
file{ '/tmp/status.txt':
content => 'Nginx has been installed successfully',
mode => '0644',
}
}

Save the file
ctrl+x
y
Enter

The file is saved
----------------------------------------------------------------------
Slave
sudo puppet agent --test		#apply the catelog

Check the software is installed or not?
Go to slave
Copy public ip
Open web browser
Paste the ip
Hit enter 
It should show the content.

Check from the file.
Slave
cat  /tmp/status.txt
It should show the content.
----------------------------------------------------------------------
