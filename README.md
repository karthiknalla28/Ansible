# Ansible-Commands
We'll install Ansible on a control node and configure two managed servers for use with Ansible. We will also create a simple inventory and run an Ansible command to verify our configuration is correct.
## Install Ansible on the Control Node
- ssh into a remote server
```bash
ssh user_name@public_ip_address
```
- Installing Ansible on the control node
```bash
sudo yum install ansible
```
## Configure the ```ansible``` user on the control node
```bash
# switching user to the ansible
su - ansible
```
- Create a key pair for the ```ansible``` user on the control host
```bash
ssh keygen
```
- Copy the public key generated above to both node1 and node2
- Copying the generated public key from the host node (control machine) to other nodes (e.g., node1, node2)   is essential for passwordless SSH authentication. This setup allows the host machine to connect to the      nodes without manually entering a password each time.
```bash
ssh-copy-id node1
ssh-copy-id node2
```
## Create a Simple Ansible Inventory
- Creating a simple Ansible inventory on the control node in ```/home/ansible/inventory``` containing ```node1``` and ```node2```
- On the control host:
```bash
su - ansible
touch /home/ansible/inventory
```
- adding ```node1``` and ```node2``` to the inventory file located at the ```/home/ansible/inventory```
```bash
echo "node1" >> /home/ansible/inventory
echo "node2" >> /home/ansible/inventory
```
## Configure ```sudo``` access for Ansible
- we'll configure sudo access for Ansible on ```node1``` and ```node2``` such that Ansible may use sudo for any command with no password prompt.
- Log in to ```node1``` as ```cloud_user``` and edit the ```sudoers``` file to contain appropriate access for the ansible user:
```bash
ssh cloud_user@node1
# editing the sudoers file
sudo visudo
```
### Explanation of the below code changes in the sudoers file
- ansible → The username to which this rule applies.
- ALL=(ALL) → The user can run commands as any user or group.
- NOPASSWD: ALL → No password is required when using sudo
```bash
ansible    ALL=(ALL)       NOPASSWD: ALL
```
- Then enter ```logout``` for exiting from node1 and also repeat the same for the ```node2```. Then back out to the control node.
## Verify Each Managed Node Is Accessible
- To verify each node, run the following as the ```ansible``` user from the control host:
 ```bash
ansible -i /home/ansible/inventory node1 -m ping
ansible -i /home/ansible/inventory node2 -m ping
```
