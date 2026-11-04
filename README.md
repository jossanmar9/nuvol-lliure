# Núvol Lliure

Projecte intermodular de 2n de CFGS ASIR.

Núvol Lliure és una cloud privada feta amb Nextcloud per a Arrel Oberta, una consultoria d'uns 50 treballadors. L'empresa té un servidor Windows amb Active Directory on cada treballador té la seua carpeta, però només s'hi pot accedir des de l'oficina.

La idea és mantindre Active Directory per als usuaris i afegir:

- **NUVOL01:** servidor Ubuntu amb Nextcloud en Docker i dos discos en RAID 1.
- **BACKUP01:** servidor per a les còpies de seguretat (BorgBackup), la monitorització (Uptime Kuma) i un entorn de proves.

Els treballadors entren amb el seu usuari de Windows, amb MFA, i poden accedir als fitxers des de l'oficina o des de casa.

## Tecnologies

- Ubuntu Server
- Docker i Docker Compose
- Nextcloud, MariaDB i Redis
- Caddy (HTTPS)
- Active Directory (LDAPS)
- mdadm (RAID 1)
- BorgBackup
- Uptime Kuma
- WireGuard

## Carpetes

- `docs/` – memòria del projecte i imatges
- `docker/` – fitxers de Docker Compose
- `config/` – fitxers de configuració
- `scripts/` – scripts de còpies i manteniment

La memòria està en [docs/memoria.md](docs/memoria.md).

Les contrasenyes no es pugen al repositori. Per provar-ho cal copiar `.env.example` a `.env` i posar les contrasenyes.

## Estat

- [x] Anàlisi i disseny
- [x] Planificació
- [ ] Instal·lació dels servidors
- [ ] Nextcloud en l'entorn de proves
- [ ] Active Directory i MFA
- [ ] Còpies i monitorització
- [ ] Proves i pas a producció
- [ ] Documentació final
