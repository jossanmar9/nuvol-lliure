<p align="center">
  <img src="docs/imatges/logo.png" alt="Logo Núvol Lliure" width="300">
</p>

<h1 align="center">☁️ Núvol Lliure</h1>

<p align="center">
  <strong>Cloud privada per a Arrel Oberta</strong>
</p>

<p align="center">
  Projecte Intermodular de <strong>2n curs del Cicle Formatiu de Grau Superior
  d'Administració de Sistemes Informàtics en Xarxa (ASIR)</strong>.
</p>

---

## 📋 Descripció

**Núvol Lliure** és una cloud privada basada en programari lliure que permet guardar, sincronitzar i compartir fitxers sense dependre de serveis externs com Google Drive o OneDrive.

El projecte planteja la seua implantació en **Arrel Oberta**, una consultoria d'aproximadament **50 treballadors**. L'empresa disposa d'un servidor Windows amb **Active Directory** que també fa de servidor de fitxers, on cada treballador té la seua carpeta individual, accessible només des de l'oficina.

Núvol Lliure manté Active Directory per gestionar els usuaris i afegeix dos servidors **Linux**: un per a la cloud privada, basada en **Nextcloud** i **Docker**, i un altre per a les còpies de seguretat, la monitorització i l'entorn de preproducció.

Els treballadors podran accedir a la seua informació des de l'oficina i des de fora, utilitzant el mateix compte que ja utilitzen en Windows.

## 🎯 Objectius

- Desplegar Nextcloud amb Docker sobre Ubuntu Server.
- Integrar Nextcloud amb Active Directory.
- Validar la solució en un entorn de preproducció abans del desplegament a producció.
- Migrar les carpetes individuals dels treballadors a Nextcloud, fora d'horari i amb un pla de marxa arrere.
- Permetre compartir fitxers entre usuaris i departaments.
- Proporcionar accés segur des de la xarxa interna i des d'Internet, amb HTTPS i autenticació multifactor.
- Protegir les dades davant de la fallada d'un disc mitjançant RAID 1.
- Fer còpies de seguretat automàtiques en un segon servidor i comprovar-ne la restauració.
- Monitoritzar els servidors i els serveis principals.
- Automatitzar tasques de manteniment mitjançant scripts.
- Documentar la instal·lació, administració i recuperació de la infraestructura.

## 🏗️ Arquitectura

```text
                                  INTERNET
                                      │
                              ┌───────┴────────┐
                              │ Router de      │
                              │ l'empresa      │
                              └───────┬────────┘
                                      │
                           LAN 192.168.10.0/24
       ┌──────────────────┬───────────┴──────┬──────────────────┐
       │                  │                  │                  │
┌──────┴────────┐ ┌───────┴───────────┐ ┌────┴─────────────┐ ┌──┴─────────┐
│ DC01          │ │ NUVOL01           │ │ BACKUP01         │ │ Equips     │
│ Windows Server│ │ PRODUCCIÓ         │ │ Ubuntu Server    │ │ Windows 11 │
│               │ │ Ubuntu Server     │ │                  │ │            │
│ · AD DS       │ │ · Docker Compose  │ │ · BorgBackup     │ │ · Navegador│
│ · DNS         │◄┤ · Nextcloud       ├─► · Uptime Kuma    │ │ · Client   │
│ · AD CS       │ │ · MariaDB         │ │ · Preproducció   │ │  Nextcloud │
│               │ │ · Redis           │ │                  │ │ · Mòbils   │
│               │ │ · Caddy           │ │                  │ │            │
│               │ │ · RAID 1          │ │                  │ │            │
└───────────────┘ └───────────────────┘ └──────────────────┘ └────────────┘
```

### Servidors

| Servidor | IP | Funció |
|---|---|---|
| **DC01** | `192.168.10.10` | Active Directory, DNS i AD CS |
| **NUVOL01** | `192.168.10.20` | Cloud privada – producció |
| **BACKUP01** | `192.168.10.30` | Còpies de seguretat, monitorització i preproducció |

L'entorn de **preproducció** permet provar configuracions, actualitzacions i scripts abans d'aplicar-los sobre el servidor de producció.

## 🛠️ Tecnologies

| Àmbit | Tecnologia |
|---|---|
| Gestió d'usuaris | Active Directory |
| Servidors Linux | Ubuntu Server |
| Contenidors | Docker + Docker Compose |
| Cloud privada | Nextcloud |
| Base de dades | MariaDB |
| Memòria cau | Redis |
| Proxy invers i HTTPS | Caddy |
| Certificats públics | Let's Encrypt |
| Certificats interns | Active Directory Certificate Services |
| Redundància | RAID 1 amb `mdadm` + LVM |
| Còpies de seguretat | BorgBackup |
| Monitorització | Uptime Kuma |
| VPN d'administració | WireGuard |
| Firewall | UFW |
| Automatització | Bash + cron |
| Control de versions | Git + GitHub |

