---
id: pterodactyl-installieren
title: Pterodactyl installieren
sidebar_label: Pterodactyl installieren
---
Hier erfährst du, wie du Pterodactyl auf deinem Server installierst und was du alles zu beachten hast.

## Information
Pterodactyl ist ein Open-Source Game Server Management Panel, das mit PHP 7, React und Go entwickelt wurde. Pterodactyl wird mit Blick auf die Sicherheit entwickelt und führt alle führt alle Spieleserver in isolierten Docker-Containern aus, während es den Endbenutzern eine schöne und intuitive Benutzeroberfläche bietet.
Die nachfolgende Dokumentation ist für das Betriebssystem Ubuntu 18.04 geschrieben.
Es kann zu unterschiedlichen Befehlen kommen, wenn du ein anderes Betriebssystem verwendest.

## Voraussetzungen
    PHP 7.4 oder 8.0 (empfohlen) mit den folgenden Erweiterungen: cli, openssl, gd, mysql, PDO, mbstring, tokenizer, bcmath, xml or dom, curl, zip, und fpm (wenn du NGINX nutzen willst).
    MySQL 5.7.22 oder höher (MySQL 8 empfohlen) oder MariaDB 10.2 oder höher.
    Redis (redis-server)
    Einen Webserver (Apache, NGINX, Caddy, etc.)
    curl
    tar
    unzip
    git
    composer v2

## Installation
### 1. Abhängigkeiten installieren
Die oberen Abhängigkeiten kannst du wie folgt installieren:

    # Befehl "add-apt-repository" hinzufügen
    apt -y install software-properties-common curl apt-transport-https ca-certificates gnupg

    # Hinzufügen zusätzlicher Repositories für PHP, Redis und MariaDB
    LC_ALL=C.UTF-8 add-apt-repository -y ppa:ondrej/php
    add-apt-repository -y ppa:chris-lea/redis-server
    curl -sS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup | sudo bash

    # Liste der Repositories aktualisieren
    apt update

    # Universe-Repository hinzufügen, wenn du mit Ubuntu 18.04 arbeitest
    apt-add-repository universe

    # Abhängigkeiten installieren
    apt -y install php8.0 php8.0-{cli,gd,mysql,pdo,mbstring,tokenizer,bcmath,xml,fpm,curl,zip} mariadb-server nginx tar unzip git redis-server

### 2. Composer Installieren
Composer ist ein Abhängigkeitsmanager für PHP, der es uns ermöglicht, alles auszuliefern, was du für den Betrieb des Panels an Code benötigst. Du musst Composer installieren, bevor du mit der Installation fortfahren kannst.

    curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer

### 3. Das Panel herunterladen
Der erste Schritt ist es einen Ordner für Pterodactyl zu erstellen. Dies machst du wie folgt:

    mkdir -p /var/www/pterodactyl
    cd /var/www/pterodactyl

