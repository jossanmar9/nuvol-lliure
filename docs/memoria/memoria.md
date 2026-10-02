# Núvol Lliure

## Cloud privada per a Arrel Oberta

**Projecte Intermodular – 2n CFGS Administració de Sistemes Informàtics en Xarxa (ASIR)**

**Autor:** Jose Vicente Sanjuan Martinez

> 🚧 **Document en desenvolupament**
>
> Esta memòria s'anirà completant i actualitzant durant el desenvolupament del projecte.

---

# 1. PRESENTACIÓ DEL PROJECTE

**Núvol Lliure** és una cloud privada basada en programari lliure que permet guardar, sincronitzar i compartir fitxers sense dependre de serveis externs com Google Drive o OneDrive.

El projecte planteja la implantació de Núvol Lliure en **Arrel Oberta**, una consultoria fictícia d'aproximadament 50 treballadors. L'empresa disposa actualment d'un servidor Windows amb Active Directory que també s'utilitza com a servidor de fitxers, on cada treballador té la seua carpeta individual.

La proposta consisteix a mantindre Active Directory per a la gestió dels usuaris i afegir dos servidors Linux: un per a la cloud privada i un altre per a les còpies de seguretat i la monitorització. Així, els treballadors podran accedir a la seua informació des de l'oficina i des de fora, de manera segura i sincronitzada en tots els seus dispositius.

Sempre que siga possible s'utilitzarà programari lliure i de codi obert, i es buscarà una solució funcional i fàcil de mantindre.

---

# 2. INTRODUCCIÓ

## 2.1 Títol del projecte

**Núvol Lliure – Cloud privada per a Arrel Oberta**

## 2.2 Resum

Arrel Oberta és una consultoria fictícia d'aproximadament 50 treballadors. Actualment, els usuaris es gestionen amb Active Directory en un servidor Windows, que també guarda la carpeta individual de cada treballador. Estes carpetes només són accessibles des de la xarxa de l'oficina, cosa que dificulta el treball fora de l'empresa.

La solució proposada és implantar **Núvol Lliure**, una cloud privada basada en **Nextcloud**, desplegada amb **Docker** sobre **Ubuntu Server**. Els treballadors continuaran iniciant sessió amb el seu usuari d'Active Directory i tindran la seua informació disponible des del navegador, des del client d'escriptori i des del mòbil.

La infraestructura inclourà accés segur per HTTPS, autenticació multifactor, emmagatzematge redundant amb RAID 1, còpies de seguretat en un segon servidor, monitorització dels serveis i scripts per automatitzar les tasques de manteniment.

## 2.3 Justificació

El projecte naix de dues necessitats principals d'Arrel Oberta:

- **Accés des de fora de l'oficina.** Alguns treballadors, especialment l'equip comercial, necessiten consultar i modificar els seus documents quan visiten clients o treballen des de casa. Amb el sistema actual no és possible fer-ho de manera còmoda i segura, i alguns treballadors han començat a utilitzar serveis personals com Google Drive o Dropbox, de manera que informació de l'empresa queda fora del seu control.
- **Protecció de la informació.** El servidor actual no disposa de redundància d'emmagatzematge ni d'una política de còpies de seguretat definida. Una fallada del disc podria provocar la pèrdua de la informació dels treballadors.

Una cloud privada resol estes necessitats mantenint les dades dins de l'empresa i sense costos de llicències per usuari.

Des del punt de vista formatiu, el projecte permet aplicar de manera conjunta continguts treballats durant el cicle d'ASIR: administració de sistemes Linux i Windows, Active Directory, xarxes, contenidors, seguretat, serveis de xarxa, emmagatzematge, còpies de seguretat, monitorització, automatització i documentació tècnica.

A més, s'utilitzaran **Git i GitHub** per controlar les versions de la documentació, les configuracions i els scripts, de manera semblant a com es treballa en un entorn professional.

## 2.4 Objectius

### Objectiu general

Implantar una **cloud privada segura i accessible des de qualsevol lloc** per a Arrel Oberta, que permeta als treballadors guardar, sincronitzar i compartir els seus fitxers utilitzant el seu usuari d'Active Directory.

### Objectius específics

- Desplegar Nextcloud amb Docker sobre un servidor Ubuntu Server.
- Integrar Nextcloud amb Active Directory perquè els usuaris utilitzen el mateix usuari i contrasenya que en Windows.
- Migrar les carpetes individuals dels treballadors des del servidor Windows a Nextcloud.
- Permetre compartir fitxers entre usuaris i departaments amb permisos de lectura o edició.
- Permetre l'accés des de la xarxa interna i des d'Internet de manera segura, amb HTTPS i autenticació multifactor.
- Protegir les dades davant de la fallada d'un disc amb un RAID 1.
- Fer còpies de seguretat automàtiques en un segon servidor i comprovar que es poden restaurar.
- Monitoritzar l'estat dels servidors i dels serveis i rebre avisos quan alguna cosa falle.
- Automatitzar les tasques de manteniment més habituals amb scripts.
- Documentar la instal·lació, l'administració i la recuperació perquè la infraestructura es puga tornar a desplegar.