La selecció i justificació de les diferents tecnologies es desenvolupa en l'apartat 4 de la memòria.

## 🔄 Flux de desplegament

Els canvis sobre la infraestructura seguiran un procés de validació abans d'arribar a producció:

```text
Configuració / canvi
        │
        ▼
       Git
        │
        ▼
  Preproducció
        │
        ▼
      Proves
        │
        ▼
  Validació correcta?
      │       │
     No       Sí
      │       │
      ▼       ▼
  Correcció  Producció
```

D'esta manera, les actualitzacions, configuracions i scripts es provaran primer en un entorn separat abans d'aplicar-los al servei utilitzat pels treballadors.

## 📁 Estructura del repositori

```text
nuvol-lliure/
├── config/                   # Fitxers de configuració
├── docker/                   # Configuració de Docker Compose
├── docs/
│   ├── arquitectura/         # Diagrames i disseny de la infraestructura
│   ├── imatges/              # Imatges utilitzades en la documentació
│   ├── installacio/          # Documentació d'instal·lació
│   ├── memoria/
│   │   └── memoria.md        # Memòria del projecte
│   ├── proves/               # Plans i resultats de proves
│   └── seguretat/            # Documentació de seguretat
├── scripts/
│   ├── backup/               # Scripts de còpies de seguretat
│   ├── manteniment/          # Scripts de manteniment
│   └── restore/              # Scripts de restauració
├── .env.example              # Plantilla de variables d'entorn
├── .gitignore
└── README.md
```

L'estructura podrà evolucionar durant la implantació del projecte segons les necessitats que apareguen.

## 📖 Documentació

La memòria completa del projecte es troba en:

[`docs/memoria/memoria.md`](docs/memoria/memoria.md)

La memòria segueix l'estructura establida per al Projecte Intermodular:

1. Presentació del projecte.
2. Introducció.
3. Anàlisi del projecte.
4. Disseny de la solució.
5. Planificació.
6. Implantació i proves.
7. Seguiment i control.
8. Resultats i conclusions.
9. Bibliografia/webgrafia i fonts.
10. Annexos.
11. Documentació tècnica.
12. Presentació i defensa.

## 🔐 Seguretat i dades sensibles

> [!IMPORTANT]
> Les contrasenyes, claus privades, certificats, tokens i altres dades sensibles **no es guarden al repositori**.

Les variables necessàries per als serveis es definiran en un fitxer `.env` local a partir de la plantilla [`.env.example`](.env.example).

El fitxer `.env` real quedarà exclòs del control de versions mitjançant `.gitignore`.

## 🔄 Control de versions

El projecte utilitza **Git** com a sistema de control de versions i **GitHub** com a repositori remot.

S'utilitza per a:

- Mantindre un historial dels canvis.
- Versionar configuracions i scripts.
- Mantindre la documentació del projecte.
- Provar en preproducció la mateixa configuració que posteriorment s'utilitzarà en producció.
- Poder identificar i recuperar versions anteriors.

## 🚧 Estat del projecte

> 🟡 **En desenvolupament – planificació completada; pendent la implantació.**

- [x] Definició del projecte
- [x] Anàlisi de la situació actual
- [x] Identificació de necessitats i requisits
- [x] Anàlisi de solucions i DAFO
- [x] Disseny de la solució
- [x] Selecció i justificació de tecnologies
- [x] Planificació
- [ ] Preparació de la infraestructura
- [ ] Desplegament en preproducció
- [ ] Integració amb Active Directory
- [ ] Configuració d'HTTPS i MFA
- [ ] Configuració de la VPN
- [ ] Configuració i prova del RAID 1
- [ ] Còpies de seguretat i proves de restauració
- [ ] Monitorització
- [ ] Scripts d'automatització
- [ ] Pla de proves en preproducció
- [ ] Desplegament a producció
- [ ] Prova pilot
- [ ] Migració de dades
- [ ] Documentació tècnica
- [ ] Finalització de la memòria
- [ ] Presentació i defensa

## 📄 Llicència de la documentació

La documentació de **Núvol Lliure** es distribueix sota la llicència
**Creative Commons Reconeixement-CompartirIgual 4.0 Internacional (CC BY-SA 4.0)**.

© 2026 Jose Vicente Sanjuan Martinez

La llicència s'aplica a la documentació del directori `docs/`. Les tecnologies i aplicacions de tercers utilitzades pel projecte mantenen les seues respectives llicències.
- [ ] Desplegament a producció i verificació
- [ ] Prova pilot i migració de dades
- [ ] Documentació tècnica
- [ ] Finalització de la memòria i presentació
