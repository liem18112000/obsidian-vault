---
ai_hash: 89c887a9a788af89
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.73
entities: []
relevance: 0.731
source: https://axonivy.atlassian.net/wiki/spaces/IO/pages/49092722690/Option+3+All-in-One+Script
space: IO
status: reference
tags:
- confluence
- programming
- space/io
title: 'Option 3: All-in-One Script'
topic: programming
type: source
updated: 2026-01-28
---

# Option 3: All-in-One Script

> [!info] Imported from Confluence
> Space **IO** · updated 2026-01-28 · [open original](https://axonivy.atlassian.net/wiki/spaces/IO/pages/49092722690/Option+3+All-in-One+Script)
> Relevance 0.731 · topic `programming`

# Option 3: All-in-One Script

**Konzept:**  
Wie Option 2, aber als Bash-Script (Linux/macOS) bzw. PowerShell-Script (Windows). Setzt voraus, dass der User die benötigten Tools installiert hat.

**Was wir bereitstellen:**

- `vault-key-input.sh` (Bash) und `vault-key-input.ps1` (PowerShell)
- Anleitung zur Tool-Installation

**Was der Key-Inhaber installieren muss:**

- Vault CLI
- SSH-Client
- GPG

**Was der Key-Inhaber tun muss:**

- Script ausführen, Key eingeben

## Verschlüsselung

<div>

|                 |                    |
|-----------------|--------------------|
| Strecke         | Verschlüsselung    |
| User → Jumphost | SSH                |
| User → Vault    | TLS (Ende-zu-Ende) |

</div>

**Wo ist der Key unverschlüsselt?**

- Nur im RAM des User-Rechners
- Auf der Leitung: Nie

**Wer sieht den Key?**

- Nur der User selbst
- Wir (Admins) sehen den Key **nie**

**Sicherheits-Score: ⭐⭐⭐⭐**

**Nachteil:**  
User muss Tools selbst installieren (Vault, SSH, GPG)

## Infrastruktur-Details

- **Server vs. Kubernetes:** VM nötig (SSH Port 22 ist kritischer Port)
- **TLS-Zertifikate:** Keine separate Verwaltung (User's SSH-Client)
- **User-Verwaltung:** Google IAM mit OS Login oder SSH-User manuell

## Aufwandseinschätzung: 3-4 PT

<div>

|                                                |         |
|------------------------------------------------|---------|
| Schritt                                        | PT      |
| *Infrastruktur*                                | \-      |
| Organisatorisch (Bitbucket, Terraform Backend) | 0,1     |
| Planung und Einrichtung GCP Projekt            | 0,25    |
| Planung und Aufsetzen Netzwerk Infrastruktur   | 0,5     |
| Einrichten Jumphost VM                         | 0,25    |
| Test Infrastruktur                             | 0,25    |
| *Script-Entwicklung*                           | \-      |
| Bash-Script Entwicklung                        | 0,5     |
| PowerShell-Script Entwicklung                  | 0,5     |
| Dokumentation für User                         | 0,25    |
| Testing (beide Plattformen)                    | 0,5     |
| **Ohne VPN**                                   | **3,1** |
| *+ Aufsetzen und Einrichten VPN (optional)*    | *1,0*   |
| **Mit VPN**                                    | **4,1** |

</div>

%% ai-graph-start %%

**Related notes:**
- [[Introduction of Hashicorp Vault]]
- [[luz-vault - Recovery key encryption with RSA Public Keys (draft - vault operator]]
- [[luz-vault - How to run Vault Benchmark]]
- [[How to unseal Vault Unseal]]
- [[Vault overview]]

%% ai-graph-end %%