# Zabbix Agent Installation Ansible Role

## Overview
This document provides a step-by-step guide to install the Zabbix agent using an Ansible role. The setup involves creating EC2 instances using a Terraform module, configuring the dynamic inventory with Ansible, and applying the Zabbix agent configuration.

## Prerequisites
1. Terraform
2. Ansible
3. AWS CLI configured with the necessary permissions

## Steps

### 1. Create EC2 Instances using Terraform

First, create two EC2 instances using the Terraform EC2 module available on GitHub. This module can be used for day-to-day instance creation for testing purposes.

GitHub URL: [Terraform EC2 Module](https://github.com/Parasharam-DevOps/Terraform-Ec2-Module)

In the Terraform code, add a group tag for your machines. This tag will be used in the Ansible dynamic inventory.

#### Run Terraform Apply

```bash
terraform apply --auto-approve
```

![image](https://github.com/user-attachments/assets/f7e2b679-0749-4d52-9b83-b47eeb641e7d)

2. Output Public IPs

Use the output.tf file to print the public IPs of the servers.

![image](https://github.com/user-attachments/assets/6ad07a4a-9e18-4236-a4b0-755d7c7c20d6)

3. Verify Ansible Inventory

Use the Ansible dynamic inventory plugin and assign the tag name for Zabbix installation. Verify the inventory using the following command:

```bash
ansible-inventory --graph
```
![image](https://github.com/user-attachments/assets/adb2d6a2-81cc-4301-b492-6669796aa688)

4. Configure Ansible Playbook

Copy the group tag and paste it into the Ansible playbook located at /zabbix-role/tests/test.yml.

![image](https://github.com/user-attachments/assets/c732087c-7311-4059-b92b-fab235b93e14)

5. Ansible Role Directory Structure

Ensure your Ansible role directory structure is properly set up.

![image](https://github.com/user-attachments/assets/a92b21d1-3cbf-44a8-b85b-b5494068aef1)

6. Run the Ansible Playbook

Execute the Ansible playbook to install the Zabbix agent.

```bash
ansible-playbook zabbix-role/tests/test.yml
```

![image](https://github.com/user-attachments/assets/ec0daad9-ab16-4ab6-882e-34eab274bbde)

![image](https://github.com/user-attachments/assets/812fbb89-87d3-4db7-8d39-353a1df591d6)

# 7. Verify Zabbix Agent Status
Check the status of the Zabbix agent on the server to ensure it is running correctly.

![image](https://github.com/user-attachments/assets/a05e72e7-3eb7-466b-b0b5-370555f64d4d)


# Troubleshooting 

For User & Password Based Authentication

# AWS Cloud Machine SSH Configuration 

This documentation provides steps to change SSH configuration to enable password authentication for a new user on an AWS Cloud machine.

## Steps to Enable Password Authentication

1. **Edit the SSH configuration file to enable password authentication:**

   Open the `50-cloud-init.conf` file in the `sshd_config.d` directory:

   ```bash
   sudo vim /etc/ssh/sshd_config.d/50-cloud-init.conf
   ```

   Change the `PasswordAuthentication` line from `no` to `yes`:

   ```text
   PasswordAuthentication yes
   ```

   ![Edit PasswordAuthentication](https://github.com/user-attachments/assets/bfb002e7-8600-48a6-bef7-31b5854a5f3c)

2. **Ensure `ChallengeResponseAuthentication` is set to `no`:**

   Open the `50-redhat.conf` file (if it exists) or the `50-cloud-init.conf` file again:

   ```bash
   sudo vim /etc/ssh/sshd_config.d/50-redhat.conf
   ```

   Ensure the `ChallengeResponseAuthentication` line is set to `no`:

   ```text
   ChallengeResponseAuthentication no
   ```

   ![Edit ChallengeResponseAuthentication](https://github.com/user-attachments/assets/4b6f743c-db90-4fe9-858d-bbee83d4854c)

3. **Restart the SSH service:**

   After making the changes, restart the SSH service to apply the new settings:

   ```bash
   sudo systemctl restart sshd
   ```
## Disable Host Key Checking & Configure Ansible Inventory Plugins

1. **Disable Host Key Checking:**
   - To disable host key checking, modify the Ansible configuration. Ensure that `host_key_checking` is set to `False` in your Ansible configuration file (`ansible.cfg`).

   ![Disable Host Key Checking](https://github.com/user-attachments/assets/56c1a6f4-8a0b-4ed9-aeac-77cd939642ad)

2. **Enable Inventory Plugins:**
   - In your Ansible configuration file (`ansible.cfg`), add or modify the `enable_plugins` section to include the necessary plugins:
     ```ini
     [defaults]
     enable_plugins = aws_ec2, yaml, ini
     ```

By following these steps, you will enable password authentication for SSH on your AWS Cloud machine and ensure that `ChallengeResponseAuthentication` is disabled.

# Example of Hosts Files
**Username and Password Authentication**

Create a hosts file for Ansible with the following content:

```bash
[all]
server1 ansible_host=65.2.169.187 ansible_ssh_user=ninja ansible_ssh_pass=admin@123
```
**Use the hosts file when running the Ansible playbook:**

```bash
ansible-playbook -i zabbix-role/tests/hosts zabbix-role/tests/test.yml --vault-password-file vault_password.txt --extra-vars "ansible_become_pass=admin@123"
```

**Public Key Authentication**

```bash
server1 ansible_host=35.166.148.190 ansible_user=ubuntu ansible_ssh_private_key_file=/home/parsu/Downloads/veeam.pem
```
Use the hosts file when running the Ansible playbook:
```bash
ansible-playbook -i zabbix-role/tests/test.yml
```

## Reference Docs

| Document                                                                                                  | Link                                                                                                                                   |
|-----------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| How to Use Ansible - Cheat Sheet Guide                                                                    | [How to Use Ansible - Cheat Sheet Guide](https://www.digitalocean.com/community/cheatsheets/how-to-use-ansible-cheat-sheet-guide)       |
| Use Ansible Vault in Playbooks to Protect Sensitive Data                                                  | [Use Ansible Vault in Playbooks to Protect Sensitive Data](https://www.tecmint.com/use-ansible-vault-in-playbooks-to-protect-sensitive-data) |
| Zabbix Official Repository |[Link](https://repo.zabbix.com/zabbix/)
