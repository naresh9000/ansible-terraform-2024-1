# Ansible-terraform-aws
->Using the packer Create the AMI in aws.Below is the URL for creating the AMI.
Gitub::https://github.com/naresh9000/packer-terraform.git
->launch the EC-2 medium/large ec2 instance for the ansible controller.
Note::to know the distribuition release
        cat /etc/lsb-release
->Login through "Ansibleadmin" user in putty terminal.

Follow the below steps for installing the ansible
```
$ sudo apt update
$ sudo apt install software-properties-common
$ sudo add-apt-repository --yes --update ppa:ansible/ansible
$ sudo apt install ansible -y

```

->sudo su -
->cd /etc/ansible
->ansible-config init --disabled -t all > ansible.cfg
->nano ansible.cfg , make the "host_key_checking=false"
we are doing this for ,Ansible should not verify host keys when connecting to remote hosts over SSH.
i.e for our lab or test environments.

<https://github.com/naresh9000/ansible-terraform-2024-1.git>
In the same project i have embed the ansible inventory file creation also...
->create the ansible template file(ansible_details.tpl) file in order to create the "inventory file" using the resource "local_file" where we will use the "templatefile" function to create the targeted inv-file having all the data of the targted machines(public-ip,private-ips).

**Inventory file**
Inside the inventory file ..
For ex:
```
testserver01 ansible_port=22 ansible_host=${testserver01} ansible_user=ansibleadmin 
```
**Let me break down for you:**

**testserver01**: This is the name of the host as recognized by Ansible. You will use this name in your Ansible playbooks and tasks to refer to this specific server.

**ansible_port=22:** This specifies the SSH port that Ansible should use when connecting to the host. In this case, it's set to the default SSH port, which is 22. If your SSH service is running on a different port, you can specify it here.

ansible_host=${testserver01}: This is the IP address or hostname of the remote host. ${testserver01} is a placeholder, and it's expected to be replaced with the actual IP address or hostname of your target server. For example, if testserver01 has an IP address of 192.168.1.101, it should be written as ansible_host=192.168.1.101.

**ansible_user=ansibleadmin:** This specifies the SSH username that Ansible should use when connecting to the host. In this case, it's set to ansibleadmin. When Ansible establishes an SSH connection to testserver01, it will use the username ansibleadmin. Ensure that you have the necessary SSH access and permissions for the specified user on the target server.
--usually we create the common_user in all the targeted machines in order ansible will talk to that user and do any actions/configuration-setup/install-agents/ etc.So instead we use ansible_user variable with ease..

**Next**
->Log in to ansibleadmin user from ansible-controller
->ssh-keygen
->Create the Github repo and push the code which consist of the inventory file too...
        in github click on the deploy the keys(if its private repository) within that repo and paste the .pub key value generated for authentication.
        Github-URL::https://github.com/naresh9000/ansible-terraform-2024-1.git

Note::
--ssh-agent is a program that runs in the background and manages the authentication process for SSH private keys. When you use ssh-agent along with Ansible or any other SSH-based tools, it provides a way to securely store and use private keys without entering the passphrase each time you connect to a remote server.

--Once your key is added, ssh-agent holds it in memory, and you can connect to remote servers without providing the passphrase each time.


```set up SSH agent
ssh-agent bash
ssh-add /home/ansibleadmin/.ssh/id_rsa
```
**Next**


```Install the terraform:
 cd /usr/local/bin/
 sudo apt install unzip
 sudo wget https://releases.hashicorp.com/terraform/1.6.6/terraform_1.6.6_linux_amd64.zip
 sudo unzip terraform_1.6.6_linux_amd64.zip
 sudo rm -rf terraform_1.6.6_linux_amd64.zip
 git clone <github-url>
 cd ansible-terraform-2024
 terraform init
 terraform fmt;terraform validate
 terraform apply --auto-approve

```

Using the terrafrom deploy the 3 t3.micro instances(host machines) in 3 subnets in different vpc.
And also establish the vpc-peering ,also inventory-file will also get created.Please go through above  github url.
Add the public key generated from the ansible controller in the place of USER-date in ec-2 resource...


Check the targted machines are able to ssh from the ansible-controller
**Ansible-commands**
ansible -i <inventroy-file-path> all -m ping 
ansible -i <inventroy-file-path> <group-namepvt/all/web-servers> -m ping all
