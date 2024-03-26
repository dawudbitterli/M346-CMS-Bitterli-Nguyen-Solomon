# Automatische Erstellung einer EC2-Instanz auf AWS

Dieses Skript automatisiert den Prozess der Erstellung einer EC2-Instanz auf Amazon Web Services (AWS) und erfasst automatisch die Security Group ID und die Subnet ID.

## Voraussetzungen

Vor der Ausführung dieses Skripts müssen Sie sicherstellen, dass die AWS Command Line Interface (CLI) installiert und konfiguriert ist.

## Installation von `jq`

Das Skript überprüft zuerst, ob `jq`, ein Befehlszeilenwerkzeug zum Parsen von JSON, installiert ist. Wenn nicht, wird es automatisch installiert.

```bash
#!/bin/bash

# Prüfen, ob jq installiert ist, andernfalls installieren
if ! [ -x "$(command -v jq)" ]; then
  echo 'Error: jq is not installed. Installing...' >&2
  sudo apt update
  sudo apt install -y jq
fi
```

# Skript zur automatischen erstellung der EC2-Instanz

```bash
#!/bin/bash

# Skript zur automatischen Erstellung einer EC2-Instanz mit dynamischer Erfassung von Security Group ID und Subnet ID

# Variablen für die AWS CLI Konfiguration
REGION="us-east-1"  # Region, in der die Instanz erstellt werden soll
AMI_ID="ami-12345678"  # AMI-ID für Ubuntu 22.04 LTS (ersetzen Sie dies durch die tatsächliche AMI-ID)
INSTANCE_TYPE="t2.micro"  # Instanztyp
KEY_NAME="mein-key-pair"  # Name des Key-Pairs

# Befehl zur Erstellung der EC2-Instanz und automatisches Abrufen der Security Group ID und Subnet ID
echo "Erstelle EC2-Instanz..."
instance_info=$(aws ec2 run-instances \
    --region $REGION \
    --image-id $AMI_ID \
    --instance-type $INSTANCE_TYPE \
    --key-name $KEY_NAME \
    --output json)

# Extrahieren der Instanz-ID, Security Group ID und Subnet ID aus der Antwort
instance_id=$(echo $instance_info | jq -r '.Instances[0].InstanceId')
security_group_id=$(echo $instance_info | jq -r '.Instances[0].SecurityGroups[0].GroupId')
subnet_id=$(echo $instance_info | jq -r '.Instances[0].SubnetId')

# Warten, bis die Instanz bereit ist
echo "Warte auf die Bereitschaft der Instanz $instance_id ..."
aws ec2 wait instance-running --instance-ids $instance_id --region $REGION

# Abrufen der öffentlichen IPv4-Adresse der Instanz
public_ip=$(aws ec2 describe-instances --instance-ids $instance_id --query 'Reservations[*].Instances[*].PublicIpAddress' --output text --region $REGION)

# Ausgabe der Informationen
echo "Die Instanz mit der ID $instance_id wurde erstellt."
echo "Die öffentliche IPv4-Adresse der Instanz lautet: $public_ip"
echo "Die Security Group ID der Instanz lautet: $security_group_id"
echo "Die Subnet ID der Instanz lautet: $subnet_id"

```

# Alles in einem:

```bash
#!/bin/bash

# Skript zur automatischen Erstellung einer EC2-Instanz mit dynamischer Erfassung von Security Group ID und Subnet ID

# Prüfen, ob jq installiert ist, andernfalls installieren
if ! [ -x "$(command -v jq)" ]; then
  echo 'Error: jq is not installed. Installing...' >&2
  sudo apt update
  sudo apt install -y jq
fi

# Variablen für die AWS CLI Konfiguration
REGION="us-east-1"  # Region, in der die Instanz erstellt werden soll
AMI_ID="ami-12345678"  # AMI-ID für Ubuntu 22.04 LTS (ersetzen Sie dies durch die tatsächliche AMI-ID)
INSTANCE_TYPE="t2.micro"  # Instanztyp
KEY_NAME="mein-key-pair"  # Name des Key-Pairs

# Befehl zur Erstellung der EC2-Instanz und automatisches Abrufen der Security Group ID und Subnet ID
echo "Erstelle EC2-Instanz..."
instance_info=$(aws ec2 run-instances \
    --region $REGION \
    --image-id $AMI_ID \
    --instance-type $INSTANCE_TYPE \
    --key-name $KEY_NAME \
    --output json)

# Extrahieren der Instanz-ID, Security Group ID und Subnet ID aus der Antwort
instance_id=$(echo $instance_info | jq -r '.Instances[0].InstanceId')
security_group_id=$(echo $instance_info | jq -r '.Instances[0].SecurityGroups[0].GroupId')
subnet_id=$(echo $instance_info | jq -r '.Instances[0].SubnetId')

# Warten, bis die Instanz bereit ist
echo "Warte auf die Bereitschaft der Instanz $instance_id ..."
aws ec2 wait instance-running --instance-ids $instance_id --region $REGION

# Abrufen der öffentlichen IPv4-Adresse der Instanz
public_ip=$(aws ec2 describe-instances --instance-ids $instance_id --query 'Reservations[*].Instances[*].PublicIpAddress' --output text --region $REGION)

# Ausgabe der Informationen
echo "Die Instanz mit der ID $instance_id wurde erstellt." 
echo "Die öffentliche IPv4-Adresse der Instanz lautet: $public_ip"
echo "Die Security Group ID der Instanz lautet: $security_group_id"
echo "Die Subnet ID der Instanz lautet: $subnet_id"

 
```




