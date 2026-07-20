# File-Server-Rolle

## Übersicht

Die File-Server-Rolle stellt Dateien/Ordner zentral im Netzwerk bereit (SMB-Freigaben mit Berechtigungen für Benutzer/Gruppen). Es gelten zwei Berechtigungsebenen – Freigabeberechtigung und NTFS-Berechtigung –, wobei die jeweils restriktivere gilt.

## Installation

Server Manager → **Manage → Add Roles and Features** → Server Roles:

```text
File and Storage Services → File and iSCSI Services → File Server
```

Ist meist bereits vorinstalliert.

## Freigegebenen Ordner erstellen

1. Ordner anlegen, z. B. `Z:\projekte`
2. Server Manager → **File and Storage Services → Shares → Tasks → New Share**
3. Profil **SMB Share – Quick**, Volume/Ordner wählen
4. Freigabename: `projekte` → Netzwerkpfad `\\SERVER01\projekte`
5. Unter **Permissions → Customize permissions**: Gruppe `CONTOSO\Domain Users` mit NTFS-Rechten `Read & execute`, `List folder contents`, `Read`, `Write` hinzufügen

> **Hinweis:** In der Praxis besser eine differenzierte Gruppenstruktur (z. B. `projekte-lesen`/`projekte-schreiben`) statt pauschal `Domain Users` mit Schreibzugriff auszustatten (Least Privilege).

## Access-based Enumeration (ABE)

Zeigt Benutzern nur Objekte, auf die sie Zugriff haben (statt „Access Denied" beim Öffnen):

```powershell
Set-SmbShare -Name projekte -FolderEnumerationMode AccessBased
Get-SmbShare -Name projekte | Select-Object Name, FolderEnumerationMode
```

## Freigabe prüfen

```powershell
Get-SmbShare
Get-SmbShareAccess -Name projekte
Get-Acl Z:\projekte | Format-List
```

## Testen (von SERVER02)

```powershell
Test-Path \\SERVER01\projekte   # → True
```
Danach Testdatei erstellen, um Schreibzugriff zu bestätigen.

## Netzlaufwerk verbinden (manuell)

Explorer: **This PC → Map network drive** → Laufwerksbuchstabe (z. B. `Z:`) → Pfad `\\SERVER01\projekte` → optional **Reconnect at sign-in**.

Per PowerShell:

```powershell
net use Z: \\SERVER01\projekte /user:CONTOSO\Benutzername *
```

(Im Test möglichst mit normalem Domänenbenutzer statt Administrator arbeiten.)

## Netzlaufwerk per GPO verbinden

1. **Group Policy Management** → GPO erstellen/bearbeiten, mit OU verknüpfen (z. B. `Remote Workers`)
2. **User Configuration → Preferences → Windows Settings → Drive Maps → New → Mapped Drive**
3. Einstellungen:

```text
Action: Create
Location: \\SERVER01\projekte
Drive Letter: P
Label: Projekte
Reconnect: aktiviert
```

4. Optional **Item-level targeting** für gezielte Benutzer/Gruppen
5. Update per `gpupdate /force` oder automatischem Refresh

Vorteil: neue Benutzer der OU erhalten das Laufwerk automatisch bei Anmeldung, ohne manuelle Client-Konfiguration.

## Fehleranalyse

```powershell
nslookup SERVER01
ping SERVER01
Test-NetConnection SERVER01 -Port 445
Test-Path \\SERVER01\projekte
Get-SmbShare
Get-SmbShareAccess -Name projekte
Get-Acl Z:\projekte | Format-List
```

In meinem Fall: DNS, Netzwerk und SMB-Port erreichbar, Zugriff scheiterte aber wegen lokalem Konto auf SERVER02. Nach Anmeldung mit Domänenkonto erfolgreich:

```powershell
net use P: \\SERVER01\projekte /user:CONTOSO\Administrator *
```

## Ergebnis

- SMB-Freigabe `projekte` auf `SERVER01` eingerichtet, Domänenbenutzer können lesen/erstellen/bearbeiten/löschen
- ABE aktiviert (nur sichtbare, berechtigte Objekte)
- Netzlaufwerk manuell (`Z:`) und Konzept für automatisiertes Drive-Mapping per GPO dokumentiert

# Screenshots
![SharedFolerServer01](Screenshots/SharedFolderServer01.png)
![SharedFolerServer02](Screenshots/SharedFolderServer02.png)
![GPODriveMap](Screenshots/GPODriveMap.png)
![DriveMapConfig](Screenshots/DriveMapConfig.png)
![Workstation](Screenshots/Workstation01.png)
