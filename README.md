# LPIC Verwechslungsfallen-Cheatsheet

Sammlung von Dingen, die denselben Zweck erfüllen, sich aber je nach Distribution, Werkzeug oder Kontext unterscheiden. Genau diese "gleicher Zweck, anderer Befehl"-Stellen sind erfahrungsgemäß die häufigste Fehlerquelle in der LPIC-Prüfung.

## Prinzip

Jede Datei hier ist eine **Vergleichstabelle**: links die Aufgabe/der Zweck, rechts die unterschiedlichen Lösungen je nach System. Kein Karteikarten-Format wie im `lpic-lernkarten` Repo – hier geht es um schnelles Nachschlagen und Wiederholen, nicht ums Selbst-Eintippen.

## Inhalt

- [`paketmanagement.md`](paketmanagement.md) – apt/dpkg vs. dnf/rpm vs. zypper
- [`init-systeme.md`](init-systeme.md) – systemd vs. SysV Init
- [`dienst-vs-paketnamen.md`](dienst-vs-paketnamen.md) – wenn Paketname, Servicename und Programmname alle unterschiedlich heißen
- [`konfigurationspfade-apache-dns.md`](konfigurationspfade-apache-dns.md) – Apache & BIND9 Config-Orte im Distro-Vergleich
- [`firewall-im-detail.md`](firewall-im-detail.md) – firewalld vs. ufw, inkl. Zonen-Konzept
- weitere Themen folgen, sobald sie beim Lernen auftauchen

## Workflow

Wenn beim Üben (in `lpic-lernkarten`) eine neue Verwechslungsfalle auffällt: hier eine neue Tabelle/Zeile ergänzen, committen, pushen.

```bash
git add .
git commit -m "Neue Verwechslungsfalle: <Thema>"
git push
```
