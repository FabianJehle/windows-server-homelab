# Storage Spaces Setup - SERVER01

## Überblick
Auf SERVER01 wurde mittels Storage Spaces ein Storage Pool aus mehreren 
virtuellen Festplatten erstellt, um Redundanz gegen Festplattenausfälle 
zu simulieren.

## Konfiguration

### Storage Pool
- **Name:** LabStoragePool
- **Physische Datenträger:** 5x virtuelle Festplatten à 10 GB
- **Gesamtkapazität:** 47.4 GB
- **Freier Speicher:** 42.2 GB

### Virtual Disk
- **Name:** LabVirtualDisk
- **Resiliency-Typ:** Parity
- **Provisioning:** Thin
- **Kapazität:** 10 GB
- **Volume/Laufwerksbuchstabe:** Z:
- **Write-Back Cache:** 1 GB

## Durchgeführte Schritte
1. 5 virtuelle Festplatten (je 10 GB) zur VM SERVER01 hinzugefügt
2. Storage Pool "LabStoragePool" über Server Manager → File and Storage 
   Services → Storage Pools erstellt, alle 5 Disks hinzugefügt
3. Virtual Disk "LabVirtualDisk" mit Resiliency-Typ **Parity** erstellt
4. Volume formatiert und Laufwerksbuchstabe **Z:** zugewiesen

## Warum Parity?
Parity bietet eine gute Balance zwischen Speichereffizienz und Redundanz: 
im Gegensatz zu Mirroring wird weniger Rohkapazität für Redundanz 
"verschwendet", dafür ist die Schreibperformance etwas geringer – 
für ein Homelab ohne hohe I/O-Anforderungen ein sinnvoller Kompromiss 
gegenüber Simple (keine Redundanz) oder Mirror (mehr Kapazitätsverlust).

## Geplante Erweiterungen
- Freigabe (Share) auf dem Z:-Volume einrichten
- NTFS-Berechtigungen für AD-Gruppen konfigurieren