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
---
## *Schritt 2: ssh-Verbindung*

durch das erstellen einer Instanz mit einem neuen Key-Pair, müsste Ihnen aufgefallen sein, das sich eine Datei heruntergeladen hat, welche den Schlüssel beinhaltet.  

Bewegen Sie wenn nötig den Schlüssel in den .ssh Ordner und prüfen Sie die ssh-Verbindung:
```
ssh -i c:\users\<Benutzer>\.ssh\<Key-Pair Dateiname.pem> ubuntu@<Public IPv4>
```
Wenn die Verbindung durch ssh erfolgreich war, führen sie über die ssh-Verbindung folgende Befehle aus:
```
sudo apt update
sudo apt install apache2
sudo chmod 777 /var/www/html/
```

Damit Ihnen die Firewall keine Probleme macht, müssen Sie kleine Veränderungen an der Firewall der erstellten INstanz vornehmen
