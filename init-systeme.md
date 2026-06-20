# Init-Systeme: systemd vs. SysV Init

| Zweck | systemd (modern) | SysV Init (klassisch) |
|---|---|---|
| Dienst starten | `systemctl start apache2` | `service apache2 start` |
| Dienst stoppen | `systemctl stop apache2` | `service apache2 stop` |
| Status prüfen | `systemctl status apache2` | `service apache2 status` |
| Dienst neu starten | `systemctl restart apache2` | `service apache2 restart` |
| Config ohne Neustart neu laden | `systemctl reload apache2` | `service apache2 reload` |
| Autostart aktivieren | `systemctl enable apache2` | `chkconfig apache2 on` |
| Autostart deaktivieren | `systemctl disable apache2` | `chkconfig apache2 off` |
| Aktivieren + sofort starten | `systemctl enable --now apache2` | (zwei separate Befehle nötig) |
| Runlevel/Target live wechseln | `systemctl isolate multi-user.target` | `init 3` / `telinit 3` |
| Standard-Runlevel/Target setzen | `systemctl set-default multi-user.target` | Eintrag in `/etc/inittab` |
| Standard-Runlevel/Target anzeigen | `systemctl get-default` | `runlevel` |
| Unit-/Skript-Ort (eigene/Override) | `/etc/systemd/system/` | `/etc/init.d/` |
| Logs ansehen | `journalctl -u apache2` | `/var/log/messages` lesen |
| System neu starten | `systemctl reboot` | `shutdown -r now` |
| System herunterfahren | `systemctl poweroff` | `shutdown -h now` |
| "Halt"-Ziel | `poweroff.target` (= Runlevel 0) | Runlevel 0 |
| "Multi-User Text"-Ziel | `multi-user.target` (= Runlevel 3) | Runlevel 3 |
| "Multi-User GUI"-Ziel | `graphical.target` (= Runlevel 5) | Runlevel 5 |
| "Reboot"-Ziel | `reboot.target` (= Runlevel 6) | Runlevel 6 |
| Unit-Datei lesen | `systemctl cat apache2` | Skript direkt in `/etc/init.d/` öffnen |
| Alle aktiven Dienste anzeigen | `systemctl list-units --type service` | `service --status-all` |

## Merksätze

- **systemd Targets entsprechen den klassischen Runleveln**, sind aber keine einzelnen Schalter, sondern **Abhängigkeitsketten** (z.B. `graphical.target` benötigt `multi-user.target`, welches wiederum `basic.target` benötigt).
- **`service` als Befehl existiert auch unter systemd noch** als Kompatibilitäts-Wrapper auf vielen Distributionen — er ruft im Hintergrund trotzdem `systemctl` auf. Für die Prüfung trotzdem beide "echten" Familien kennen.
- **`/etc/init.d/`-Skripte und `/etc/systemd/system/`-Unit-Dateien können auf hybriden Systemen parallel existieren** — systemd bevorzugt die eigene Unit-Datei, fällt aber auf das init.d-Skript zurück, falls keine existiert.