---

# 3. ANÀLISI DEL PROJECTE

## 3.1 Situació actual

**Arrel Oberta** és una consultoria fictícia d'aproximadament **50 treballadors**, organitzats en cinc departaments:

- Recursos Humans.
- Administració.
- Sistemes.
- Equip Comercial.
- Direcció.

La infraestructura actual es basa en un **servidor Windows** que proporciona:

- **Active Directory i DNS**, per gestionar de manera centralitzada els usuaris, grups i equips de l'empresa.
- **Servidor de fitxers**, on cada treballador té la seua **carpeta individual**, protegida amb permisos NTFS perquè només hi puga accedir el seu propietari.

Quan dos treballadors necessiten intercanviar documents, ho fan per correu electrònic o amb memòries USB, cosa que genera còpies duplicades i versions diferents d'un mateix document.

Les carpetes només són accessibles des de la xarxa de l'oficina. Els treballadors que es desplacen, especialment l'equip comercial, no poden accedir a la seua informació quan estan fora.

Les principals limitacions detectades són:

- No hi ha accés a la informació des de fora de l'oficina.
- No hi ha una manera còmoda i controlada de compartir fitxers entre treballadors.
- Ús de serveis cloud personals per part d'alguns treballadors, amb informació de l'empresa fora del seu control.
- No hi ha redundància d'emmagatzematge: la fallada d'un disc pot provocar la pèrdua de dades.
- No hi ha una política de còpies de seguretat definida ni proves de restauració.
- No hi ha cap sistema que avise quan un servei deixa de funcionar.

## 3.2 Necessitats

### Necessitats funcionals

- Cada treballador ha de tindre un espai personal on guardar els seus fitxers.
- Els fitxers s'han de poder consultar des del navegador i sincronitzar en l'ordinador i el mòbil.
- Els treballadors han de poder compartir fitxers amb companys o amb tot un departament.
- Els usuaris autoritzats han de poder accedir des de dins i des de fora de l'oficina.

### Necessitats tècniques

- La cloud s'ha d'instal·lar en un servidor Linux dedicat, separat del servidor Windows.
- Active Directory ha de continuar sent el sistema on es gestionen els usuaris.
- Cal migrar les carpetes individuals actuals a la nova plataforma.

### Necessitats de seguretat

- Cada usuari ha d'iniciar sessió amb el seu compte i només ha de poder veure la seua informació i la que li han compartit.
- Les connexions des de fora han d'anar xifrades.
- Cal una segona verificació en l'inici de sessió (autenticació multifactor).
- Només s'han d'exposar a Internet els serveis imprescindibles.

### Necessitats de disponibilitat

- La fallada d'un disc no ha de provocar la pèrdua de dades ni aturar el servei.
- Cal fer còpies de seguretat automàtiques, separades del servidor principal, i comprovar que es poden restaurar.
- Cal saber quan un servei deixa de funcionar.

### Necessitats econòmiques i d'escalabilitat

- S'utilitzarà programari lliure per evitar costos de llicències per usuari.
- La solució s'ha de poder ampliar si augmenta el nombre de treballadors o l'espai necessari.

## 3.3 Requisits

