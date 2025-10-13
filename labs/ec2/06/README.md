# Auto Scaling Group und Load Balancer erstellen

In dieser Übungsaufgabe erstellen wir eine VPC über 2 Availability Zones, eine Auto Scaling Group sowie einen Load Balancer.

## VPC erstellen

Erstellen Sie wie in der vorletzten Aufgabe eine VPC – dieses Mal aber über 2 AZs.
Nennen Sie die VPC "Webserver".

![000-create-vpc.png](images/000-create-vpc.png)

## Startvorlage erstellen

Gehen Sie in der EC2-Webkonsole in das Startvorlagen-Menü.

![001-create-template-button.png](images/001-create-template-button.png)

---

Benennen Sie das Template und aktivieren Sie die Anleitung zur Einrichtung für Auto Scaling.

![002-template-wizard.png](images/002-template-wizard.png)

---

Wählen Sie Amazon Linux 2023 als AMI und t3.micro als Instance Type.

![003-template-wizard-ami.png](images/003-template-wizard-ami.png)
![004-template-wizard-type.png](images/004-template-wizard-type.png)

---

Nehmen Sie in den Netzwerkeinstellungen kein Subnetz auf. Legen Sie aber eine Sicherheitsgruppe an, für die Sie die VPC "Webserver" auswählen.

![005-template-wizard-network.png](images/005-template-wizard-network.png)

---

Richten Sie eine Regel ein, die auf Port 80 Traffic von jeder Quelle erlaubt.
Nennen Sie die Sicherheitsgruppe "Webserver".
![006-template-wizard-sg.png](images/006-template-wizard-sg.png)

---

Aktivieren Sie in den erweiterten Netzwerkeinstellungen die automatische Vergabe einer öffentlichen IP.
![008-ip.png](images/008-ip.png)

---

In den erweiterten Einstellungen ganz unten benötigen wir ein Benutzerdaten-Skript.

![007-template-wizard-userdata.png](images/007-template-wizard-userdata.png)

```bash
#!/bin/bash
# Update packages
dnf update -y

# Install Apache
dnf install -y httpd

# Enable and start Apache
systemctl enable httpd
systemctl start httpd

# Create a simple index page with the hostname
echo "<html><h1>Hello from $(hostname)</h1></html>" > /var/www/html/index.html
```

Sie können nun das Anlegen der Vorlage abschließen.

## Auto Scaling Group und Load Balancer erstellen 

Gehen Sie in der EC2-Webkonsole in das Auto Scaling-Menü.

![010-asg-create.png](images/010-asg-create.png)

---

Vergeben Sie einen Namen und wählen Sie die oben erstellte Startvorlage aus.

![011-asg-wizard-select-template.png](images/011-asg-wizard-select-template.png)

Gehen Sie nun zum nächsten Schritt.

---

Wählen Sie die Webserver-VPC aus und weiter unten die beiden öffentlichen Subnetze.

![012-az-select.png](images/012-az-select.png)

Gehen Sie nun zum nächsten Schritt.

---

Wählen Sie die Option, einen neuen Loadbalancer zu erstellen. Als Typ wählen Sie "Application Load Balancer", als Schema "Internet Facing".

![013-create-lb.png](images/013-create-lb.png)

Erzeugen Sie weiter unten eine neue Zielgruppe für den neuen Load Balancer.

![014-tg.png](images/014-tg.png)

Gehen Sie nun zum nächsten Schritt.

---

Geben Sie als gewünschte Kapazität und gewünschte Mindestkapazität jeweils 2 ein.

![015-counts.png](images/015-counts.png)

Sie können nun fortfahren, bis die Auto Scaling Group fertig angelegt ist.

## Security Group des Load Balancers anpassen

Gehen Sie in der EC2-Webkonsole in das Load Balancer-Menü. Wählen Sie den Loadbalancer aus und klicken Sie im Tab "Sicherheit" auf "Bearbeiten".

![020-edit-sg.png](images/020-edit-sg.png)

---

Nehmen Sie zu den Sicherheitsgruppen des Loadbalancers die Webserver-Sicherheitsgruppe hinzu.

![021-edit-sg-select.png](images/021-edit-sg-select.png)

---

Zuletzt können Sie nun die URL des Load Balancer kopieren, wenn Sie in die Detailansicht gehen und DNS-Namen kopieren.

![022-dns-name.png](images/022-dns-name.png)

## Test

Rufen Sie die URL auf. Verwenden Sie dabei aber auf jeden Fall HTTP als Protokoll, **nicht** HTTPS!

