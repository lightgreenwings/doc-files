---
id: proxmox-installieren
title: Proxmox installieren
sidebar_label: Proxmox installieren
---
Hier erfährst du, wie du nachträglich Proxmox auf deinem Server installierst und was du alles zu beachten hast.


## Information
Proxmox VE (Proxmox Virtual Environment; kurz PVE) ist eine auf Debian basierende Open-Source-Virtualisierungsplattform zum Betrieb von virtuellen Maschinen mit einer Web-Oberfläche zur Einrichtung und Steuerung von KVM und LXC Servern. Hier wird die Installation von PVE 6 auf einem Debian 10 Server erklärt.
PVE 7 ist bereits draußen und kann ähnlich installiert werden.
<br></br>

## Voraussetzungen
Du besitzt einen Dedicated Server und hast dort Debian 10 installiert.

### 1. Setze den richtigen Hostnamen! 
Dein PVE Server braucht einen sprechenden Namen, wenn dir der festgelegte Name gefällt, dann kannst du diesen Schritt überspringen.

Geändert wird der Hostnamen wie folgt. Führe in der Shell/ Terminal des Servers folgenden Befehl aus:

    nano /etc/hosts

Ändere den Inhalt der Datei so, wie im folgenden Beispiel:

    127.0.0.1       localhost.localdomain localhost
    HierStehtDeineIp   HierStehtDieDomain AbkürzungDerDomain

    # The following lines are desirable for IPv6 capable hosts
    ::1     localhost ip6-localhost ip6-loopback
    ff02::1 ip6-allnodes
    ff02::2 ip6-allrouters

Hinweis: Die Abkürzung ist kein muss, führt aber zu einer schöneren Anzeige in der Shell.
Ebenso sollte die Datei /etc/hostname bearbeitet werden. Dazu gehst du wie folgt vor:

    nano /etc/hostname

Hier änderst du den Namen dann zu dem, den du in der /etc/hosts Datei gesetzt hast. Die IP ist nicht notwendig.

Testen ob alles geklappt hat kannst du wie folgt:

    hostname --ip-address
    192.168.15.77 # should return your IP address here

In manchen Fällen ist auch ein Neustart des Servers oder des SSH Clients notwendig.


### 2. Sources.list
Füge mit folgendem Befehl PVE zu deinen Installations-Sourcen hinzu:

    echo "deb [arch=amd64] http://download.proxmox.com/debian/pve buster pve-no-subscription" > /etc/apt/sources.list.d/pve-install-repo.list

Wenn du PVE unterstützt und eine Subscription erworben hast, dann kannst du hier die Enterprise Edition nehmen.

### 3. Proxmox VE repository key
Füge mit folgenden Befehlen den PVE Repository Key hinzu:

    wget http://download.proxmox.com/debian/proxmox-ve-release-6.x.gpg -O /etc/apt/trusted.gpg.d/proxmox-ve-release-6.x.gpg

    chmod +r /etc/apt/trusted.gpg.d/proxmox-ve-release-6.x.gpg  # optional, if you have a non-default umask

### 4. Repository und System aktualisieren
Aktualisiere dein System und das Repository mit folgendem Befehl:

    apt update && apt full-upgrade
Dadurch kann die eigentliche Installation von PVE starten.
<br></br>

## 💻 Installation
### 1. Proxmox VE Packages installieren
Mit folgendem Befehl wird PVE endlich installiert

    apt install proxmox-ve postfix open-iscsi

Konfiguriere die Pakete, die bei der Installation eine Benutzereingabe erfordern, nach deinen Bedürfnissen (z.B. Samba, das nach WINS/DHCP-Unterstützung fragt). 
Wenn du einen Mailserver in deinem Netzwerk hast, solltest du Postfix konfigurieren. Dein vorhandener Mailserver ist dann der Relay-Host, der die vom Proxmox-Server gesendeten E-Mails an ihren endgültigen Empfänger weiterleitet.

Wenn du nicht weißt, was du hier eingeben sollst, dann wähle nur lokal und lasse den Systemnamen so wie er ist.

Zum Schluss startest du dein System neu, der neue PVE-Kernel sollte automatisch im GRUB-Menü ausgewählt werden.

Hinweis: Wenn du einen Subscriptions-Key hast, dann solltest du nach der Installation zum Enterprise-Repository zu wechseln.

### 2. OS-Prober entfernen
Es wird empfohlen den OS-Prober zu entfernen. Dies machst du mit folgendem Befehl:

    apt remove os-prober

### 3. Webinterface und Linux Bridge
Nach der Installation ist die PVE Oberfläche mit folgender URL aufrufbar:
https://youripaddress:8006

Deine Logindaten sind die vom Root Nutzer. Auswählen musst du "PAM Authentication".

Nach der Installation musst du unter Netzwerk/ Network eine Linux Bridge mit dem Namen vmbr0 erstellen. Dies sollte dann ähnlich wie folgendes Bild aussehen:

![img](/img/proxmox/proxmox-linux-bridge.png)
