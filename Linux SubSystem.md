Set Linux Sub System in Windows

```bash
sudo apt-get install build-essential curl file git git-core
```

Install Z Shell

```bash
sudo apt-get install zsh
```

Install Oh-my-zsh

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Install Node JS

```bash
# installs NVM (Node Version Manager)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

# download and install Node.js
nvm install 21

# verifies the right Node.js version is in the environment
node -v # should print `v21.7.1`

# verifies the right NPM version is in the environment
npm -v # should print `10.5.0`
```

Update Sub System

```bash
sudo apt-get update
```

Install Apache Server

```bash
sudo apt-get install apache2
```

For Check Apache Server Status

```bash
sudo systemctl status apache2
```

Install PHP

```bash
sudo add-apt-repository -y ppa:ondrej/php
```

```bash
sudo apt install php8.2 php8.2-cli php8.2-common php8.2-curl php8.2-pgsql php8.2-fpm php8.2-gd php8.2-imap php8.2-intl php8.2-mbstring php8.2-mysql php8.2-opcache php8.2-soap php8.2-xml php8.2-zip
```

Install Composer

```bash
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === 'dac665fdc30fdd8ec78b38b9800061b4150413ff2e3b6f88543c636f7cd84f6db9189d43a81e5503cda447da73c7e5b6') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
sudo mv composer.phar /usr/local/bin/composer
```

Install MySQL/ MariaDB Server

```bash
sudo apt install mariadb-server
```


