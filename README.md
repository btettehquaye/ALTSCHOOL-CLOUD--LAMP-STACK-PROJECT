# ALTSCHOOL-CLOUD--LAMP-STACK-PROJECT
## Develop a bash script to orchestrate the automated deployment of two Vagrant-based Ubuntu systems, designated as 'Master' and 'Slave', with an integrated LAMP stack on both systems.


What is a script?

A script is text document that contains instructions or commands that will then be executed by an intelligent device. These instructions will be written in some programming language in which their syntax must be respected so that each instruction can be translated into machine language.

This script will helps install a master and slave ubuntu vagrant machine and runs LAMP stands for: Linux (OS/Kernel), Apache (Web Server), MySQL (Database), PHP (Scripting Language). It is an open-source Web development environment which lets you create web applications. It is generally also referred as Web Stack.

Prerequisite

Vagrant – which is an open source tool for building and managing virtual machine environments.

Virtual box is a software-based emulation of a physical computer. It is created using software on one physical computer to emulate the functionality of another separate physical computer , has a CPU, memory, disks to store files, and can connect to the internet if needed.

What is a Master Machine?

A master machine in the context of Linux system administration refers to a computer that is used to manage and control other computers, devices, processes also known as the slaves. The master machine serves as a communication hub and manages the slaves

What is a slave Machine?

A slave is a computer or peripheral device that operates under the control of another computer peripheral known as the Master.

### Process in deployment of two Vagrant-based Ubuntu systems, designated as 'Master' and 'Slave', with an integrated LAMP stack on both systems.

1. First Create a folder where you want to put your vagrant-based Ubuntu system in either your VS Code or Gitbash by using the command below
   
 * mkdir vagrant && cd vagrant && mkdir Master && cd Master      to create a folder
 * Configure the file by issuing the command VAGRANT INIT, which  is used to initialize the current directory to be a Vagrant environment by creating an initial Vagrantfile if one does not already exist and configuration file for your Virtual Machine.
 * Nano the vagrantfile to open the file and write your code.
  
2. Customize the amount of memory on the Master and slave  Machines to "1024" and also limit the number of central processing unit "cpus" to 2.

3. Specification of both Master and Slave machine must be as follows,
   
* Host name = Master 
* Host name = Slave
* image = ubuntu/focal64.
* network: private_network. 
* constant ip: "192.168.20.10" with the ip of the the slave being "192.168.20.11".

4. Configuration of Master VM and slave VM is as follows,
* In nano issue the following commands
* config.vm.define "Master" do |subconfig|
* subconfig.vm.box = "ubuntu/focal64"
* subconfig.vm.hostname = "Master"
* subconfig.vm.network :private_network, type: "192.168.20.10"
* end
* In the case of the Slave Machine the config.vm.define will hange to "Slave" do |subconfig| with the network or ip changing to "192.168.20.11"

when that is done you now provision the Master VM and Slave VM to install and update network tool such as daemon by issuing these commands
* config.vm.provision "shell", inline: <<-SHELL
* sudo apt-get update && sudo apt-get upgrade -y
* sudo apt-get install -y avahi-daemon libnss-mdns
* sudo apt install sshpass -y
* sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
* sudo systemctl restart sshd
* SHELL
* end

Installing 
- sshpass: Ensures secure communication between the server and the client, monitors data encryption/decryption, and protects the integrity of the connection. It also performs data caching and compression. allows you to chip in the password while logging in
- Conducts the client authentication procedure
- Manages communication channels after the authentication by switching the password authenticator on to loggin using password
- restarting the sshd service
- installing avahi-daemon libnss-mdns

5. Creating User named Altschool and granting altschool user root (superuser) privilegeby running the follwing commands
* sudo useradd -m -G sudo altschool
* sudo usermod -aG root altschool
* sudo useradd -ou 0 -g 0 altschool
* sudo -u altschool ssh-keygen -t rsa -b 4096 -f /home/altschool/.ssh/id_rsa -N "" -y
* sudo cp /home/altschool/.ssh/id_rsa.pub altschoolkey
* sudo ssh-keygen -t rsa -b 4096 -f /home/vagrant/.ssh/id_rsa -N ""
* sudo cat /home/vagrant/.ssh/id_rsa.pub | sshpass -p "vagrant" ssh -o StrictHostKeyChecking=no vagrant@192.168.20.11 'mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys'
* sudo cat ~/altschoolkey | sshpass -p "vagrant" ssh vagrant@192.168.20.11 'mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys'
*  sshpass -p "james" sudo -u altschool mkdir -p /mnt/altschool/slave
*  sshpass -p "james" sudo -u altschool scp -r /mnt/* vagrant@192.168.20.11:/home/vagrant/mnt
*  sudo ps aux > /home/vagrant/running_processes
    exit
6. As already stated in the introduction, LAMP stands for: Linux (OS/Kernel), Apache (Web Server), MySQL (Database), PHP (Scripting Language). It is an open-source Web development environment which lets you create web applications. It is generally also referred as Web Stack.

Install LAMP by running the following commands

Installing Apache

* sudo apt-get -y install apache2

Installing MySQL and it's dependencies, Also, setting up root password for MySQL as it will prompt to enter the password during installation

* sudo debconf-set-selections <<< 'mysql-server-5.5 mysql-server/root_password password rootpass'
* sudo debconf-set-selections <<< 'mysql-server-5.5 mysql-server/root_password_again password rootpass'
* sudo apt-get -y install mysql-server libapache2-mod-auth-mysql php5-mysql

Installing PHP 
* sudo apt-get -y install php5 libapache2-mod-php5 php5-mcrypt

When this is sone LAMP wil be installed on both Master and Slave Machine once the script is run.

7. Finally manually installing lamp on all servers can be repetitive and time consuming. Therefore, it is advisable to have written a script that installs LAMP which configures the necessary permissions and then, starts the web server.

