### Prerequisites
1. Jenkins Master Running 
1. EC2 RHEL 7.x Instance - _for Slave Node
   - With Internet Access
   - Security Group with Port `8080` open for internet
   - Java v1.8.x 

## Install Java
We will be using open java for our demo, Get latest version from http://openjdk.java.net/install/. Also configure the default `JAVA_HOME` path
```sh
yum install java-1.8*
#yum -y install java-1.8.0-openjdk
```
## Setup Jenkins Slave
```sh
# Create user and add the user to wheel group
useradd jenkins-01
# Create SSH Keys
sudo su - jenkins-01
ssh-keygen -t rsa -N "" -f /home/jenkins-01/.ssh/id_rsa
# The private and public keys will be created at these locations `/home/jenkins-01/.ssh/id_rsa` and `/home/jenkins-01/.ssh/id_rsa.pub`
cd .ssh
cat id_rsa.pub > authorized_keys
chmod 700 authorized_keys
```

### Configuration on Master
Copy the slave node's public key[`id_rsa.pub`] to Master Node's `known_hosts` file
```sh
mkdir -p /var/lib/jenkins/.ssh
cd /var/lib/jenkins/.ssh
ssh-keyscan -H SLAVE-NODE-IP-OR-HOSTNAME >>/var/lib/jenkins/.ssh/known_hosts
# ssh-keyscan -H 172.31.38.42 >>/var/lib/jenkins/.ssh/known_hosts
chown jenkins:jenkins known_hosts
chmod 700 known_hosts
```

#### Configure the Slave using `Manage Jenkins`
Configure the node as shown here
`Manage Jenkins` > `Manage Nodes` > `New Node`
### Test Jenkins Jobs
1. Create “new item”
1. Enter an item name – `My-First-Project`
   - Chose `Freestyle` project
1. Under `General` Section
   - Choose `Restrict where this project can be run`
     - Update your _jenkins slave label_ `Java` 
1. Under Build section
   Execute shell
  echo "Hello world"
1. Save your job 
1. Build job
1. Check "console output"