| ID | Tipus | Requisit | Prioritat | Com es verificarà |
|---|---|---|---|---|
| RF-01 | Funcional | Cada usuari disposarà d'un espai personal per guardar i gestionar els seus fitxers. | Alta | Crear, modificar, descarregar i eliminar fitxers de prova. |
| RF-02 | Funcional | Els usuaris podran accedir als seus fitxers des de la xarxa interna. | Alta | Accedir des de CLIENT01 dins de la xarxa de l'oficina. |
| RF-03 | Funcional | Els usuaris podran accedir als seus fitxers des d'Internet. | Alta | Accedir des d'un equip extern a la xarxa de l'oficina. |
| RF-04 | Funcional | Els usuaris podran compartir fitxers i carpetes amb altres usuaris o grups. | Alta | Compartir una carpeta i comprovar que el destinatari hi accedeix. |
| RF-05 | Funcional | En compartir, es podrà indicar si el destinatari pot només llegir o també modificar. | Alta | Comprovar que un usuari amb permís de lectura no pot modificar. |
| RF-06 | Funcional | Les carpetes individuals del servidor Windows es migraran a Nextcloud. | Alta | Migrar les carpetes de prova i comprovar que el contingut coincideix. |
| RF-07 | Funcional | Els fitxers es podran sincronitzar en l'ordinador i el mòbil. | Mitjana | Instal·lar el client de Nextcloud i comprovar la sincronització. |
| RT-01 | Tècnic | La cloud s'executarà en un servidor Linux dedicat. | Alta | Comprovar el servidor desplegat. |
| RT-02 | Tècnic | Active Directory continuarà gestionant els usuaris i grups. | Alta | Crear un usuari en AD i comprovar que pot iniciar sessió en Nextcloud. |
| RT-03 | Tècnic | Nextcloud s'integrarà amb Active Directory. | Alta | Iniciar sessió en Nextcloud amb un usuari d'AD. |
| RS-01 | Seguretat | L'accés a la informació requerirà iniciar sessió. | Alta | Intentar accedir sense iniciar sessió. |
| RS-02 | Seguretat | Els usuaris hauran d'utilitzar autenticació multifactor. | Alta | Activar MFA i comprovar que es demana el segon factor. |
| RS-03 | Seguretat | L'accés web es farà per HTTPS. | Alta | Comprovar la connexió HTTPS des del navegador. |
| RS-04 | Seguretat | Un usuari no podrà accedir a fitxers d'altres usuaris si no li els han compartit. | Alta | Intentar accedir a fitxers d'un altre usuari. |
| RD-01 | Disponibilitat | L'emmagatzematge disposarà de redundància. | Alta | Simular la fallada d'un disc i comprovar que les dades continuen disponibles. |
| RD-02 | Disponibilitat | Es faran còpies de seguretat automàtiques en un servidor diferent. | Alta | Comprovar que la còpia nocturna s'ha executat correctament. |
| RD-03 | Disponibilitat | Les còpies permetran recuperar la informació. | Alta | Eliminar un fitxer de prova i restaurar-lo des de la còpia. |
| RM-01 | Monitorització | Es supervisarà l'estat dels servidors i dels serveis principals. | Mitjana | Aturar un servei i comprovar que arriba l'avís. |
| RE-01 | Escalabilitat | Es podrà ampliar la capacitat d'emmagatzematge. | Mitjana | Documentar el procediment d'ampliació. |
| RA-01 | Automatització | S'automatitzarà almenys una tasca de manteniment. | Mitjana | Comprovar l'execució programada de l'script. |
| RDOC-01 | Documentació | Es documentarà la instal·lació, l'administració i la recuperació. | Alta | Seguir la documentació durant les proves finals. |

Els requisits podran revisar-se durant el desenvolupament si apareixen noves necessitats o limitacions.

## 3.4 Anàlisi de solucions existents

### Serveis al núvol públic

La primera alternativa seria contractar un servei com **Google Workspace (Google Drive)** o **Microsoft 365 (OneDrive)**. Són fàcils d'utilitzar i no requereixen servidors propis, però s'han descartat perquè:

- La informació de l'empresa s'emmagatzema en servidors de tercers.
- Tenen un cost mensual per usuari que augmenta amb la plantilla.
- Generen dependència d'un proveïdor concret.

Per això s'opta per una **cloud privada autoallotjada**. S'han estudiat tres plataformes de programari lliure:

### Nextcloud

Plataforma d'emmagatzematge, sincronització i compartició de fitxers amb moltes funcionalitats addicionals mitjançant aplicacions. Disposa d'integració amb LDAP i Active Directory, autenticació multifactor, clients per a tots els sistemes i una comunitat molt gran.

### ownCloud Infinite Scale

Versió moderna d'ownCloud, orientada al rendiment i l'escalabilitat. La seua integració completa amb Active Directory requereix configurar un proveïdor d'identitat addicional, cosa que augmenta la complexitat per a una empresa d'esta grandària.

### Seafile

Plataforma centrada en la sincronització de fitxers, molt ràpida i eficient. Té menys funcionalitats de col·laboració i algunes opcions estan reservades a l'edició de pagament.

### Comparació

| Característica | Nextcloud | ownCloud Infinite Scale | Seafile |
|---|---|---|---|
| Programari lliure | Sí | Sí | Sí (edició comunitària) |
| Sincronització i compartició | Sí | Sí | Sí |
| Integració amb Active Directory | Directa (LDAP) | Amb proveïdor d'identitat | Directa (LDAP) |
| Autenticació multifactor | Integrada | Amb proveïdor d'identitat | Limitada |
| Desplegament amb Docker | Sí | Sí | Sí |
| Documentació i comunitat | Molt àmplia | Mitjana | Àmplia |
| Orientació principal | Cloud i col·laboració | Rendiment i escalabilitat | Sincronització de fitxers |

