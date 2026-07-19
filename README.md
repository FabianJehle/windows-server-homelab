# Windows Server 2025 Homelab

Dieses Repository dokumentiert den Aufbau und die Konfiguration eines 
Windows Server 2025 Homelabs als praktische Lernumgebung für 
Systemadministration und IT-Infrastruktur.

## Ziel des Projekts

Selbstständiger Aufbau einer typischen Windows-Server-Infrastruktur zur 
Vertiefung praktischer Kenntnisse in Active Directory, DNS, DHCP und 
Gruppenrichtlinien – als Vorbereitung auf den Einstieg in die IT, 
sei es über eine Ausbildung im Bereich Fachinformatik oder eine 
Einstiegsposition im First Level Support.

## Verwendete Technologien

- Windows Server 2025
- Windows 11 Pro (Client)
- VMware Workstation Pro
- Active Directory Domain Services (AD DS)
- DNS (Forward & Reverse Lookup Zones)
- DHCP
- GPO
- File Server Role
- PowerShell

## Struktur

- [`active-directory/`](./active-directory) – Domänenstruktur, OUs, Benutzer- und Gruppenverwaltung
- [`dns/`](./dns) – DNS-Zonenkonfiguration
- [`dhcp/`](./dhcp) – DHCP-Server-Setup und IP-Verwaltung
- [`storage-spaces/`](./storage-spaces) – Storage Pool
- [`file-server-role/`](./file-server-role) – File Server Role
## Learnings

Jedes Teilprojekt enthält einen eigenen Abschnitt zu aufgetretenen 
Problemen und deren Lösung – siehe jeweilige README für Details.

## Status

🚧 In aktiver Entwicklung
