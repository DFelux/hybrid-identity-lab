# Hybrid Identity Lab – On-Prem AD DS ↔ Microsoft Entra ID

Erweiterung meines bestehenden Homelabs um eine hybride Identitätsanbindung zwischen
lokalem Active Directory und Microsoft Entra ID. Ziel: praktische Umsetzung der
AZ-104-Domäne "Identities & Governance" auf realer Infrastruktur, nicht nur in der
Theorie.

> Teil der [it-infrastructure-lab](https://github.com/DFelux/it-infrastructure-lab) Reihe.

## Architektur

```
┌─────────────────────────┐         ┌──────────────────────────┐
│   On-Premises (Lab)     │         │      Microsoft Entra ID  │
│                         │         │                          │
│  Windows Server 2022    │  Sync   │   Entra ID Tenant        │
│  AD DS (homelab.local)  │───────▶│   - Synced Users/Groups  │
│  192.168.56.10          │ Entra   │   - Conditional Access   │
│                         │ Connect │   - MFA                  │
│  Windows 11 Client      │         │                          │
│  192.168.56.20          │ Hybrid  │                          │
│  (Hybrid Join)          │◀──────▶│                          │
└─────────────────────────┘         └──────────────────────────┘
```

## Ziele

- [ ] Microsoft Entra Connect Sync aufsetzen (PHS oder PTA – Entscheidung dokumentiert)
- [ ] Entra Hybrid Join für Windows-11-Client
- [ ] Conditional-Access-Policy (MFA-Enforcement)
- [ ] RBAC-Vergleich lokale AD-Gruppen vs. Entra-ID-Rollen
- [ ] Tagging- und Lock-Konzept auf Ressourcengruppen-Ebene
- [ ] Troubleshooting-Runbook für Sync-Fehler

## Warum dieses Projekt

Bestehende Homelab-Komponenten (AD DS, Zammad-Ticketsystem) decken On-Premises-Themen
ab. Dieses Projekt schließt gezielt die Lücke zu Cloud-Identity-Management, das im
AZ-104-Examen einen der größten Gewichtsanteile hat.

## Setup

### Voraussetzungen

- Azure-Subscription (Free/Dev Tier ausreichend)
- Bestehendes AD DS Lab (siehe [it-infrastructure-lab](https://github.com/DFelux/it-infrastructure-lab))
- Windows Server mit .NET Framework 4.7.2+ für Entra Connect

### Installation (Kurzfassung)

```powershell
# Entra Connect herunterladen und installieren
# Custom Installation wählen für Sync-Regel-Kontrolle
.\AzureADConnect.exe
```

Detaillierte Schritte folgen in `docs/setup.md` (in Arbeit).

## Entscheidung: PHS vs. PTA

| Kriterium | Password Hash Sync | Pass-through Authentication |
|---|---|---|
| Ausfallsicherheit bei Azure-Störung | Höher (Hash liegt in Cloud) | Abhängig von Agent/On-Prem |
| Infrastruktur-Overhead | Gering | PTA-Agent-Server nötig |
| Bevorzugt für | Kleine/mittlere Umgebungen | Umgebungen mit strikten On-Prem-Policies |

*Begründung der finalen Wahl wird nach Abschluss von Phase 1 ergänzt.*

## Troubleshooting-Log

Dokumentierte Probleme und Lösungen folgen hier, sobald Phase 5 abgeschlossen ist
(bewusst herbeigeführter Sync-Fehler + Behebung).

## Stack

`Windows Server 2022` · `AD DS` · `Microsoft Entra Connect` · `Entra ID` ·
`Conditional Access` · `PowerShell`

## Status

🚧 In Aufbau – Begleitprojekt zur AZ-104-Vorbereitung.

## Bezug zu AZ-104

Dieses Projekt bildet praktisch folgende Skill-Areas der AZ-104-Prüfung ab:
- Manage Azure identities and governance
- Implement and manage governance (Tags, Locks)
- Monitor and maintain Azure resources (Sync-Health, Troubleshooting)
