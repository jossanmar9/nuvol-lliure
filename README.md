# Núvol Lliure

Projecte intermodular de 2n de CFGS ASIR.

*[Versión en castellano](README.es.md)*

Núvol Lliure és una cloud privada feta amb Nextcloud per a Arrel Oberta, una consultoria d'uns 50 treballadors. L'empresa té un servidor Windows amb Active Directory on cada treballador té la seua carpeta, però només s'hi pot accedir des de l'oficina.

La idea és mantindre Active Directory per als usuaris i afegir:

- **NUVOL01:** servidor Ubuntu amb Nextcloud en Docker, el sistema en un SSD i les dades en quatre discos en RAID 6.
- **BACKUP01:** servidor per a les còpies de seguretat (BorgBackup) i la monitorització (Uptime Kuma).

Els treballadors entren amb el seu usuari de Windows, amb MFA, i poden accedir als fitxers des de l'oficina o des de casa.

Abans d'instal·lar-ho en els servidors reals, tot es prova en màquines virtuals. La solució també serveix per a casa: sense Active Directory, Nextcloud funciona amb els seus propis usuaris.

## Tecnologies

- Ubuntu Server
- Docker i Docker Compose
- Nextcloud, MariaDB i Redis
- Caddy (HTTPS)
- Active Directory (LDAPS)
- mdadm (RAID 6) i ext4
- smartmontools (SMART)
- BorgBackup
- Uptime Kuma
- WireGuard
- VirtualBox (proves)

## Carpetes

- `docs/` – memòria del projecte (valencià i castellà) i imatges
- `docker/` – fitxers de Docker Compose
- `config/` – fitxers de configuració
- `scripts/` – scripts de còpies i manteniment

La memòria està en [docs/memoria.md](docs/memoria.md) (valencià) i [docs/memoria-es.md](docs/memoria-es.md) (castellà).

Les contrasenyes no es pugen al repositori. Per provar-ho cal copiar `.env.example` a `.env` i posar les contrasenyes.

## Estat

- [x] Anàlisi i disseny
- [x] Planificació
- [ ] Proves en màquines virtuals
- [ ] Instal·lació dels servidors
- [ ] Active Directory i MFA
- [ ] Còpies i monitorització
- [ ] Proves i posada en marxa
- [ ] Documentació final
