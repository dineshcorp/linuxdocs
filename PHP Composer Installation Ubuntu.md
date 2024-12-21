# Install PHP Composer On Ubuntu 20.04

## Update and Install Required PHP Packages
```bash
sudo apt update
sudo apt install -y php php-cli php-zip php-json
```

## Download Composer Executable File
```bash
curl -sS https://getcomposer.org/installer | php
```

## Make Composer Available Globally
```bash
sudo mv composer.phar /usr/local/bin/composer
sudo chmod +x /usr/local/bin/composer
```

## Check the Version of PHP Composer
```bash
composer -V
```

## Update Composer Version
```bash
composer self-update
```
