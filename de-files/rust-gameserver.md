---
id: rust-gameserver
title: Rust Gameserver
sidebar_label: Allgemeines
---
Hier findest du alles wichtige, was du über deinen Rust Gameserver wissen musst.


## Verwaltung

Deinen Gameserver kannst du unter [panel.deinserverhost.de](https://panel.deinserverhost.de) verwalten.
Wie du dich dort anmelden kannst, [erfährst du hier](gameserver#-gameserver-panel).

### ⚔ Admin werden

1. In der Dateiverwaltung zu `server/rust/cfg/` navigieren

2. Öffne die Datei `users.cfg` und füge folgendes hinzu: 

     ```sh
     <Rolle> <64bit Steam ID> <Name des Admins>
     ```
   
   Deine 64bit Steam ID kannst du [hier abrufen](https://steamid.io/).
   
   Verfügbare Rollen:
   
   | Rolle | Berechtigung |
   | --------   | --------- |
   | ownerid    | Der Benutzer kann alle Admin Befehle ausführen und neue Admins hinzufügen oder entfernen            |
   | moderatorid| Der Benutzer kann alle Admin Befehle ausführen, jedoch keine neuen Admins hinzufügen oder entfernen |

3. Speichere die Datei und starte den Server neu

Du bist nun Admin auf deinem Server ✔

### 🚀 RCON verbinden

Wenn du dir deinen persönlichen RCON Port noch nicht beantragt hast, [öffne bitte ein Supportticket](https://deinserverhost.de/store/submitticket.php).

So verbindest du dich per WebRCON auf deinen Server um ihn zu verwalten: 

1. Lade dir beispielsweise unter [rustadmin.com](https://www.rustadmin.com/) das RCON Tool herunter

2. Klicke in der Leiste auf `Configuration` und fülle die eingerahmten Felder aus

3. Danach auf `Save` klicken. Hier kannst du der Konfiguration noch einen Namen geben.

![img](../static/img/rust/rcontool.png)
> [!] Der Server muss vollständig hochgefahren sein, um sich per RCON verbinden zu können.

Klicke danach links oben auf `Server` und `Connect`.

![img](../static/img/rust/rconconnect.png)

Jetzt kannst du RCON zur Verwaltung nutzen ✔