La selecció definitiva es justifica en l'apartat 4.4.

## 3.5 Anàlisi DAFO

| Fortaleses | Debilitats |
|---|---|
| Les dades es queden dins de l'empresa. | Cal administrar i mantindre la infraestructura pròpia. |
| Sense costos de llicències per usuari. | Més responsabilitat per al departament de Sistemes. |
| Es reutilitzen els usuaris i grups d'Active Directory. | Cal migrar les dades del servidor actual. |
| Accés des de qualsevol lloc i dispositiu. | Els usuaris s'han d'adaptar a la nova eina. |
| Redundància, còpies i monitorització des del principi. | Cal personal amb coneixements de Linux i Docker. |

| Oportunitats | Amenaces |
|---|---|
| Facilitar el teletreball i el treball amb clients. | Atacs contra els serveis accessibles des d'Internet. |
| Eliminar l'ús de serveis cloud personals. | Vulnerabilitats en el programari utilitzat. |
| Ampliar la plataforma amb noves aplicacions en el futur. | Avaries de maquinari. |
| Créixer al mateix ritme que l'empresa. | Errors de configuració. |
| Modernitzar la manera de treballar. | Dependència de la connexió a Internet per a l'accés remot. |

L'anàlisi mostra que la cloud privada resol les limitacions actuals, però trasllada a l'empresa la responsabilitat de mantindre-la i protegir-la. Per això el disseny posa especial atenció en la seguretat, les còpies de seguretat i la monitorització.

---

# 4. DISSENY DE LA SOLUCIÓ

En este apartat es defineix l'arquitectura de Núvol Lliure i es justifiquen les tecnologies seleccionades a partir de les necessitats i requisits de l'apartat 3.

El projecte s'implantarà en un **entorn de laboratori virtualitzat** que reprodueix la infraestructura d'Arrel Oberta. Quan és rellevant, s'indica també com s'aplicaria la solució en un entorn real.

## 4.1 Arquitectura general

La infraestructura manté el servidor Windows actual i hi afegeix dos servidors Linux:

- **DC01 (Windows Server):** continua proporcionant Active Directory i DNS. És on es gestionen els usuaris i grups de l'empresa.
- **NUVOL01 (Ubuntu Server):** nou servidor que allotja la cloud privada (Nextcloud en contenidors Docker) i l'emmagatzematge redundant.
- **BACKUP01 (Ubuntu Server):** segon servidor, més senzill, que guarda les còpies de seguretat i allotja la monitorització. Com que és una màquina separada, si NUVOL01 falla les còpies continuen disponibles i la incidència es detecta igualment.

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

Els usuaris accedeixen sempre a la mateixa adreça (`https://nuvol.arreloberta.com`), tant des de l'oficina com des de fora. L'administració dels servidors (SSH) només és accessible des de la xarxa interna o a través de la VPN.

## 4.2 Entorn de laboratori

La infraestructura es muntarà amb **VirtualBox** en un portàtil. VirtualBox és gratuït, multiplataforma i conegut, i permet afegir discos virtuals per provar el RAID i fer instantànies abans de cada canvi important.

| Màquina | Sistema operatiu | vCPU | RAM | Discos | Funció |
|---|---|---|---|---|---|
| DC01 | Windows Server 2022 (avaluació) | 2 | 4 GB | 50 GB | Active Directory, DNS |
| NUVOL01 | Ubuntu Server 26.04 LTS | 2 | 4 GB | 25 GB (sistema) + 2 × 20 GB (RAID 1) | Cloud privada |
| BACKUP01 | Ubuntu Server 26.04 LTS | 1 | 2 GB | 20 GB (sistema) + 60 GB (còpies) | Còpies de seguretat i monitorització |
| CLIENT01 | Windows 10/11 | 2 | 4 GB | 50 GB | Equip d'un treballador unit al domini |

Les màquines es connectaran a una **Xarxa NAT** de VirtualBox, que fa de xarxa de l'empresa. El reenviament de ports de la Xarxa NAT simula el router: les peticions que arriben al portàtil es reenvien a NUVOL01, de manera que des del mateix portàtil (o des d'un altre equip) es pot provar l'accés extern.

No cal tindre totes les màquines enceses alhora: per a la majoria de proves n'hi ha prou amb DC01 i NUVOL01, i BACKUP01 només es necessita per a les proves de còpies i monitorització.

## 4.3 Xarxa i adreçament

