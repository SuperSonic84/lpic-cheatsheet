# Firewall im Detail: firewalld (RHEL/SUSE) vs. ufw (Ubuntu)

## Grundphilosophie-Unterschied (wichtig zu verstehen, nicht nur Befehle!)

**firewalld arbeitet mit Zonen** — jedes Netzwerk-Interface gehört zu einer Zone (z.B. `public`, `home`, `trusted`), und Regeln gelten pro Zone. Eine Regel ohne Zonen-Angabe wirkt auf die **aktuell aktive Standard-Zone**.

**ufw ist einfacher/linearer** — es gibt keine Zonen, Regeln gelten direkt global oder pro Interface, in der Reihenfolge wie sie hinzugefügt wurden (sichtbar via Regel-Nummer).

| Aufgabe | firewalld | ufw |
|---|---|---|
| Firewall aktivieren | `systemctl enable --now firewalld` | `ufw enable` |
| Alle Regeln anzeigen | `firewall-cmd --list-all` | `ufw status verbose` |
| Regeln mit Nummern anzeigen | (keine direkte Nummerierung, Zonen-basiert) | `ufw status numbered` |
| Dienst dauerhaft erlauben | `firewall-cmd --add-service=nfs --permanent` | `ufw allow nfs` |
| Port dauerhaft öffnen | `firewall-cmd --add-port=80/tcp --permanent` | `ufw allow 80/tcp` |
| Änderung wirksam machen | `firewall-cmd --reload` | (sofort wirksam, kein extra reload) |
| Regel löschen | `firewall-cmd --remove-port=80/tcp --permanent` + `--reload` | `ufw delete <nummer>` |
| Quelle/Subnetz erlauben | `firewall-cmd --add-source=192.168.1.0/24 --permanent` | `ufw allow from 192.168.1.0/24` |
| Alle Zonen anzeigen | `firewall-cmd --list-all-zones` | (Konzept existiert nicht) |
| Grafische Oberfläche | `firewall-config` | `gufw` |
| Explizit verweigern | (Zonen-Policy nutzen) | `ufw deny <dienst/port>` |

## Die zwei wichtigsten Merksätze für die Prüfung

1. **firewalld: `--permanent` allein reicht NICHT** — die Regel ist dann zwar dauerhaft gespeichert, aber erst nach einem zusätzlichen `--reload` (oder Neustart) tatsächlich aktiv. Vergisst man den Reload, wirkt die Regel scheinbar "nicht" — eine klassische Prüfungsfalle.

2. **ufw braucht KEIN extra reload** — Regeln wirken sofort bei Eingabe. Das ist der Hauptunterschied im täglichen Workflow zwischen den beiden Systemen.

## Beide gleichzeitig auf einem System?

Praktisch nie sinnvoll und meist nicht mal möglich — beide würden um die Kontrolle über die zugrundeliegenden netfilter/iptables-Regeln konkurrieren. Pro System läuft genau eines von beiden (je nach Distribution vorinstalliert: ufw bei Ubuntu, firewalld bei RHEL/Fedora/SUSE).
