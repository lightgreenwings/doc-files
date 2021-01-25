---
id: minecraft-gameserver-befehle
title: Minecraft Befehlsliste
sidebar_label: Befehle
---
Hier findest du eine Überischt über Befehle bei deinem Minecraft Server.


## Information
Das Spielgeschehen in Minecraft kannst du über Befehle im Chat oder per Konsole beeinflussen und deinen Wünschen nach anpassen.
Wie du zur Konsole kommst [erfährst du hier](gameserver#-gameserver-panel).

### 💻 Befehlsliste
Hier findest du eine Liste von den gängisten Befehlen und jeweils eine kurze Erklärung. Penguin ist hier immer ein Spielername

Befehl  | Erklärung  | Beispiel
------  | ------     | ------
/op SPIELER    | Macht einen Spieler zum Administrator auf deinem Server       | /op Penguin
/deop SPIELER  | Entfernt den Administratorenstatus eines Spielers       | /deop Penguin
/tp SPIELER ZIELSPIELER/X Y Z  | Ermöglicht das Teleportieren von Spielern zu Spielern oder Koordinaten     | /tp Penguin Penguin2, /tp Penguin 10 5 70
/gamemode GAMEMODE SPIELER  | Ändert den Spielmodus des eigenen oder angegeben Spielers | /gamemode creative Penguin
/time set TAGESZEIT  | Setzt die Tageszeit auf dem Server | /time set day
/weather WETTER  | Setzt das Wetter auf dem Server | /weather clear
/kill SPIELER  | Tötet den angegeben Spieler sofort | /kill Penguin
/clear SPIELER  | Leert das Inventar eines Spielers | /clear Penguin
/give SPIELER GEGENSTAND ANZAHL  | Gibt dem Spieler den festgelegten Gegenstand ([Gegenstandsliste](https://minecraftitemids.com/)) | /give Penguin minecraft:stone 64
/spawnpoint SPIELER | Setzt den Spawnpoint eines Spielers | /spawnpoint Penguin
/setworldspawn | Setzt den Spawnpunkt des Servers | /setworldspawn
/save-all | Speichert den aktuellen Stand der Welt und die Spieler | /save-all
/kick SPIELER GRUND | Kickt einen Spieler vom Server, dieser kann ihn aber wieder betreten | /kick Penguin Bitte im Support melden!
/ban SPIELER | Bannt einen Spieler vom Server, so dass dieser nicht mehr beitreten kann | /ban Penguin
/ban-ip SPIELER | Bannt die IP Adresse eines Spielers. Bei Servern im Offline Modus nötig | /ban-ip Penguin
/pardon Spieler | Entbannt einen Spieler, so dass dieser wieder dem Server beitreten kann | /pardon Penguin
/gamerule | Verwaltung der Gamerules von Minecraft. Das Beispiel stoppt den Tag-Nacht Zyklus auf dem Server | /gamerule doDaylightCycle false
/difficulty DIFFICULTY | Ändert den Schwierigkeitsgrad auf dem Server | /difficulty peaceful
/whitelist | Verwalten der Whitelist des Servers. Weitere Informationen findest du unten | /whitelist add Penguin

> In der Konsole müssen Befehle ohne `/` geschrieben werden. 

### 🚨 /op und /deop

In Minecraft gibt es 4 OP Level:

Level  | Rechte und Befehle  
------  | ------     
1 | Der Operator kann die Spawn Protection ignorieren. Es werden keine Befehle freigeschalten
2 | Die Befehle /clear, /difficulty, /effect, /gamemode, /gamerule, /give, /summon, /setblock und /tp werden freigeschaltet
3 | Operatoren können /ban, /deop, /whitelist, /kick und /op verwenden
4 | Die Befehle /stop und /save-all werden freigeschalten.

Standardmäßig wird das OP Level 4 vergeben. Die Level lassen sich in der Datei `ops.json` im Dateibrowser für den jeweiligen Spieler ändern.
Das Standardlevel kann in der Datei `server.properties` unter dem Punkt `op-permission-level=` festgelegt werden.

### 🕹 /gamemode

Es gibt folgende Gamemodes für Spieler:

Modus  | Beschreibung  
------  | ------     
survival | Der Spieler kann Blöcke abbauen und verliert Hunger, so wie Leben.
creative | Der Spieler hat ein unendliches Inventar mit allen Blöcken und verliert keine Lebens oder Hungercontainer
spectator | Der Spieler wird unsichtbar und kann durch die Welt fliegen. Ebenso kann man sich in die Sicht anderer Spieler begeben 
adventure | Der Spieler verliert keinen Hunter und kann die meisten Blöcke nicht abbauen

### 📕 /gamerule

Eine Liste mit allen Gamerules und einer ausführlichen Beschreibung kannst du [dem Minecraft Gamepedia entnehmen](https://minecraft-de.gamepedia.com/Befehl/gamerule).

### ⚔ /difficulty

Es gibt folgende Schwierigkeitsgräder:

Schwierigkeitsgrad  | Beschreibung  
------  | ------     
peaceful | Nachts erscheinen keine Monster und Spieler verlieren keinen Hunger. Leben wird automatisch regeneriert
easy | Monster sind aktiviert. Spieler verlieren maximal die Hälfte an Leben, wenn sie keine Hungercontainer mehr haben.
normal | Monster sind aktiviert und machen normalen Schaden. Ohne Hungercontainer verliert man, bis auf ein halbes Herz, alle.
hard | Zombies können Türen einschlagen. Leere Hungercontainer führen zum Tod

### 📃 /whitelist

Die Whitelist lässt sich über folgende Befehle verwalten: 

Befehl  | Beschreibung  
------  | ------     
/whitelist add SPIELER | Fügt einen Spieler zur Whitelist hinzu
/whitelist remove SPIELER | Entfernt einen Spieler von der Whitelist
/whitelist list | Listet alle Spieler auf der Whitelist im Chat auf
/whitelist on | Aktiviert die Whitelist
/whitelist off | Deaktiviert die Whitelist
/whitelist reload | Lädt die Whitelist neu. Dieser Befehl ist nur nötig, wenn über den Dateibrowser die Datei `whitelist.json` manuell bearbeitet wurde