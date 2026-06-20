# Dienst-Namen vs. Paket-Namen vs. Befehlsnamen

Eine der häufigsten Fallen: Paketname, systemd-Servicename und das eigentliche Programm heißen oft **alle drei unterschiedlich** — selbst innerhalb derselben Distribution.

| Dienst | Paketname (apt) | systemd-Servicename | Hauptprogramm/Daemon | Config-Datei |
|---|---|---|---|---|
| DNS-Server | `bind9` | `named` (manchmal `bind9`) | `/usr/sbin/named` | `/etc/bind/named.conf*` |
| Webserver (Ubuntu) | `apache2` | `apache2` | `apache2` | `/etc/apache2/` |
| Webserver (RHEL) | `httpd` | `httpd` | `httpd` | `/etc/httpd/conf/httpd.conf` |
| NFS-Server (Ubuntu) | `nfs-kernel-server` | `nfs-server` | — | `/etc/exports` |
| NFS-Server (RHEL) | `nfs-utils` | `nfs-server` | — | `/etc/exports` |
| DHCP-Server (Ubuntu) | `isc-dhcp-server` | `isc-dhcp-server` | `dhcpd` | `/etc/dhcp/dhcpd.conf` |
| DHCP-Server (RHEL) | `dhcp-server` | `dhcpd` | `dhcpd` | `/etc/dhcp/dhcpd.conf` |
| Firewall (Ubuntu) | `ufw` | `ufw` | — | `/etc/ufw/` |
| Firewall (RHEL) | `firewalld` | `firewalld` | — | `firewall-cmd` zur Laufzeit |
| iSCSI Target (Ubuntu) | `tgt` | `tgt` | `tgtd` | `/etc/tgt/conf.d/` |
| iSCSI Initiator | `open-iscsi` (Ubuntu) / `iscsi-initiator-utils` (RHEL) | `iscsid` | `iscsid` | `/etc/iscsi/` |

## Merksätze

- **DNS ist der Klassiker:** Paket heißt `bind9`, aber der Dienst/Prozess heißt `named` — wenn eine Prüfungsfrage `systemctl status bind9` zeigt und das nicht funktioniert, ist `named` der gesuchte richtige Name (je nach Ubuntu-Version kann aber auch ein Alias existieren).
- **DHCP-Server-Paketname unterscheidet sich nach Distribution** (`isc-dhcp-server` vs. `dhcp-server`), der Daemon selbst heißt aber überall `dhcpd`.
- Bei Unsicherheit: `systemctl status <vermuteter-name>` probieren, oder `dpkg -L paketname | grep systemd` (Debian) bzw. `rpm -ql paketname | grep systemd` (RHEL) um den exakten Unit-Namen zu finden, den ein Paket mitbringt.