| Element | Valor |
|---|---|
| Xarxa LAN | 192.168.10.0/24 |
| Porta d'enllaç (router / Xarxa NAT) | 192.168.10.1 |
| DC01 | 192.168.10.10 |
| NUVOL01 | 192.168.10.20 |
| BACKUP01 | 192.168.10.30 |
| Clients | DHCP |
| Xarxa VPN (WireGuard) | 10.10.10.0/24 |
| Domini públic de l'empresa | `arreloberta.com` |
| Domini d'Active Directory | `corp.arreloberta.com` |
| Adreça de la cloud | `nuvol.arreloberta.com` |

> Els noms de domini són fictics i s'utilitzen només com a exemple per al projecte.

El domini d'Active Directory és un subdomini del domini públic (`corp.`), tal com recomana Microsoft, per evitar conflictes entre els noms interns i els públics.

En el DNS d'Active Directory es crearan els registres dels servidors Linux i un registre `nuvol.arreloberta.com` que apunte a l'adreça interna de NUVOL01. Així, des de l'oficina els equips accedeixen directament al servidor i des de fora accedeixen a través de l'adreça pública del router.

## 4.4 Plataforma de cloud privada

### Selecció de la plataforma

Per triar la plataforma s'han valorat les tres alternatives de l'apartat 3.4 amb una matriu de decisió ponderada (puntuació de 1 a 5):

| Criteri | Pes | Nextcloud | ownCloud Infinite Scale | Seafile |
|---|---|---|---|---|
| Integració amb Active Directory (RT-03) | 25 % | 5 | 3 | 4 |
| Documentació i comunitat | 20 % | 5 | 3 | 4 |
| Autenticació multifactor (RS-02) | 15 % | 5 | 3 | 3 |
| Facilitat d'administració | 15 % | 4 | 3 | 4 |
| Funcionalitats (compartició, clients, aplicacions) | 15 % | 5 | 4 | 3 |
| Rendiment | 10 % | 3 | 5 | 5 |
| **Puntuació ponderada** | **100 %** | **4,65** | **3,35** | **3,80** |

**Es selecciona Nextcloud** perquè:

- S'integra directament amb Active Directory mitjançant l'aplicació *LDAP user and group backend*.
- Inclou autenticació multifactor (TOTP) i permet fer-la obligatòria.
- Té clients per a Windows, Android i iOS per sincronitzar els fitxers.
- Disposa d'una documentació molt completa i d'una comunitat molt activa, cosa que en facilita l'administració.

El seu rendiment és inferior al de les altres opcions, però no és un problema per a 50 usuaris, sobretot si s'utilitza Redis com a memòria cau.

### Desplegament

Nextcloud es desplegarà amb **Docker Compose** utilitzant les imatges oficials. Tota la configuració queda en un únic fitxer `docker-compose.yml` versionat en el repositori, i les contrasenyes en un fitxer `.env` fora del repositori.

| Contenidor | Funció |
|---|---|
| `caddy` | Proxy invers amb HTTPS |
| `nextcloud` | Aplicació Nextcloud |
| `cron` | Tasques en segon pla de Nextcloud |
| `mariadb` | Base de dades |
| `redis` | Memòria cau i bloqueig de fitxers |

Només el contenidor de Caddy publicarà ports (80 i 443). La base de dades i Redis no seran accessibles des de fora de Docker.

**Base de dades:** se selecciona **MariaDB**, que és l'opció més utilitzada en la documentació i la comunitat de Nextcloud. SQLite es descarta perquè no és adequat per a diversos usuaris alhora.

**Proxy invers i HTTPS:** se selecciona **Caddy** perquè configura HTTPS automàticament amb molt poques línies de configuració, en comparació amb Nginx o Traefik. Com que el domini és fictici, en el laboratori Caddy utilitzarà la seua pròpia autoritat de certificació (`tls internal`) i el certificat arrel s'importarà als equips client. En un entorn real, amb un domini registrat, Caddy obtindria automàticament un certificat de Let's Encrypt.

## 4.5 Integració amb Active Directory

Nextcloud es connectarà a DC01 per **LDAPS (port 636)**, de manera que les credencials viatgen xifrades. Els usuaris inicien sessió amb el **mateix usuari i contrasenya que en Windows**, i les altes, baixes i canvis de contrasenya es continuen fent únicament en Active Directory.

| Paràmetre | Valor |
|---|---|
| Servidor | `dc01.corp.arreloberta.com` (port 636) |
| Compte de connexió | `svc-nextcloud` (compte de servei només de lectura) |
| Usuaris amb accés | Membres del grup `GG_Nuvol_Usuaris` |
| Nom d'usuari | `sAMAccountName` (el mateix que en Windows) |

En Active Directory es crearan els grups següents:

