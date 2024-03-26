## Benötigte Codes um ein Wordpress CMS zu installieren
### Muss noch in Bash script umgewandelt werden
```
sudo apt update
sudo apt install apache2 mysql-server php libapache2-mod-php php-mysql
sudo mysql
```
```
CREATE DATABASE wordpress;
CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpressuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```
```
cd /var/www/html
sudo wget https://wordpress.org/latest.tar.gz
sudo tar -xzvf latest.tar.gz
cd wordpress
sudo cp wp-config-sample.php wp-config.php
sudo nano wp-config.php
```
```
<?php
define( 'DB_NAME', 'wordpress' );
define( 'DB_USER', 'wordpressuser' );
define( 'DB_PASSWORD', 'password' );
define(‘DB_HOST’, ‘localhost);
define(‘DB_CHARSET’, ‘utf8’);
define(‘DB_COLLATE’, ‘’);
define( 'AUTH_KEY',         'put your unique phrase here' );
define( 'SECURE_AUTH_KEY',  'put your unique phrase here' );
define( 'LOGGED_IN_KEY',    'put your unique phrase here' );
define( 'NONCE_KEY',        'put your unique phrase here' );
define( 'AUTH_SALT',        'put your unique phrase here' );
define( 'SECURE_AUTH_SALT', 'put your unique phrase here' );
define( 'LOGGED_IN_SALT',   'put your unique phrase here' );
define( 'NONCE_SALT',       'put your unique phrase here' );
$table_prefix = 'wp_';
define( 'WP_DEBUG', false );
if ( ! defined( 'ABSPATH' ) ) {
        define( 'ABSPATH', __DIR__ . '/' );
}
require_once ABSPATH . 'wp-settings.php';
```
```
sudo chown -R www-data:www-data /var/www/html/wordpres´
sudo chmod -R 755 /var/www/html/wordpress
sudo a2enmod rewrite
sudo systemctl restart apache2
Cd ..
sudo rm index.html
sudo rm latest.tar.gz
Cd / 
sudo find /var/www/html/wordpress -mindepth 1 -exec mv -t /var/www/html {} +
Cd var/www/html
Sudo rmdir wordpress
```