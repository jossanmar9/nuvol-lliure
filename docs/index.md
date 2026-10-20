# ☁️ Núvol Lliure

## Cloud privada per a Arrel Oberta

**Núvol Lliure** és una infraestructura de cloud privada basada en
programari lliure, desenvolupada com a Projecte Intermodular del
CFGS d'Administració de Sistemes Informàtics en Xarxa (ASIR).

La plataforma permet emmagatzemar, sincronitzar i compartir informació
de manera segura, mantenint les dades sota el control de l'organització.

---

## 🎯 Objectiu

Implantar una cloud privada segura i accessible des de qualsevol lloc
per a **Arrel Oberta**, una organització d'aproximadament 50 treballadors.

Els usuaris podran accedir als seus fitxers utilitzant els mateixos
comptes d'Active Directory que ja utilitzen als equips Windows.

## 🏗️ Infraestructura

La infraestructura està formada per tres servidors principals:

| Servidor | Sistema | Funció |
|---|---|---|
| **DC01** | Windows Server | Active Directory i DNS |
| **NUVOL01** | Ubuntu Server | Nextcloud i serveis de producció |
| **BACKUP01** | Ubuntu Server | Backups, monitorització i preproducció |

## 🛠️ Tecnologies

Núvol Lliure utilitza principalment:

- Nextcloud
- Docker i Docker Compose
- Ubuntu Server
- Active Directory
- MariaDB
- Redis
- Caddy
- BorgBackup
- Uptime Kuma
- WireGuard
- Git i GitHub

## 📚 Documentació

Utilitza el menú superior per consultar l'arquitectura, la instal·lació,
la seguretat, les proves i la memòria completa del projecte.