| Grup | Ús |
|---|---|
| `GG_Nuvol_Usuaris` | Usuaris que poden accedir a la cloud |
| `GG_Nuvol_Admins` | Administradors de Nextcloud |
| `GG_RRHH`, `GG_Administracio`, `GG_Sistemes`, `GG_Comercial`, `GG_Direccio` | Un grup per departament, per poder compartir fitxers amb tot un departament |

Quan un treballador deixa l'empresa i se'n deshabilita el compte en AD, deixa de poder accedir a la cloud.

## 4.6 Organització de la informació

Actualment cada treballador té **la seua carpeta individual** en el servidor Windows. En Nextcloud, cada usuari disposarà del seu **espai personal**, on tindrà la seua informació disponible des de qualsevol lloc i sincronitzada en els seus dispositius.

- **Accés:** cada usuari només veu els seus fitxers i els que altres usuaris han compartit amb ell (RS-04).
- **Quota:** 10 GB per usuari, ampliable segons les necessitats.
- **Compartició:** un usuari pot compartir fitxers o carpetes amb altres usuaris o amb un grup de departament, indicant si poden només llegir o també modificar (RF-04, RF-05).
- **Enllaços externs:** per compartir amb persones de fora de l'empresa, només es permetran enllaços amb contrasenya i data de caducitat.
- **Versions i paperera:** Nextcloud guarda versions anteriors dels fitxers i una paperera, de manera que l'usuari pot recuperar un fitxer modificat o eliminat per error sense ajuda del departament de Sistemes.

## 4.7 Emmagatzematge i redundància

Les dades de Nextcloud s'emmagatzemaran en dos discos configurats en **RAID 1 amb mdadm**:

```text
  /dev/sdb ─┐
            ├─► /dev/md0 (RAID 1) ─► LVM ─► ext4 ─► /srv/nuvol
  /dev/sdc ─┘
```

- **RAID 1 (mdadm):** els dos discos contenen la mateixa informació. Si un falla, el servei continua funcionant amb l'altre fins que es substitueix. És el RAID per programari estàndard de Linux i no necessita maquinari específic.
- **LVM:** permet ampliar l'espai en el futur afegint nous discos (RE-01).
- **ext4:** sistema de fitxers estable i conegut.

S'ha descartat ZFS perquè, tot i oferir més funcionalitats, és més complex d'administrar i no és necessari per a esta grandària d'empresa.

> [!IMPORTANT]
> El RAID protegeix davant de la fallada d'un disc, però **no és una còpia de seguretat**: si un usuari elimina un fitxer o un virus el xifra, el canvi es copia immediatament als dos discos. Per això es fan còpies de seguretat independents (apartat 4.8).

## 4.8 Còpies de seguretat

La política de còpies segueix la **regla 3-2-1**: tres còpies de les dades, en dos suports diferents i una fora de l'oficina.

| Còpia | Ubicació |
|---|---|
| 1 | Dades en el RAID 1 de NUVOL01 |
| 2 | Servidor BACKUP01 |
| 3 | Disc extern que es guarda fora de l'oficina |

Guardar les còpies en un segon servidor fa que una fallada greu de NUVOL01 (avaria, error de configuració o virus) no afecte també les còpies.

Per fer les còpies s'utilitzarà **BorgBackup**, una eina lliure que fa còpies incrementals (només copia els canvis), comprimeix i xifra les dades i elimina automàticament les còpies antigues. S'ha preferit a una còpia simple amb `rsync` perquè permet guardar diverses versions sense multiplicar l'espai ocupat.

Un script (`scripts/backup/`) s'executarà cada nit a les 02:00 amb `cron` i farà:

1. Posar Nextcloud en mode manteniment.
2. Exportar la base de dades (`mariadb-dump`).
3. Copiar amb Borg les dades, la configuració i l'exportació de la base de dades a BACKUP01 per SSH, amb autenticació per clau i un usuari dedicat que només pot executar Borg.
4. Traure Nextcloud del mode manteniment.
5. Esborrar les còpies antigues, conservant **7 diàries, 4 setmanals i 6 mensuals**.

Periòdicament es copiarà el repositori de BACKUP01 al disc extern.

Es farà també una còpia de l'estat del sistema de DC01 amb *Windows Server Backup*, ja que sense Active Directory els usuaris no podrien iniciar sessió.

**Proves de restauració (RD-03):** s'eliminarà un fitxer de prova i es restaurarà des de la còpia, i es documentarà el procediment per restaurar el servei complet en un servidor nou.

## 4.9 Monitorització

Se selecciona **Uptime Kuma**, una eina lliure i senzilla que s'executa en un contenidor i es configura des del navegador. S'instal·larà en **BACKUP01** perquè, si NUVOL01 deixa de funcionar, el sistema de monitorització continue actiu i puga avisar. Altres opcions com Zabbix o Prometheus amb Grafana són més completes, però també molt més complexes de configurar del que necessita una empresa d'esta grandària.

