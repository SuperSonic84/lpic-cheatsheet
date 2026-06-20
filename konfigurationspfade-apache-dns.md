# Konfigurationspfade: Apache & DNS im Distro-Vergleich

## Apache Webserver

| Was | Ubuntu (apache2) | RHEL/CentOS (httpd) |
|---|---|---|
| Hauptkonfiguration | `/etc/apache2/apache2.conf` | `/etc/httpd/conf/httpd.conf` |
| Virtual-Host-Configs | `/etc/apache2/sites-available/` | `/etc/httpd/conf.d/` |
| Aktivierte Sites einbinden | `a2ensite name` (Symlink-Mechanismus) | einfach Datei in `conf.d/` ablegen — wird automatisch gelesen |
| Module aktivieren | `a2enmod modulname` | Modul-Paket installieren (z.B. `mod_ssl`) |
| Standard-DocumentRoot | `/var/www/html` | `/var/www/html` (gleich!) |
| Log-Verzeichnis | `/var/log/apache2/` | `/var/log/httpd/` |

**Wichtige Falle:** Ubuntu nutzt das `a2ensite`/`a2enmod`-System mit Symlinks zwischen `sites-available/` und `sites-enabled/` — eine Config-Datei in `sites-available/` allein reicht NICHT, sie muss erst mit `a2ensite` aktiviert werden. Bei RHEL/httpd reicht es, die Datei einfach in `conf.d/` abzulegen.

---

## DNS (BIND9)

| Was | Ubuntu | RHEL/CentOS |
|---|---|---|
| Hauptkonfiguration | `/etc/bind/named.conf` | `/etc/named.conf` |
| Globale Optionen | `/etc/bind/named.conf.options` | meist direkt in `/etc/named.conf` |
| Lokale Zonen-Einbindung | `/etc/bind/named.conf.local` | meist direkt in `/etc/named.conf` |
| Zonen-Dateien (eigene Konvention) | `/etc/bind/zones/` | `/var/named/` |
| systemd-Service | `named` | `named` |
| Paketname | `bind9 bind9-utils` | `bind` `bind-utils` |

**Wichtige Lektion:** Ubuntu/Debian trennt die BIND-Konfiguration traditionell in mehrere Dateien (`named.conf`, `.options`, `.local`) die sich gegenseitig per `include` einbinden — RHEL hält oft alles direkter in einer Datei. Funktional identisch, aber beim Troubleshooting "wo trage ich das ein" muss man wissen, welche Konvention gerade vorliegt.

---

## Validierung vor dem Neuladen (gilt für beide!)

Bei BEIDEN Distributionen unbedingt vor jedem Neuladen prüfen:

```bash
named-checkconf                              # Syntax der Hauptkonfiguration
named-checkzone <zonenname> <zonendatei>     # Syntax einer einzelnen Zone
```

Erst danach:
```bash
systemctl reload named
```

**Merksatz:** Ein Syntaxfehler in einer Zonendatei verhindert NICHT zwingend, dass named überhaupt startet — aber die betroffene Zone wird dann einfach nicht geladen, ohne dass es offensichtlich auffällt. Immer named-checkzone nutzen, bevor man rätselt warum eine Zone "nicht funktioniert".