Wenn du dies gemacht hast, dann kannst du das Panel mit folgenden Befehlen herunterladen und entpacken:

    curl -Lo panel.tar.gz https://github.com/pterodactyl/panel/releases/latest/download/panel.tar.gz
    tar -xzvf panel.tar.gz
    chmod -R 755 storage/* bootstrap/cache/

### 4. MySQL Passwort setzen
Um fortfahren zu können müssen wir für Pterodactyl erstmal einen MySQL User und ein Passwort anlegen. Zu erst loggst du dich mit folgendem Befehl in den MySQL Root Nutzer ein:

    mysql -u root -p

Wenn du kein Passwort bei der Installation gesetzt hast (oder setzen konntest), dann drücke einfach Enter.

Erstelle nun den neuen Benutzer wie folgt:

    USE mysql;
    CREATE USER 'pterodactyl'@'127.0.0.1' IDENTIFIED BY 'einPasswort';

Ändere `einPasswort` mit einem Passwort deiner Wahl. Vergiss nicht, dass du dir dieses Passwort irgendwie abspeichern oder merken musst.

Danach erstellst du die Datenbank mit folgendem Befehl:

    CREATE DATABASE panel;

Nun Braucht der Nutzer natürlich noch das Recht diese Datenbank bearbeiten zu können. Dies machst du wie folgt:

    GRANT ALL PRIVILEGES ON panel.* TO 'pterodactyl'@'127.0.0.1' WITH GRANT OPTION;
    FLUSH PRIVILEGES;

Du kannst natürlich sowohl den Nutzer als auch die Datenbank anders nennen. Vergiss aber dann nicht, dass du bei den Befehlen den Nutzernamen und die Datenbank auch abändern musst.

Aus der MySQL Konsole kommst du mit dem Befehl `exit` raus.

### 5. Installation des Panels
Nachdem du MySQL eingerichtet hast kannst du mit der Installation des Panels fortfahren.
Dafür führst du folgende Befehle aus:

    cp .env.example .env

    composer install --no-dev --optimize-autoloader

    php artisan key:generate --force

### 6. Panel Konfigurieren
Nachdem du das Panel installiert hast, musst du dieses nun konfigurieren.
Zu allerst führen wir das Setup mit folgendem Befehl durch:

    php artisan p:environment:setup

Nachdem dies erfolgreich abgeschlossen wurde müssen wir noch die Datenbank konfigurieren.
Nimm hier den MySQL Nutzer und die MySQL Datenbank aus Schritt 4.
Mit folgendem Befehl konfigurierst du die Datenbank:

    php artisan p:environment:database

 Wenn du jetzt noch Mails vom Panel erhalten willst, dann führe musst du den Mailserver noch konfigurieren.
Um den internen Mailversand von PHP zu verwenden (nicht empfohlen), wähle "mail". Zur Verwendung eines benutzerdefinierten SMTP-Server wähle "smtp".

    php artisan p:environment:mail

### 7. Datenbank einrichten
Nachdem wir die oberen Schritte durchgeführt haben muss die Datenbank noch initialisert und vom Panel gefüllt werden. Dies machst du mit folgendem Befehl:

    php artisan migrate --seed --force

### 8. Ersten Nutzer hinzufügen
Wenn alle Schritte erfolgreich abgeschlossen wurden kannst du mit der Erstellung des ersten Admin Nutzer fortfahren.
Führe dafür folgenden Befehl aus:

    php artisan p:user:make

### 9. Berechtigungen setzen
Jetzt fehlen lediglich noch die Berechtigungen für den Nutzer vom Webserver.
Führe dafür den richtigen Befehl aus den nachfolgenden Befehlen aus:

    # Für NGINX oder  Apache (nicht auf CentOS):
    chown -R www-data:www-data /var/www/pterodactyl/*

    # Für NGINX auf CentOS:
    chown -R nginx:nginx /var/www/pterodactyl/*

    # Für Apache auf CentOS
    chown -R apache:apache /var/www/pterodactyl/*

### 10. Feinschliff
Pterodactyl verwendet Warteschlangen, damit das System schnell läuft und Aufgaben wie Mailversand, etc. im Hintergrund durchführt. 

Dafür muss in der Crontab folgendes ergänzt werden. Zur Crontab kommst du mit dem Befehl

    sudo crontab -e

Der zu ergänzende Inhalt ist folgender:

    * * * * * php /var/www/pterodactyl/artisan schedule:run >> /dev/null 2>&1

Damit deine Warteschlange auch abgearbeitet wird musst du einen Queue Worker in der systemd Umgebung hinzufügen. Dafür führst du zu erst folgenden Befehl aus:

    sudo nano /etc/systemd/system/pteroq.service

Der Inhalt ist folgender:

    # Pterodactyl Queue Worker File
    # ----------------------------------

    [Unit]
    Description=Pterodactyl Queue Worker
    After=redis-server.service

    [Service]
    # On some systems the user and group might be different.
    # Some systems use `apache` or `nginx` as the user and group.
    User=www-data
    Group=www-data
    Restart=always
    ExecStart=/usr/bin/php /var/www/pterodactyl/artisan queue:work --queue=high,standard,low --sleep=3 --tries=3

    [Install]
    WantedBy=multi-user.target

Danach fehlen nur noch folgende Befehle:

    sudo systemctl enable --now redis-server
    sudo systemctl enable --now pteroq.service

Das wäre die Installation vom Panel. Als nächster Schritt wäre die Konfiguration des Webservers, damit du das Panel über das HTTPS Protokoll erreichen kannst. Dies findest du unter `Hier Link zur Doku hinzufügen`.