Uptime Kuma comprovarà periòdicament:

| Element | Comprovació |
|---|---|
| Nextcloud | La web respon per HTTPS |
| Certificat | Avís abans que caduque |
| NUVOL01 | Respon al ping i el port SSH està obert |
| DC01 | Respon al ping i el port LDAPS (636) està obert |

Quan un servei deixe de respondre, s'enviarà un avís per correu electrònic. A més, l'script de còpia avisarà si la còpia falla, i `mdadm` es configurarà per enviar un correu si un dels discos del RAID falla.

RM-01 es verificarà aturant un servei de manera controlada i comprovant que arriba l'avís.

## 4.10 Seguretat

| Àmbit | Mesures |
|---|---|
| Accés extern | El router només reenvia el port 443 (HTTPS) i el 51820/udp (VPN). |
| Administració | SSH i panells d'administració només des de la xarxa interna o per **VPN WireGuard**. |
| Servidors | Tallafocs UFW, SSH amb claus i sense accés de `root`, actualitzacions de seguretat automàtiques. |
| Nextcloud | MFA (TOTP) obligatori, protecció contra força bruta integrada i enllaços externs amb contrasenya. |
| Comunicacions | HTTPS per a l'accés web i LDAPS entre Nextcloud i AD. |
| Còpies | Repositori Borg xifrat i usuari dedicat en BACKUP01 que només accepta la clau de NUVOL01. |
| Contrasenyes | Fitxer `.env` amb permisos restringits i exclòs del repositori. |

## 4.11 Automatització

Es crearan scripts Bash, guardats en la carpeta `scripts/` del repositori:

- **Còpia de seguretat:** executada automàticament cada nit amb `cron` (RA-01).
- **Restauració:** per recuperar les dades a partir d'una còpia.
- **Actualització:** per actualitzar els contenidors de manera controlada.

## 4.12 Migració de les dades

Les carpetes individuals del servidor Windows es migraran a l'espai personal de cada usuari en Nextcloud:

1. Configurar Nextcloud perquè el nom intern de cada usuari siga el seu `sAMAccountName`, de manera que les carpetes tinguen el mateix nom que en Windows.
2. Fer que cada usuari inicie sessió una primera vegada perquè es cree el seu espai (o crear-los per línia de comandes).
3. Muntar la carpeta compartida del servidor Windows en NUVOL01 i copiar la carpeta de cada usuari al seu espai amb `rsync`.
4. Executar `occ files:scan --all` perquè Nextcloud reconega els fitxers copiats.
5. Comprovar que el nombre de fitxers i l'espai ocupat coincideixen amb l'origen.
6. Deixar les carpetes antigues en només lectura durant un temps abans de retirar-les.

## 4.13 Resum de tecnologies

| Àmbit | Tecnologia |
|---|---|
| Virtualització (laboratori) | VirtualBox |
| Gestió d'usuaris | Active Directory (Windows Server) |
| Servidors Linux (NUVOL01, BACKUP01) | Ubuntu Server 26.04 LTS |
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

## 4.14 Relació entre requisits i solució

| Requisit | Solució | Apartat |
|---|---|---|
| RF-01 | Nextcloud en NUVOL01 | 4.4 |
| RF-02 | Registre DNS intern cap a NUVOL01 | 4.3 |
| RF-03 | Accés HTTPS des de fora a través del router | 4.1, 4.10 |
| RF-04 | Compartició amb usuaris i grups | 4.6 |
| RF-05 | Permisos de lectura o edició en cada compartició | 4.6 |
| RF-06 | Migració de les carpetes individuals amb `rsync` | 4.12 |
| RF-07 | Client d'escriptori i aplicacions mòbils de Nextcloud | 4.4 |
| RT-01 | NUVOL01 amb Ubuntu Server | 4.2 |
| RT-02 | AD continua gestionant usuaris i grups | 4.5 |
| RT-03 | Integració LDAPS | 4.5 |
| RS-01 | Inici de sessió amb compte d'AD | 4.5 |
| RS-02 | MFA TOTP | 4.10 |
| RS-03 | HTTPS amb Caddy | 4.4 |
| RS-04 | Espais personals i compartició controlada | 4.6 |
| RD-01 | RAID 1 amb mdadm | 4.7 |
| RD-02 | BorgBackup cap a BACKUP01 i disc extern | 4.8 |
| RD-03 | Proves de restauració | 4.8 |
| RM-01 | Uptime Kuma i avisos de mdadm | 4.9 |
| RE-01 | LVM | 4.7 |
| RA-01 | Script de còpia amb `cron` | 4.11 |
| RDOC-01 | Documentació en el repositori | 4.11 |

