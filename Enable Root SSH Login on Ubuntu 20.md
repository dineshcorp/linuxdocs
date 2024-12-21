# Enable Root SSH Login On Ubuntu 20

This guide explains how to enable root SSH login on Ubuntu 20 by editing the SSH configuration file, setting a root password, and restarting the SSH service.

---

## Steps to Enable Root SSH Login

### 1. Set a Root Password (if not already set)
Run the following command to set a password for the root user:

```bash
sudo passwd root
```

### 2. Edit the SSH Configuration File
Open the SSH configuration file in a text editor:

```bash
sudo vim /etc/ssh/sshd_config
```

### 3. Update the SSH Configuration

- Find the line that says `PermitRootLogin` and update it:

  ```plaintext
  PermitRootLogin yes
  ```

- Ensure the following line is also present to allow password authentication (if you plan to use passwords):

  ```plaintext
  PasswordAuthentication yes
  ```

If these lines are commented out (with `#`), remove the `#` to uncomment them.

### 4. Restart the SSH Service
After making changes, restart the SSH service to apply the new settings:

```bash
sudo systemctl restart sshd
```

### 5. Test Root SSH Login
Attempt to log in as the root user from a remote system:

```bash
ssh root@<your-server-ip>
```

---

## Security Warning

Enabling root SSH login can pose a security risk, especially if the server is exposed to the internet. To mitigate risks:

- Use **key-based authentication** instead of passwords.
- Restrict root access to specific IP addresses using the `AllowUsers` or `Match` directives in `sshd_config`.
- Enable a firewall (e.g., `ufw`) to limit SSH access.
- Consider using `sudo` instead of root for administrative tasks.
