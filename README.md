# WAS-Scripts

To create an Ansible script that retrieves the WebSphere Application Server (WAS) version from five remote Linux servers, you need to use an Ansible playbook. Below is an example playbook that connects to the remote servers, checks the WAS version, and prints the results.

Prerequisites
Ensure you have Ansible installed on your control machine.
Ensure SSH access to the remote servers from the control machine.
Ensure the wasadmin user (or the user used for WebSphere management) has the necessary permissions to read the WAS version.

Ansible Inventory File
Create an inventory file (e.g., inventory.ini) listing the IP addresses or hostnames of your remote servers:

[was_servers]
server1 ansible_host=192.168.1.101 ansible_user=your_ssh_user
server2 ansible_host=192.168.1.102 ansible_user=your_ssh_user
server3 ansible_host=192.168.1.103 ansible_user=your_ssh_user
server4 ansible_host=192.168.1.104 ansible_user=your_ssh_user
server5 ansible_host=192.168.1.105 ansible_user=your_ssh_user


Ansible Playbook
Create an Ansible playbook (e.g., get_was_version.yml) to retrieve the WebSphere Application Server version:

---
- name: Get WebSphere Application Server version
  hosts: was_servers
  become: yes
  tasks:
    - name: Check WebSphere version
      shell: |
        su - wasadmin -c '/opt/IBM/WebSphere/AppServer/bin/versionInfo.sh | grep "Version"'
      register: was_version_output

    - name: Print WebSphere version
      debug:
        msg: "WebSphere Application Server version on {{ inventory_hostname }}: {{ was_version_output.stdout }}"


Explanation
Inventory File (inventory.ini):

Lists the remote servers under the group was_servers.
Specifies the SSH user for Ansible to connect to each server.
Playbook (get_was_version.yml):

Targets the was_servers group defined in the inventory file.
Uses become: yes to gain elevated privileges on the remote servers (assumes the SSH user can sudo without a password, or modify as needed).
Executes the versionInfo.sh script as the wasadmin user to get the WebSphere version.
Captures the output and registers it in the was_version_output variable.
Prints the WebSphere version using the debug module.
Running the Playbook
Execute the playbook using the following command:
ansible-playbook -i inventory.ini get_was_version.yml

Note
Adjust the path to versionInfo.sh and the wasadmin user as per your WebSphere installation and environment configuration.
Ensure that the user running the playbook has the necessary SSH keys set up for passwordless authentication to the remote servers.
This playbook will connect to each server listed in the inventory file, execute the versionInfo.sh script to get the WebSphere version, and print the version information for each server.


