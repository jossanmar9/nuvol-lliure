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

El projecte planteja la seua implantació en **Arrel Oberta**, una consultoria fictícia d'aproximadament **50 treballadors**. L'empresa disposa d'un servidor Windows amb **Active Directory** que també fa de servidor de fitxers, on cada treballador té la seua carpeta individual, accessible només des de l'oficina.

Núvol Lliure manté Active Directory per gestionar els usuaris i afegeix dos servidors **Linux**: un per a la cloud privada, basada en **Nextcloud** i **Docker**, i un altre per a les còpies de seguretat i la monitorització. Els treballadors poden accedir a la seua informació des de l'oficina i des de fora, amb el mateix usuari que utilitzen en Windows.

## 🎯 Objectius

- Desplegar Nextcloud amb Docker sobre Ubuntu Server.
- Integrar Nextcloud amb Active Directory.
- Migrar les carpetes individuals dels treballadors a Nextcloud.
- Permetre compartir fitxers entre usuaris i departaments.
- Proporcionar accés segur des de la xarxa interna i des d'Internet, amb HTTPS i autenticació multifactor.
- Protegir les dades amb un RAID 1.
- Fer còpies de seguretat automàtiques en un segon servidor i comprovar-ne la restauració.
- Monitoritzar els servidors i els serveis.
- Automatitzar les tasques de manteniment amb scripts.
- Documentar la instal·lació, l'administració i la recuperació.

## 🏗️ Arquitectura

```text
                                  INTERNET
                                      │
                              ┌───────┴────────┐
                              │ Router de      │  Ports reenviats a NUVOL01:
                              │ l'empresa      │  · 443/tcp   (HTTPS)
                              └───────┬────────┘  · 51820/udp (VPN WireGuard)
                                      │
                           LAN 192.168.10.0/24
       ┌──────────────────┬───────────┴──────┬──────────────────┐
       │                  │                  │                  │
┌──────┴────────┐ ┌───────┴───────────┐ ┌────┴─────────────┐ ┌──┴─────────┐
│ DC01          │ │ NUVOL01           │ │ BACKUP01         │ │ CLIENT01   │
│ Windows Server│ │ Ubuntu Server     │ │ Ubuntu Server    │ │ Windows    │
│ · AD DS       │ │ · Docker Compose  │ │ · Repositori     │ │ · Navegador│
│ · DNS         │◄┤   - Caddy (HTTPS) │ │   BorgBackup     │ │ · Client   │
│               │ │   - Nextcloud     ├─►   (còpies)       │ │  Nextcloud │
│               │ │   - MariaDB/Redis │ │ · Monitorització │ │            │
│               │ │ · RAID 1 (mdadm)  │ │   (Uptime Kuma)  │ │            │
└───────────────┘ └───────────────────┘ └──────────────────┘ └────────────┘
```

| Servidor | IP | Funció |
|---|---|---|
| DC01 | 192.168.10.10 | Active Directory i DNS |
| NUVOL01 | 192.168.10.20 | Cloud privada (Nextcloud) |
| BACKUP01 | 192.168.10.30 | Còpies de seguretat i monitorització |

La cloud és accessible en `https://nuvol.arreloberta.com` (domini fictici). L'entorn de laboratori es munta amb màquines virtuals de VirtualBox.

## 🛠️ Tecnologies

| Àmbit | Tecnologia |
|---|---|
| Virtualització (laboratori) | VirtualBox |
| Gestió d'usuaris | Active Directory (Windows Server) |
| Servidors Linux | Ubuntu Server 26.04 LTS |
| Contenidors | Docker + Docker Compose |
| Plataforma de cloud | Nextcloud |
| Base de dades i memòria cau | MariaDB + Redis |
| Proxy invers i HTTPS | Caddy |
| Redundància | RAID 1 (mdadm) + LVM |
| Còpies de seguretat | BorgBackup |
| Monitorització | Uptime Kuma |
| Accés d'administració | WireGuard |
| Automatització | Scripts Bash + cron |
| Control de versions | Git + GitHub |

La justificació de cada tecnologia es troba en l'apartat 4 de la memòria.

## 📁 Estructura del repositori

```text
cloud-privada-asir/
├── config/                   # Fitxers de configuració (Caddy, WireGuard...)
├── docker/                   # Fitxers docker-compose dels serveis
├── docs/
│   ├── arquitectura/         # Diagrames de la infraestructura
│   ├── imatges/              # Imatges de la documentació
│   ├── installacio/          # Guies d'instal·lació
│   ├── memoria/              # Memòria del projecte
│   │   └── memoria.md
│   ├── proves/               # Plans i resultats de proves
│   └── seguretat/            # Documentació de seguretat
├── scripts/
│   ├── backup/               # Scripts de còpies de seguretat
│   ├── manteniment/          # Scripts de manteniment
│   └── restore/              # Scripts de restauració
├── .env.example
├── .gitignore
└── README.md
```

## 📖 Documentació

La memòria del projecte es troba en [`docs/memoria/memoria.md`](docs/memoria/memoria.md).

Apartats completats:

- Presentació del projecte.
- Introducció.
- Anàlisi del projecte (situació actual, necessitats, requisits, solucions existents i DAFO).
- Disseny de la solució.

## 🔐 Dades sensibles

> [!IMPORTANT]
> Les contrasenyes, claus privades, certificats i altres dades sensibles **no es guarden al repositori**.

Les variables necessàries es defineixen en un fitxer `.env` a partir de la plantilla [`.env.example`](.env.example), que no conté credencials reals.

## 🚧 Estat del projecte

> 🟡 **En desenvolupament** – fase de planificació de la implantació.

- [x] Definició del projecte
- [x] Anàlisi de la situació actual, necessitats i requisits
- [x] Anàlisi de solucions i DAFO
- [x] Disseny de la solució
- [ ] Planificació
- [ ] Muntatge de l'entorn de laboratori
- [ ] Desplegament de Nextcloud
- [ ] Integració amb Active Directory
- [ ] Seguretat (HTTPS, MFA, VPN)
- [ ] RAID 1
- [ ] Còpies de seguretat i restauració
- [ ] Monitorització
- [ ] Scripts d'automatització
- [ ] Migració de dades
- [ ] Proves
- [ ] Documentació tècnica
- [ ] Finalització de la memòria i presentació
