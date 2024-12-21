# Install PHP Composer on RHEL8

## Install the Required PHP Packages
```bash
yum -y install php php-cli php-zip php-json
```

## Download Composer Executable File
```bash
curl -sS https://getcomposer.org/installer | php
```

## Make Composer Available Globally
```bash
mv composer.phar /usr/local/bin/composer
chmod +x /usr/local/bin/composer
```

## Check the Version of PHP Composer
```bash
composer -V
```

## Update Composer Version
```bash
composer self-update
