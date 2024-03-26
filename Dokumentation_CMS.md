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
