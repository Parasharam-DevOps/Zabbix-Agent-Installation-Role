# zabbix-agent-installation
Zabbix Agent Installation Ansible Role



# Troubleshooting
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

## Reference Docs

| Document                                                                                                  | Link                                                                                                                                   |
|-----------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| How to Use Ansible - Cheat Sheet Guide                                                                    | [How to Use Ansible - Cheat Sheet Guide](https://www.digitalocean.com/community/cheatsheets/how-to-use-ansible-cheat-sheet-guide)       |
| Use Ansible Vault in Playbooks to Protect Sensitive Data                                                  | [Use Ansible Vault in Playbooks to Protect Sensitive Data](https://www.tecmint.com/use-ansible-vault-in-playbooks-to-protect-sensitive-data) |
