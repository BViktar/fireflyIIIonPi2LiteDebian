# fireflyIIIonPi2LiteDebian
How to install firefly-iii on raspberry Pi 2 with debian lite image

sudo nano /etc/locale.gen

- look for de_DE.UTF8 and uncomment the line
- save locale.gen

sudo locale-gen

- update the locale needed to reboot

sudo wget -qO /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg

echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/php.list

sudo apt install -y mariadb-server

mysql_secure_installation

- set the root password
- skip unix_authentification
- change the root password
- reload privileges tables
 
mariadb -u root -pROOT_PASS

CREATE DATATBASE firefly;

GRANT ALL ON firefly.* TO 'firefly'@'localhost' IDENTIFIED BY 'db_pass' WITH GRANT OPTION;

FLUSH PRIVILEGES;

exit;

sudo apt update

sudo apt install -y php8.3-common php8.3-cli

sudo apt full-upgrade -y

sudo apt install -y php8.3-curl php8.3-gd php8.3-mbstring php8.3-xml php8.3-zip php8.3-bcmath php8.3-intl php8.3-mysql

curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer

sudo apt install -y libapache2-mod-php8.3

service --status-all

sudo service apache2 stop

cd /var/www

sudo composer create-project grumpydictator/firefly-iii --no-dev --prefer-dist firefly-iii 6.1.4

sudo chown -R www-data:www-data firefly-iii

sudo chmod -R 775 firefly-iii/storage

cd firefly-iii

sudo nano .env 
- make changes below

DB_HOST=localhost

DB_PASSWORD=db_pass
- save the file

sudo php artisan firefly-iii:upgrade-database

sudo php artisan firefly-iii:correct-database

sudo php artisan firefly-iii:report-integrity

sudo ln -s /var/www/firefly-iii /var/www/html/firefly-iii

sudo service apache2 start

- in browser go to http://raspberry_url_ip/firefly-iii/public

💪 🤘 👏 Enjoy your meal! 😄
