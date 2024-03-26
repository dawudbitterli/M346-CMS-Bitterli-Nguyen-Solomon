# M346-CMS-Bitterli-Nguyen-Solomon
**Dokumentation des CMS Projektes der Gruppe Bitterli-Nguyen-Solomon.**  
  
**Autor:** *David Bitterli*  
**Datum:** *19.03.2024*  
**Ort:** *St. Gallen*  
**Aufgabe:** *LBV2 CMS*  
---
## *Schritt 1: Erstellung der EC2 Instanz*

Als ersten Schritt zur Funktionierenden CMS Anwendung, muss eine **EC2** Instanz erstellt werden auf der später das Programm läuft. In unserem Fall haben wir das Betriebssystem **Ubuntu 22.04 LTS** mit den Folgenden spezifikationen konfiguriert:  
  * **Instanztyp:** *t2.micro*
  * **Key-Pair:** *Create new key pair*
  * **Allow SSH traffic from:** *Anywhere: (0.0.0.0/0)*

Nach dem Sie die Instanz konfiguriert haben, *launchen* Sie diese.

Nun sehen Sie die erstellte Instanz wenn sie auf *Instances* drücken. Wenn man nun auf die erstellte Instanz drückt, öffnet sich ein feinster, in welchem man sich Wichtige Informationen entnehmen kann wie z.B. die **Public IPv4** Adresse.  


Damit Ihnen die Firewall keine Probleme macht, müssen Sie kleine Veränderungen an der Firewall der erstellten Instanz vornehmen. Folgen Sie hierfür dem folgenden Pfad:
  * Instances
  * Security
  * Security groups
  * Edit inbound rules
  * Add rule

Nun wählen sie als Typ: **HTTP**  
Port range: **80**  
CIDR blocks: **0.0.0.0/0**  

Speichern Sie nun die Regeln und verwenden Sie die Public IPv4 Adresse der Instanz in einem Browser mit dem Port 80 am Ende:

`0.0.0.0/:80`

## *Schritt 2: ssh-Verbindung*

durch das erstellen einer Instanz mit einem neuen Key-Pair, müsste Ihnen aufgefallen sein, das sich eine Datei heruntergeladen hat, welche den Schlüssel beinhaltet.  

Bewegen Sie wenn nötig den Schlüssel in den .ssh Ordner und prüfen Sie die ssh-Verbindung:
```
ssh -i c:\users\<Benutzer>\.ssh\<Key_Pair_Dateiname.pem> ubuntu@<Public IPv4>
```
Wenn die Verbindung durch ssh erfolgreich war, führen Sie über die ssh-Verbindung folgende installationen aus:
```
sudo apt update
sudo apt install apache2 mysql-server php libapache2-mod-php php-mysql
```

## *Schritt 3: MySql Datenbank*

Nun muss eine Datenbank für unsere Wordpressdatei angelegt werden.
```
sudo mysql
```

```
CREATE DATABASE wordpress;
CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpressuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

## *Schritt 4: Wordpress*

Wechseln sie durch den befehl in das Arbeitsverzeichnis
```
cd /var/www/html
```

Durch den folgenden Befehl weird die neueste Version von WordPress heruntergeladen:
```
sudo wget https://wordpress.org/latest.tar.gz
```

Nun wurde die neueste Version von WordPress als .zip Datei heruntergeladen. Diese Datei muss nun noch extrahiert werden. Verwenden sie den folgenden Befehl um die Datei in Ihr jetziges Verzeichnis zu extrahieren:

```
sudo tar -xzvf latest.tar.gz
```

Nach dem WordPress extrahiert wurde, können sie in das WordPress verzeichnis wechseln:
```
cd wordpress
```

```
sudo cp wp-config-sample.php wp-config.php
```
Dieser Befehl kopiert die Beispieldatei `wp-config-sample.php` und erstellt eine neue Konfigurationsdatei namens `wp-config.php`. Diese Datei wird später bearbeitet, um die Verbindung zur Datenbank und herzustellen und andere Einstellungen für WordPress festzulegen.

Öffnen Sie nun den Texteditor im Terminal um die `wp-config.php` Datei zu bearbeiten. In ihr werden nun wichtige Konfigurationseinstellungen vorgenommen.

```
sudo nano wp-config.php
```

## *Schritt 5: wp-config.php*

Folgend zeigen wir den Code den wir in der `wp-config.php` verwendet haben, um die Datei nach unseren Anforderungen zu Konfigurieren:

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

## *Schritt 6: Endspurt*

Der folgende Code ändert den Besitzer und die Gruppe aller Dateien und Verzeichnisse innerhalb des WordPress-Verzeichnisses in `www-data`.

```
sudo chown -R www-data:www-data /var/www/html/wordpress
```
Nun müssen wir die Berechtigung aller Dateien Verzeichnisse innerhalb des WordPress Verzeichnisses ändern damit sichergestellt ist, das der Webserver auf die Dateien zugreifen kann. Diese Berechtigungen sind wichtig, damit der Besitzer die dateien umschreiben und ausführen kann während andere Benutzer sie nur ausführen können.

```
sudo chmod -R 755 /var/www/html/wordpress
```
Nun müssen wir das Apache-Modul `mod_rewrite` aktivieren, das für das Umschreiben von URLs verwendet wird. Dies ist notwendig, um die Permalinks in WordPress zu aktivieren.

```
sudo a2enmod rewrite
```

Nun starten wir den Server neu, um Änderungen anzuwenden und anschliessend das Verzeichnis wechseln.
```
sudo systemctl restart apache2
cd ..
```

Mit dem folgenden Code löschen wir die Datei `index.html` im aktuellen Verzeichnis, damit gewährleistet ist, das der Webserver auf die `index.php` Datei zugreift. Ebenfalls Löschen wir anschliessend die Datei `latest.tar.gz` Datei, da sie überflüssig ist und durch das Löschen Speicherplatz freigegeben wird
```
sudo rm index.html
sudo rm latest.tar.gz
```

Nun in das Stammverzeichnis wechseln
```
Cd /
```
Der folgende Code verschiebt alle Dateien und Verzeichnisse innerhalb des WordPress-Verzeichnisses in das übergeordnete Verzeichnis `/var/www/html`. Dieser Befehl wird verwendet, um sicherzustellen, dass die Dateien und Verzeichnisse direkt im Hauptverzeichnis des Webservers liegen.
```
sudo find /var/www/html/wordpress -mindepth 1 -exec mv -t /var/www/html {} +
```

Nun wechseln wir in unser Kernverzeichnis `/var/www/html`.
```
Cd var/www/html
```

Abschliessend Löschen wir nun das leere wordpress Verzeichnis, da alle Dateien in diesem Verzeichnis schon verschoben wurden und es für uns keinen nutzen mehr hat.

```
Sudo rmdir wordpress
```
