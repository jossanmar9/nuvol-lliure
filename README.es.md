# Núvol Lliure

Proyecto intermodular de 2.º de CFGS ASIR.

*[Versió en valencià](README.md)*

Núvol Lliure es una nube privada hecha con Nextcloud para Arrel Oberta, una consultoría de unos 50 trabajadores. La empresa tiene un servidor Windows con Active Directory donde cada trabajador tiene su carpeta, pero solo se puede acceder a ella desde la oficina.

La idea es mantener Active Directory para los usuarios y añadir:

- **NUVOL01:** servidor Ubuntu con Nextcloud en Docker, el sistema en un SSD y los datos en cuatro discos en RAID 6.
- **BACKUP01:** servidor para las copias de seguridad (BorgBackup) y la monitorización (Uptime Kuma).

Los trabajadores entran con su usuario de Windows, con MFA, y pueden acceder a los archivos desde la oficina o desde casa.

Antes de instalarlo en los servidores reales, todo se prueba en máquinas virtuales. La solución también sirve para casa: sin Active Directory, Nextcloud funciona con sus propios usuarios.

## Tecnologías

- Ubuntu Server
- Docker y Docker Compose
- Nextcloud, MariaDB y Redis
- Caddy (HTTPS)
- Active Directory (LDAPS)
- mdadm (RAID 6) y ext4
- smartmontools (SMART)
- BorgBackup
- Uptime Kuma
- WireGuard
- VirtualBox (pruebas)

## Carpetas

- `docs/` – memoria del proyecto (valenciano y castellano) e imágenes
- `docker/` – archivos de Docker Compose
- `config/` – archivos de configuración
- `scripts/` – scripts de copias y mantenimiento

La memoria está en [docs/memoria-es.md](docs/memoria-es.md) (castellano) y [docs/memoria.md](docs/memoria.md) (valenciano).

Las contraseñas no se suben al repositorio. Para probarlo hay que copiar `.env.example` a `.env` y poner las contraseñas.

## Estado

- [x] Análisis y diseño
- [x] Planificación
- [ ] Pruebas en máquinas virtuales
- [ ] Instalación de los servidores
- [ ] Active Directory y MFA
- [ ] Copias y monitorización
- [ ] Pruebas y puesta en marcha
- [ ] Documentación final
