# Manual Image Installation

This guide outlines the steps to manually install an image, configure necessary users, and set appropriate permissions. Follow the instructions carefully to ensure proper setup.

## Step 1: Extract the Backup File

Extract the backup file into the root directory.

```bash
tar xvpfz backup.tgz -C /
```

### Explanation:
- `tar`: The command to extract archive files.
- `xvpfz`: Options for extracting the file (eXtract, Verbose, Preserve file permissions, and compressed using gzip).
- `backup.tgz`: The archive file to be extracted.
- `-C /`: Specifies the target directory for extraction, which is the root (`/`).

## Step 2: Reboot the Server

After extracting the backup, reboot the server to apply the changes.

```bash
reboot
```

## Step 3: Configure Asterisk

### Create a User Group and User for Asterisk

```bash
groupadd asterisk
useradd asterisk -g asterisk
```

### Set Ownership for Asterisk Directories

```bash
chown asterisk:asterisk /etc/asterisk/
chown asterisk:asterisk /var/lib/asterisk/
chown asterisk:asterisk /run/asterisk
chown asterisk:asterisk /var/log/asterisk
chown asterisk:asterisk /usr/lib/asterisk
chown asterisk:asterisk /var/spool/asterisk
```

### Explanation:
- `groupadd asterisk`: Creates a new group named `asterisk`.
- `useradd asterisk -g asterisk`: Creates a new user `asterisk` and assigns it to the `asterisk` group.
- `chown`: Changes ownership of specified directories to the `asterisk` user and group.

Ensure that all required directories for Asterisk have the correct permissions for the `asterisk` user.

## Step 4: Configure MySQL

### Create a User for MySQL

```bash
useradd mysql
```

### Set Ownership and Permissions for MySQL Directories

```bash
chown -R mysql:mysql /var/lib/mysql
chown -R mysql:mysql /var/log/mysql
chmod 755 /var/log/mysql
```

### Start MySQL Service

```bash
systemctl start mysql
```

### Explanation:
- `useradd mysql`: Creates a new user `mysql`.
- `chown -R`: Recursively changes ownership of directories to the `mysql` user and group.
- `chmod 755`: Sets permissions to allow the owner to read, write, and execute, while group members and others can only read and execute.
- `systemctl start mysql`: Starts the MySQL service.

## Final Notes
- Ensure that the correct file paths and permissions are applied as required for your specific setup.
- Always verify the status of the services and ensure that they are running correctly after the configuration steps.