---

# 5. PLANIFICACIÓ

> ⏳ **Pendent de desenvolupament**

---

# 6. IMPLANTACIÓ I PROVES

> ⏳ **Pendent de desenvolupament**

---

# 7. SEGUIMENT I CONTROL

> ⏳ **Pendent de desenvolupament**

---

# 8. RESULTATS I CONCLUSIONS

> ⏳ **Pendent de desenvolupament**

---

# 9. BIBLIOGRAFIA, WEBGRAFIA I FONTS

Fonts consultades per a l'anàlisi i el disseny de la solució. Tots els recursos en línia s'han consultat l'octubre de 2026. La llista s'ampliarà durant la implantació.

## 9.1 Plataformes de cloud privada

- Nextcloud GmbH. *Nextcloud Server Administration Manual*. https://docs.nextcloud.com/server/stable/admin_manual/
- Nextcloud GmbH. *System requirements*. https://docs.nextcloud.com/server/stable/admin_manual/installation/system_requirements.html
- Nextcloud GmbH. *User authentication with LDAP*. https://docs.nextcloud.com/server/stable/admin_manual/configuration_user/user_auth_ldap.html
- Nextcloud GmbH. *Two-factor authentication*. https://docs.nextcloud.com/server/stable/admin_manual/configuration_user/two_factor-auth.html
- Nextcloud GmbH. *Backup*. https://docs.nextcloud.com/server/stable/admin_manual/maintenance/backup.html
- Nextcloud. *Imatge oficial de Docker de Nextcloud*. https://github.com/nextcloud/docker
- ownCloud. *Infinite Scale Admin Documentation*. https://doc.owncloud.com/ocis/next/admin/index.html
- Seafile Ltd. *Seafile Admin Manual*. https://manual.seafile.com/

## 9.2 Sistemes operatius i identitats

- Canonical. *Ubuntu 26.04 LTS release notes*. https://documentation.ubuntu.com/release-notes/26.04/
- Canonical. *Ubuntu Server documentation*. https://documentation.ubuntu.com/server/
- Microsoft. *Selecting the Forest Root Domain*. https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/selecting-the-forest-root-domain
- Microsoft. *Evaluation Center (Windows Server)*. https://www.microsoft.com/en-us/evalcenter/

## 9.3 Contenidors i serveis

- Docker Inc. *Docker Docs*. https://docs.docker.com/
- Docker Inc. *Docker Compose*. https://docs.docker.com/compose/
- Docker Hub. *Imatge oficial de MariaDB*. https://hub.docker.com/_/mariadb
- Docker Hub. *Imatge oficial de Redis*. https://hub.docker.com/_/redis
- Caddy. *Caddy Documentation*. https://caddyserver.com/docs/
- Caddy. *Directiva tls (certificats interns)*. https://caddyserver.com/docs/caddyfile/directives/tls
- Let's Encrypt. *Documentation*. https://letsencrypt.org/docs/

## 9.4 Emmagatzematge, còpies i monitorització

- Linux man-pages. *mdadm(8)*. https://man7.org/linux/man-pages/man8/mdadm.8.html
- Linux man-pages. *lvm(8)*. https://man7.org/linux/man-pages/man8/lvm.8.html
- BorgBackup. *BorgBackup Documentation*. https://borgbackup.readthedocs.io/
- INCIBE. *Copias de seguridad: una guía de aproximación para el empresario*. https://www.incibe.es/empresas/blog/copias-seguridad-guia-aproximacion-el-empresario
- Uptime Kuma. *Repositori oficial*. https://github.com/louislam/uptime-kuma

## 9.5 Xarxa, seguretat i virtualització

- WireGuard. *WireGuard: fast, modern, secure VPN tunnel*. https://www.wireguard.com/
- Ubuntu Community. *UFW*. https://help.ubuntu.com/community/UFW
- Oracle. *VirtualBox User Manual*. https://www.virtualbox.org/manual/

## 9.6 Control de versions

- Git. *Git Documentation*. https://git-scm.com/doc
- GitHub. *GitHub Docs*. https://docs.github.com/

---

# 10. ANNEXOS

> ⏳ **Pendent de desenvolupament**

---

# 11. DOCUMENTACIÓ TÈCNICA

Durant el desenvolupament del projecte es prepararà la documentació necessària per a la instal·lació, administració, ús i recuperació de Núvol Lliure.

> ⏳ **Pendent de desenvolupament**

---

# 12. PRESENTACIÓ I DEFENSA

> ⏳ **Pendent de desenvolupament**