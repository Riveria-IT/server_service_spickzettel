# Linux Server & Services – README

Praktische Übersicht der wichtigsten Befehle, um auf einem Linux-Server zu prüfen, **was läuft**, **welche Dienste aktiv sind**, **welche Ports offen sind** und **wie du Fehler findest**.

---

## Inhalt

- Server allgemein prüfen
- Laufende Dienste anzeigen
- Dienste starten, stoppen, neu starten
- Autostart prüfen
- Logs ansehen
- Offene Ports prüfen
- Prozesse prüfen
- Installierte Pakete anzeigen
- Docker prüfen
- Speicher, RAM und Festplatten prüfen
- Nützliche Schnellchecks

---

## 1. Server allgemein prüfen

```bash
hostnamectl
```
Zeigt Hostname, Betriebssystem, Kernel und Architektur.

```bash
uname -a
```
Zeigt Kernel- und Systeminformationen.

```bash
whoami
```
Zeigt den aktuellen Benutzer.

```bash
uptime
```
Zeigt, wie lange der Server bereits läuft.

---

## 2. Laufende Dienste anzeigen

```bash
systemctl list-units --type=service --state=running
```
Zeigt alle aktuell laufenden Services.

```bash
systemctl list-units --type=service
```
Zeigt Services allgemein.

```bash
systemctl list-unit-files --type=service
```
Zeigt installierte Service-Dateien.

---

## 3. Status eines bestimmten Dienstes prüfen

Beispiele:

```bash
systemctl status nginx
systemctl status apache2
systemctl status mysql
systemctl status mariadb
systemctl status docker
systemctl status ssh
```

Damit siehst du direkt:
- ob der Dienst läuft
- ob Fehler vorhanden sind
- ob der Dienst beim Boot aktiviert ist

---

## 4. Dienste starten, stoppen, neu starten

```bash
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
sudo systemctl reload nginx
```

### Aktivieren beim Start
```bash
sudo systemctl enable nginx
```

### Deaktivieren beim Start
```bash
sudo systemctl disable nginx
```

---

## 5. Autostart prüfen

```bash
systemctl list-unit-files --state=enabled
```
Zeigt alle Dienste, die automatisch beim Boot starten.

---

## 6. Logs ansehen

### Logs eines Dienstes
```bash
journalctl -u nginx
```

### Live mitlaufen lassen
```bash
journalctl -u nginx -f
```

### Letzte Fehler im System
```bash
journalctl -xe
```

### Letzte Zeilen einer Logdatei
```bash
tail -n 50 /var/log/syslog
```

### Log live ansehen
```bash
tail -f /var/log/syslog
```

---

## 7. Offene Ports prüfen

```bash
ss -tulpn
```
Zeigt:
- offene Ports
- Protokolle
- welcher Dienst daran hängt
- welche IP verwendet wird

Alternative:

```bash
lsof -i -P -n
```

---

## 8. Prozesse prüfen

```bash
ps aux
```
Zeigt alle laufenden Prozesse.

```bash
top
```
Live-Ansicht der Systemlast.

```bash
htop
```
Schönere Ansicht, falls installiert.

Prozess beenden:

```bash
kill PID
```

Mit Gewalt:

```bash
kill -9 PID
```

---

## 9. Installierte Pakete anzeigen

### Ubuntu / Debian
```bash
dpkg -l
```

Nur manuell installierte Pakete:

```bash
apt-mark showmanual
```

### Rocky / AlmaLinux / CentOS
```bash
dnf list installed
```

Oder:

```bash
rpm -qa
```

---

## 10. Docker prüfen

Laufende Container:

```bash
docker ps
```

Alle Container:

```bash
docker ps -a
```

Docker Images:

```bash
docker images
```

Docker Logs:

```bash
docker logs CONTAINERNAME
```

Live Logs:

```bash
docker logs -f CONTAINERNAME
```

---

## 11. Speicher, RAM und Festplatten prüfen

### Speicherplatz
```bash
df -h
```

### Ordnergrössen
```bash
du -sh /var
```

### RAM
```bash
free -h
```

### Laufwerke anzeigen
```bash
lsblk
```

---

## 12. Festplattenzustand prüfen

Falls `smartctl` noch nicht installiert ist:

### Ubuntu / Debian
```bash
sudo apt install smartmontools
```

### Zustand prüfen
```bash
sudo smartctl -a /dev/sda
```

Bei NVMe:

```bash
sudo smartctl -a /dev/nvme0n1
```

Wichtig dort:
- Health Status
- Errors
- Temperature
- Power On Hours
- Percentage Used

---

## 13. Backup grob prüfen

Ob ein Timer oder Dienst läuft:

```bash
systemctl list-timers
systemctl status dein-backup-dienst
```

Ob neue Dateien vorhanden sind:

```bash
ls -lt /pfad/zum/backup
```

Grösse prüfen:

```bash
du -sh /pfad/zum/backup/*
```

Logs prüfen:

```bash
journalctl -u dein-backup-dienst
```

---

## 14. Nützliche Schnellchecks

### Nur laufende Dienste
```bash
systemctl list-units --type=service --state=running
```

### Nur offene Ports
```bash
ss -tulpn
```

### Nur Prozesse
```bash
ps aux
```

### Nur RAM
```bash
free -h
```

### Nur Festplattenplatz
```bash
df -h
```

### Nur Docker
```bash
docker ps -a
```

---

## 15. Sehr praktische Standard-Kombi

Wenn du einen Server schnell prüfen willst:

```bash
hostnamectl
uptime
free -h
df -h
lsblk
systemctl list-units --type=service --state=running
ss -tulpn
ps aux
docker ps -a
```

Damit bekommst du sehr schnell einen guten Überblick.

---

## 16. Typische Dienste, die man oft prüft

```bash
systemctl status nginx
systemctl status apache2
systemctl status mysql
systemctl status mariadb
systemctl status php8.2-fpm
systemctl status docker
systemctl status ssh
systemctl status fail2ban
```

---

## 17. Tipp

Wenn du auf einem Server etwas suchst, arbeite meist in dieser Reihenfolge:

1. Läuft der Dienst?
2. Ist der Port offen?
3. Gibt es Fehler im Log?
4. Hat der Server genug RAM und Speicherplatz?
5. Läuft der Dienst auch nach einem Neustart automatisch?

---

## 18. Kurzfassung

Die wichtigsten Befehle für den Alltag:

```bash
systemctl status DIENST
systemctl restart DIENST
journalctl -u DIENST -f
ss -tulpn
ps aux
free -h
df -h
lsblk
docker ps -a
```

---

Erstellt als einfache Server-Service-Übersicht für Linux.
