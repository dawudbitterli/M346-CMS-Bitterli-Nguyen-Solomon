# WordPress Installationsskript

Dieses Skript automatisiert die Installation von WordPress auf einem Server.

```bash
#!/bin/bash

# Automatisch die IP-Adresse des Servers abrufen
server_ip=$(hostname -I | awk '{print $1}')

# Aktualisiere Paketlisten
sudo apt update

# Installiere Apache, MySQL, PHP und erforderliche PHP-Erweiterungen
sudo apt install -y apache2 mysql-server php libapache2-mod-php php-mysql

# MySQL-Befehle ausführen, um Datenbank und Benutzer zu erstellen
sudo mysql <<MYSQL_SCRIPT
CREATE DATABASE wordpress;
CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpressuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
MYSQL_SCRIPT

# Wechsle in das Verzeichnis /var/www/html
cd /var/www/html || exit

# Lade die neueste WordPress-Version herunter und entpacke sie
sudo wget https://wordpress.org/latest.tar.gz
sudo tar -xzvf latest.tar.gz
cd wordpress || exit

# Kopiere die Beispielkonfiguration und bearbeite sie mit den MySQL-Zugangsdaten
sudo cp wp-config-sample.php wp-config.php
sudo sed -i "s/database_name_here/wordpress/" wp-config.php
sudo sed -i "s/username_here/wordpressuser/" wp-config.php
sudo sed -i "s/password_here/password/" wp-config.php

# Ändere die Dateiberechtigungen und aktiviere mod_rewrite für saubere URLs
sudo chown -R www-data:www-data /var/www/html/wordpress
sudo chmod -R 755 /var/www/html/wordpress
sudo a2enmod rewrite
sudo systemctl restart apache2

# Zeige eine Erfolgsmeldung und Anweisungen zum Abschluss der Installation
echo "WordPress wurde erfolgreich installiert!"
echo "Bitte öffnen Sie Ihre Webseite im Browser und folgen Sie den Anweisungen, um die Einrichtung abzuschließen."
echo "Webseite: http://$server_ip/wordpress"
```
