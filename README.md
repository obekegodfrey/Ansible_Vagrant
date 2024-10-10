# Ansible Setup Guide with Vagrant

For this project, I needed Linux machines created by Vagrant. The following are the steps:

1. How to set up an Ansible Controller Machine.
2. How to set up Ansible host machines.
3. Making a connection between the controller and hosts.
4. Adding host and playbook file on the controller.
5. Running a playbook to configure host machines.

## 1. How to Set Up an Ansible Controller Machine

### Step 1
Install VirtualBox and Vagrant on your local machine.

### Step 2
Open terminal and navigate to the directory where you want to set up your Ansible project.

### Step 3
Create a new directory for your Ansible controller VM by running the command:
mkdir ansible-controller

###Step 4
Navigate to the directory and create a new file called Vagrantfile by running the command:
vagrant init

### Step 5
Edit the Vagrantfile and add the following lines to the end of the file to provision Ansible on the VM:

Vagrant.configure("2") do |config|
  
  # Define the Host machine as "ansible-controller"
  config.vm.define "ansible-controller" do |controller|
    controller.vm.hostname = "controller"
  end
  
  # Configure the Linux centos/7 to the ansible-controller machine
  config.vm.box = "centos/7"
  
  # Provision Ansible by installing it on centos/7
  config.vm.provision "shell", inline: <<-SHELL
    sudo yum install epel-release -y
    sudo yum install ansible -y
  SHELL
end

### Step 6
Save and check if it’s a valid Vagrantfile by running:
vagrant validate

Then run the command:vagrant up to start the VM.

###Step 7
Once the VM is up and running, connect to it using SSH by running the command:
vagrant ssh
Check if Ansible is installed by running:
ansible --version

### Step 8
Create a new directory for the Ansible project on the controller VM by running:
mkdir ansible-project

### Step 9
Navigate to the ansible-project directory and create a new file called hosts by running:
touch hosts

### Step 10
Create a new file called playbook.yml. This file will contain the tasks you want to perform on your managed hosts.

### 2. How to Set Up Ansible Host Machines
##Step 1
In the terminal, navigate to your Ansible project folder.

##Step 2
Create a new directory for your host machines by running the command:
mkdir host-machines

##Step 3
Navigate to the host-machines directory and create a new Vagrantfile by running the command:
vagrant init

##Step 4
Edit the Vagrantfile and modify the following lines to set up two Vagrant machines:

Vagrant.configure("2") do |config|

  # Configure centos/7 operating system
  config.vm.box = "centos/7"

  # Configure a web application 
  config.vm.define "web" do |web|
    web.vm.hostname = "web"
    web.vm.network "private_network", ip: "192.168.33.10"
  end

  # Configure a database application
  config.vm.define "db" do |db|
    db.vm.hostname = "db"
    db.vm.network "private_network", ip: "192.168.33.11"
  end

  config.vm.network "forwarded_port", guest: 80, host: 8080, auto_correct: true
  config.vm.usable_port_range = (8000..9000)

end

## Step 5
Save and check if it’s a valid Vagrantfile by running:
vagrant validate
Then run the command:
vagrant up
to start the VM.

## Step 6
Check the status of the machines by running:
vagrant status

Once the VMs are up, connect to them using the command:
vagrant ssh <machine-name>

For example, you can connect to the web or db machine using:
vagrant ssh web
or
vagrant ssh db
