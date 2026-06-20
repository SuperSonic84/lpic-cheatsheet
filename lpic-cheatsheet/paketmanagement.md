# Paketmanagement: Debian/Ubuntu vs. RHEL/Fedora/CentOS vs. openSUSE

| Zweck | Debian / Ubuntu | RHEL / Fedora / CentOS | openSUSE |
|---|---|---|---|
| Paket installieren | `apt install httpd` | `dnf install httpd` | `zypper install httpd` |
| Webserver-Paketname | `apache2` | `httpd` | `apache2` |
| Datei → Paket | `dpkg -S /pfad` | `rpm -qf /pfad` | `rpm -qf /pfad` |
| Paket → alle Dateien | `dpkg -L paket` | `rpm -ql paket` | `rpm -ql paket` |
| Repo-Index aktualisieren | `apt update` | (automatisch bei dnf) | `zypper refresh` |
| Repo-Konfig Ort | `/etc/apt/sources.list` | `/etc/yum.repos.d/` | zypper-Repos |
| Paket entfernen | `apt remove paket` | `dnf remove paket` | `zypper remove paket` |
| Paket + Config entfernen | `apt purge paket` | `dnf remove paket` (entfernt meist alles) | `zypper remove paket` |
| Paketinfo anzeigen | `apt show paket` / `dpkg -s paket` | `dnf info paket` / `rpm -qi paket` | `zypper info paket` |
| Nach Datei suchen (welches Paket enthält...) | `apt-file search /pfad` | `dnf provides /pfad` | `zypper se --provides /pfad` |
| Lokale .deb/.rpm Datei installieren | `dpkg -i paket.deb` | `rpm -ih paket.rpm` | `rpm -ih paket.rpm` |
| Firewall-Tool | `ufw` | `firewall-cmd` | `firewall-cmd` |
| GRUB-Konfig generieren | `update-grub` | `grub2-mkconfig -o /boot/grub2/grub.cfg` | `grub2-mkconfig -o /boot/grub2/grub.cfg` |
| GRUB-Config Pfad | `/boot/grub/grub.cfg` | `/boot/grub2/grub.cfg` | `/boot/grub2/grub.cfg` |
| Standard-Editor (minimal) | `nano` | `vi` / `vim` | `vi` / `vim` |
| DHCP-Server-Paket | `isc-dhcp-server` | `dhcp-server` | `dhcp-server` |
| NFS-Server-Paket | `nfs-kernel-server` | `nfs-utils` | `nfs-kernel-server` |

## Merksätze

- **Debian-Welt** (apt/dpkg) hat oft "freundlichere", eingedeutschte/englische Paketnamen → `apache2`, `nano`
- **RPM-Welt** (RHEL/Fedora/CentOS) nutzt oft die "originalen" Upstream-Programmnamen → `httpd`, `vi`
- **openSUSE ist ein Sonderfall:** nutzt `zypper` als High-Level-Tool, aber technisch trotzdem `rpm` darunter — deshalb sind `rpm -qf`/`rpm -ql` bei SUSE identisch zu RHEL, obwohl der Installationsbefehl (`zypper`) anders heißt als bei RHEL (`dnf`).
