# Maludb-Dev-Setup.md

This file provides guidance to for installing Apache, PHP, Postgres Drivers, and MaluDB PHP SDK on Ubuntu 24.04.

The instructions assume a fresh installation of Ubuntu 24.04.  The process includes:
	1. Preparing Ubuntu
	2. Installing Apache, PHP, required libraries, and drivers.
	3. Installing the Composer and the MaluDB Client
	4. Installing NodeJs and Claude Code or Codex,

## Server Setup Instructions

1a. Extend the root filesystem created in the Ubuntu installation to use 100% of the space allocated when the server was provisioned.  If you provision 50-GB or more Ubuntu on ProxMox does not allocate all of the storage. 
```
sudo lvextend -l +100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv
```
1b. Update and upgrade the Ubuntu installation.
```
sudo apt update
sudo apt upgrade
```
2a. Install Apache and PHP drivers
```
sudo apt install apache2 -y
sudo systemctl enable apache2
sudo systemctl start apache2
sudo apt install php8.3 libapache2-mod-php8.3 php8.3-mysql php8.3-cli php8.3-curl php8.3-gd php8.3-mbstring php8.3-xml php8.3-zip -y
sudo apt install -y php8.3 libapache2-mod-php8.3 php8.3-pgsql
```
2b. Enable php and apache. 

```
sudo a2enmod php8.3
sudo systemctl restart apache2
```
3a. Install Composer 
```
# If composer isn't installed, install it system-wide:
curl -sS https://getcomposer.org/installer -o composer-setup.php
php composer-setup.php
sudo mv composer.phar /usr/local/bin/composer
rm composer-setup.php
composer --version
```
3b. Make sure the folder is in the path
```
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.profile
. ~/.profile
```
3c. Install MaluDB PHP Client using Composer.

```
composer require maludb/client
```
4a. Install NodeJs
```
curl -fsSL https://deb.nodesource.com/setup_24.x | sudo -E bash -
sudo apt install -y nodejs
mkdir -p ~/.npm-global
npm config set prefix ~/.npm-global
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```
4b. Install Claude Code or Codex
```
npm install -g @anthropic-ai/claude-code
```
or
```
npm install -g @openai/codex
```

## Sample Test Connection Script

Create  /var/www/html/test-pdo.php to test that you can connect to the database.
```
  <?php
  ini_set('display_errors', '1');
  error_reporting(E_ALL);

  try {
      $pdo = new PDO(
          'pgsql:host=<ip>;port=5432;dbname=<datbase>;sslmode=disable',
          '<user>',
          '<password>',
          [PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION]
      );

      header('Content-Type: text/plain');
      echo "connected\n";
      echo $pdo->query('select current_user, current_database()')->fetchColumn() . "\n";
  } catch (Throwable $e) {
      http_response_code(500);
      header('Content-Type: text/plain');
      echo get_class($e) . "\n";
      echo $e->getMessage() . "\n";
  }
```