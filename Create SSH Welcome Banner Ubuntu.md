# Steps to Hide the Ubuntu Welcome Banner

## Disable Dynamic MOTD Generation

```bash
vim /etc/default/motd-news
```

Change the following line:

```plaintext
ENABLED=1
```
to:

```plaintext
ENABLED=0
```

## Customize or Clear the MOTD

```bash
vim /etc/motd
```

## Remove MOTD Script Permissions

```bash
chmod -x /etc/update-motd.d/*
```

## Disable MOTD in SSH Sessions (Optional)

```bash
vim /etc/ssh/sshd_config
```

Find and set:

```plaintext
PrintMotd no
```

Restart the SSH service:

```bash
systemctl restart ssh
```

## Create a Banner File

```bash
vim /etc/banner.txt
```

Paste the following content:

```plaintext
█░█░█ █▀▀ █░░ █▀▀ █▀█ █▀▄▀█ █▀▀   ▀█▀ █▀█
▀▄▀▄▀ ██▄ █▄▄ █▄▄ █▄█ █░▀░█ ██▄   ░█░ █▄█

█▀▄ █▀▀ █▀▀ █▀█ █ ░░█ ▄▀█   ▀█▀ █▀▀ █░░ █▀▀ █▀▀ █▀█ █▀▄▀█
█▄▀ ██▄ ██▄ █▀▀ █ █▄█ █▀█   ░█░ ██▄ █▄▄ ██▄ █▄▄ █▄█ █░▀░█
```

Save and exit with `:wq!`.

## Edit the SSHD Configuration File

```bash
vim /etc/ssh/sshd_config
```

Modify the `Banner` parameter:

```plaintext
Banner /etc/banner.txt
```

Restart the SSH service:

```bash
systemctl restart ssh
```

## Verify the Banner

When logging in, you should see the banner displayed as follows:

```plaintext
login as: dtel
Pre-authentication banner message from server:
|
| █░█░█ █▀▀ █░░ █▀▀ █▀█ █▀▄▀█ █▀▀   ▀█▀ █▀█
| ▀▄▀▄▀ ██▄ █▄▄ █▄▄ █▄█ █░▀░█ ██▄   ░█░ █▄█
|
| █▀▄ █▀▀ █▀▀ █▀█ █ ░░█ ▄▀█   ▀█▀ █▀▀ █░░ █▀▀ █▀▀ █▀█ █▀▄▀█
| █▄▀ ██▄ ██▄ █▀▀ █ █▄█ █▀█   ░█░ ██▄ █▄▄ ██▄ █▄▄ █▄█ █░▀░█
End of banner message from server
