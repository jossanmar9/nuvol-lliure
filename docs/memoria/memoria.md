# Núvol Lliure

## Cloud privada per a Arrel Oberta

**Projecte Intermodular – 2n curs (ASIR)**

| | |
|---|---|
| **Títol del projecte** | Núvol Lliure – Cloud privada per a Arrel Oberta |
| **Alumne** | Jose Vicente Sanjuan Martinez |
| **Curs** | 2n CFGS Administració de Sistemes Informàtics en Xarxa (ASIR) |
| **Professor/a** | [Pendent] |
| **Data** | [Pendent] |

> 🚧 **Document en desenvolupament**
>
> Esta memòria s'anirà completant i actualitzant durant el desenvolupament del projecte.

---

# ÍNDEX

1. Presentació del projecte
2. Introducció
3. Anàlisi del projecte
4. Disseny de la solució
5. Planificació
6. Implantació i proves
7. Seguiment i control
8. Resultats i conclusions
9. Bibliografia/webgrafia i fonts
10. Annexos
11. Documentació tècnica
12. Presentació i defensa

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

El resultat esperat és una cloud privada funcional i demostrable, on cada treballador d'Arrel Oberta pot treballar amb els seus fitxers des de qualsevol lloc, compartir-los amb els companys i recuperar-los en cas d'error o avaria, amb un cost de llicències nul i amb tota la informació sota el control de l'empresa.

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
- Tenen un cost mensual per usuari que augmenta amb la plantilla: uns 14 dòlars per usuari i mes en els plans Business Standard de Google Workspace i Microsoft 365, és a dir, uns 8.400 dòlars anuals per a 50 treballadors.
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
| Cost de llicències | Gratuït (subscripció de suport opcional) | Gratuït (subscripció de suport opcional) | Gratuït en l'edició comunitària; edició Pro de pagament |
| Orientació principal | Cloud i col·laboració | Rendiment i escalabilitat | Sincronització de fitxers |

La selecció definitiva es justifica en l'apartat 4.3.

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

En este apartat es defineix com serà Núvol Lliure i es justifiquen les tecnologies triades a partir de les necessitats i requisits de l'apartat 3.

El projecte s'implantarà en un **entorn de laboratori virtualitzat** que reprodueix la infraestructura d'Arrel Oberta. Quan és rellevant, s'indica també com s'aplicaria la solució en un entorn real.

## 4.1 Recopilació d'informació tècnica

Per dissenyar la solució s'han consultat principalment les documentacions oficials de cada tecnologia (vegeu l'apartat 9). Les tecnologies investigades són:

- **Nextcloud:** plataforma de cloud privada. S'ha revisat el manual d'administració, especialment els requisits del sistema, la integració amb LDAP/Active Directory, l'autenticació en dos factors i el procediment de còpies de seguretat.
- **ownCloud Infinite Scale i Seafile:** alternatives a Nextcloud, estudiades per poder comparar-les (apartat 4.3).
- **Ubuntu Server 26.04 LTS:** sistema operatiu dels servidors Linux. És una versió amb suport a llarg termini, cosa que garanteix actualitzacions de seguretat durant anys.
- **Docker i Docker Compose:** permeten desplegar Nextcloud i els serveis que necessita (base de dades, memòria cau i proxy) en contenidors, definits en un únic fitxer que es pot versionar i tornar a desplegar.
- **MariaDB i Redis:** base de dades i memòria cau recomanades per la documentació de Nextcloud.
- **Caddy:** proxy invers que configura HTTPS de manera automàtica.
- **Active Directory:** s'ha revisat la recomanació de Microsoft sobre el nom del domini i el funcionament de LDAPS, que necessita un certificat en el controlador de domini.
- **mdadm i LVM:** RAID per programari i gestió de volums de Linux.
- **BorgBackup:** eina de còpies de seguretat incrementals, xifrades i amb deduplicació.
- **Uptime Kuma:** eina senzilla de monitorització de serveis.
- **WireGuard:** VPN moderna i senzilla per a l'accés d'administració.
- **VirtualBox:** hipervisor per muntar l'entorn de laboratori.

## 4.2 Arquitectura

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

**Elements:**

- **DC01 (Windows Server):** el servidor actual. Continua proporcionant Active Directory i DNS, i és on es gestionen els usuaris i grups.
- **NUVOL01 (Ubuntu Server):** nou servidor que allotja Nextcloud en contenidors Docker i les dades en un RAID 1.
- **BACKUP01 (Ubuntu Server):** segon servidor, més senzill, que guarda les còpies de seguretat i allotja la monitorització. Com que és una màquina separada, si NUVOL01 falla les còpies continuen disponibles i la incidència es detecta igualment.
- **CLIENT01 (Windows):** equip d'un treballador unit al domini.
- **Router:** connecta l'oficina a Internet i reenvia a NUVOL01 només els ports de HTTPS i de la VPN.

**Fluxos principals:**

1. **Accés d'un usuari:** l'usuari entra en `https://nuvol.arreloberta.com`. Des de l'oficina, el DNS d'AD el porta directament a NUVOL01; des de fora, arriba a través del router. Caddy rep la connexió HTTPS i la passa a Nextcloud.
2. **Autenticació:** Nextcloud comprova l'usuari i la contrasenya contra Active Directory per LDAPS i, a continuació, demana el segon factor (MFA).
3. **Còpies:** cada nit NUVOL01 envia una còpia de les dades i de la base de dades a BACKUP01 per SSH.
4. **Monitorització:** Uptime Kuma, des de BACKUP01, comprova periòdicament que els serveis responen i avisa per correu si alguna cosa falla.
5. **Administració:** l'administrador accedeix per SSH des de la xarxa interna o, des de fora, a través de la VPN WireGuard.

## 4.3 Estudi de viabilitat tècnica

**Viabilitat tècnica.** Totes les tecnologies triades són programari lliure, madur i àmpliament documentat, i funcionen sobre maquinari convencional. El laboratori complet necessita uns 14 GB de RAM si s'encenen totes les màquines alhora, però per a la major part de les proves n'hi ha prou amb DC01 i NUVOL01, de manera que es pot muntar en un portàtil. Per a 50 usuaris, Nextcloud no necessita un servidor especialment potent: amb 4 nuclis, 8–16 GB de RAM i discos en RAID n'hi ha prou en un entorn real.

**Viabilitat econòmica.** No cal pagar llicències de programari per a la cloud: Nextcloud, Ubuntu Server, Docker i la resta d'eines són gratuïtes. El cost principal és el maquinari (servidor i discos) i el temps d'administració. En comparació, un servei com Google Workspace Business Standard o Microsoft 365 Business Standard costa al voltant de 14 dòlars per usuari i mes, és a dir, uns **8.400 dòlars a l'any** per a 50 treballadors. El pressupost detallat es presenta en l'apartat 5.4.

### 4.3.1 Alternatives

Les alternatives públiques (Google Drive, OneDrive) es van descartar en l'apartat 3.4. Per a la plataforma de cloud privada es consideren tres alternatives viables:

- **Alternativa A – Nextcloud:** plataforma completa de cloud i col·laboració, amb integració directa amb Active Directory.
- **Alternativa B – ownCloud Infinite Scale:** plataforma moderna orientada al rendiment, que necessita un proveïdor d'identitat addicional per integrar-se completament amb AD.
- **Alternativa C – Seafile:** plataforma centrada en la sincronització de fitxers, molt eficient però amb menys funcionalitats en l'edició comunitària.

### 4.3.2 Comparativa

Les alternatives s'han valorat amb una matriu de decisió ponderada (puntuació de 1 a 5):

| Criteri | Pes | A. Nextcloud | B. ownCloud Infinite Scale | C. Seafile | Observacions |
|---|---|---|---|---|---|
| Integració amb Active Directory | 25 % | 5 | 3 | 4 | oCIS necessita un proveïdor d'identitat addicional |
| Documentació i comunitat | 20 % | 5 | 3 | 4 | Nextcloud té la comunitat més gran |
| Autenticació multifactor | 15 % | 5 | 3 | 3 | Nextcloud la inclou i permet fer-la obligatòria |
| Facilitat d'administració | 15 % | 4 | 3 | 4 | oCIS té una arquitectura més complexa |
| Funcionalitats | 15 % | 5 | 4 | 3 | Compartició, clients i aplicacions |
| Rendiment | 10 % | 3 | 5 | 5 | Nextcloud millora amb Redis |
| **Puntuació ponderada** | **100 %** | **4,65** | **3,35** | **3,80** | |

### 4.3.3 Elecció i justificació

**Es tria Nextcloud** perquè obté la millor puntuació en els criteris més importants per al projecte:

- S'integra directament amb Active Directory amb l'aplicació *LDAP user and group backend*, de manera que els treballadors utilitzen el mateix usuari que en Windows.
- Inclou autenticació multifactor (TOTP) i permet fer-la obligatòria.
- Té clients per a Windows, Android i iOS.
- Té una documentació molt completa i una comunitat molt activa, cosa que en facilita l'administració.

El seu rendiment és inferior al de les altres opcions, però no és un problema per a 50 usuaris si s'utilitza Redis com a memòria cau.

La resta de components també s'han triat comparant alternatives:

| Component | Opció triada | Alternatives descartades | Motiu |
|---|---|---|---|
| Base de dades | MariaDB | PostgreSQL, SQLite | La més utilitzada amb Nextcloud. SQLite no és adequat per a diversos usuaris. |
| Proxy invers | Caddy | Nginx, Traefik | Configura HTTPS automàticament amb molt poques línies. |
| Redundància | RAID 1 amb mdadm | ZFS | Més senzill d'administrar i suficient per a esta empresa. |
| Còpies | BorgBackup | rsync, Restic | Còpies incrementals, xifrades i amb retenció automàtica. |
| Monitorització | Uptime Kuma | Zabbix, Prometheus + Grafana | Molt més senzill de configurar i suficient per a esta empresa. |
| Virtualització | VirtualBox | KVM, VMware | Gratuït, conegut i fàcil d'utilitzar en un portàtil. |

## 4.4 Disseny detallat

| Element | Tecnologia | Configuració / recursos | Justificació |
|---|---|---|---|
| Virtualització | VirtualBox | 4 màquines virtuals en un portàtil | Permet muntar i provar tota la infraestructura sense maquinari dedicat. |
| Xarxa | Xarxa NAT de VirtualBox | 192.168.10.0/24, porta d'enllaç 192.168.10.1 | Simula la xarxa de l'oficina i el router amb reenviament de ports. |
| DNS | DNS d'Active Directory | Zona `corp.arreloberta.com` i registre `nuvol.arreloberta.com` → 192.168.10.20 | Els equips de l'oficina accedeixen directament al servidor. |
| DHCP | Xarxa NAT (laboratori) | Clients per DHCP; servidors amb IP fixa | Simplicitat en el laboratori. |
| Router / tallafocs | Xarxa NAT + UFW | Només 443/tcp i 51820/udp reenviats a NUVOL01 | Exposar a Internet el mínim imprescindible. |
| VPN | WireGuard | Xarxa 10.10.10.0/24 en NUVOL01 | Accés d'administració segur des de fora. |
| DC01 | Windows Server 2022 | 2 vCPU, 4 GB RAM, 50 GB; IP 192.168.10.10. AD DS, DNS i AD CS | Gestió d'usuaris; AD CS emet el certificat necessari per a LDAPS. |
| NUVOL01 | Ubuntu Server 26.04 LTS | 2 vCPU, 4 GB RAM, 25 GB + 2 × 20 GB; IP 192.168.10.20 | Servidor de la cloud. |
| BACKUP01 | Ubuntu Server 26.04 LTS | 1 vCPU, 2 GB RAM, 20 GB + 60 GB; IP 192.168.10.30 | Còpies i monitorització separades del servidor principal. |
| CLIENT01 | Windows 10/11 | 2 vCPU, 4 GB RAM, 50 GB; unit al domini | Proves com a usuari final. |
| Contenidors | Docker Compose | Caddy, Nextcloud, cron, MariaDB, Redis | Desplegament reproduïble a partir d'un fitxer versionat. |
| Emmagatzematge | mdadm + LVM + ext4 | RAID 1 amb 2 discos → `/srv/nuvol` | Redundància davant de la fallada d'un disc i possibilitat d'ampliar. |

> Els noms de domini són fictics i s'utilitzen només com a exemple per al projecte. El domini d'AD és un subdomini del domini públic (`corp.`), tal com recomana Microsoft.

### Contenidors de NUVOL01

| Contenidor | Funció |
|---|---|
| `caddy` | Proxy invers amb HTTPS. És l'únic que publica ports (80 i 443). |
| `nextcloud` | Aplicació Nextcloud (imatge oficial). |
| `cron` | Tasques en segon pla de Nextcloud. |
| `mariadb` | Base de dades. |
| `redis` | Memòria cau i bloqueig de fitxers. |

La configuració es guarda en un fitxer `docker-compose.yml` dins del repositori, i les contrasenyes en un fitxer `.env` que no es puja al repositori. Com que el domini és fictici, en el laboratori Caddy utilitzarà la seua pròpia autoritat de certificació (`tls internal`) i el certificat arrel s'importarà als equips client. En un entorn real, Caddy obtindria automàticament un certificat de Let's Encrypt.

### Integració amb Active Directory

Nextcloud es connecta a DC01 per **LDAPS (port 636)**, amb un compte de servei de només lectura (`svc-nextcloud`). Només poden accedir els membres del grup `GG_Nuvol_Usuaris`, i el nom d'usuari és el mateix que en Windows (`sAMAccountName`). Les altes, baixes i canvis de contrasenya es continuen fent en AD: quan es deshabilita el compte d'un treballador, este perd l'accés a la cloud.

| Grup d'AD | Ús |
|---|---|
| `GG_Nuvol_Usuaris` | Usuaris que poden accedir a la cloud |
| `GG_Nuvol_Admins` | Administradors de Nextcloud |
| `GG_RRHH`, `GG_Administracio`, `GG_Sistemes`, `GG_Comercial`, `GG_Direccio` | Un grup per departament, per compartir fitxers amb tot un departament |

### Organització de la informació

Cada treballador disposa del seu **espai personal**, equivalent a la seua carpeta individual actual:

- Cada usuari només veu els seus fitxers i els que li han compartit.
- Quota de 10 GB per usuari, ampliable.
- Es pot compartir amb altres usuaris o amb un grup de departament, amb permís de lectura o d'edició.
- Per compartir amb persones de fora de l'empresa, només s'admeten enllaços amb contrasenya i data de caducitat.
- Nextcloud guarda versions anteriors dels fitxers i té una paperera, de manera que l'usuari pot recuperar un fitxer modificat o eliminat per error.

### Emmagatzematge

```text
  /dev/sdb ─┐
            ├─► /dev/md0 (RAID 1) ─► LVM ─► ext4 ─► /srv/nuvol
  /dev/sdc ─┘
```

Els dos discos contenen la mateixa informació: si un falla, el servei continua amb l'altre fins que es substitueix. LVM permet ampliar l'espai en el futur afegint nous discos.

### Migració de les dades

1. Configurar Nextcloud perquè el nom intern de cada usuari siga el seu `sAMAccountName`.
2. Crear l'espai de cada usuari (primer inici de sessió o per línia de comandes).
3. Muntar la carpeta compartida de DC01 en NUVOL01 i copiar la carpeta de cada usuari al seu espai amb `rsync`.
4. Executar `occ files:scan --all` perquè Nextcloud reconega els fitxers.
5. Comprovar que el nombre de fitxers i l'espai ocupat coincideixen amb l'origen.
6. Deixar les carpetes antigues en només lectura durant un temps abans de retirar-les.

### Relació entre requisits i disseny

| Requisit | Solució | Apartat |
|---|---|---|
| RF-01 | Espai personal en Nextcloud | 4.4 |
| RF-02 | Registre DNS intern cap a NUVOL01 | 4.4 |
| RF-03 | Accés HTTPS des de fora a través del router | 4.2, 4.5 |
| RF-04 / RF-05 | Compartició amb usuaris i grups, amb permís de lectura o edició | 4.4 |
| RF-06 | Migració de les carpetes individuals amb `rsync` | 4.4 |
| RF-07 | Client d'escriptori i aplicacions mòbils | 4.3.3 |
| RT-01 | NUVOL01 amb Ubuntu Server | 4.4 |
| RT-02 / RT-03 | Integració LDAPS amb Active Directory | 4.4 |
| RS-01 / RS-04 | Inici de sessió amb compte d'AD i espais personals | 4.4 |
| RS-02 | MFA TOTP obligatori | 4.5 |
| RS-03 | HTTPS amb Caddy | 4.4, 4.5 |
| RD-01 | RAID 1 amb mdadm | 4.4 |
| RD-02 / RD-03 | BorgBackup cap a BACKUP01 i proves de restauració | 4.6 |
| RM-01 | Uptime Kuma i avisos de mdadm | 4.7 |
| RE-01 | LVM | 4.4 |
| RA-01 | Script de còpia programat amb `cron` | 4.6 |
| RDOC-01 | Documentació tècnica (apartat 11) | 4.8 |

## 4.5 Seguretat

| Amenaça | Mesures |
|---|---|
| Atacs des d'Internet contra els serveis publicats | El router només reenvia el port 443 (HTTPS) i el 51820/udp (VPN). Tallafocs UFW en els servidors amb tot tancat per defecte. |
| Robatori o endevinació de contrasenyes | MFA (TOTP) obligatori en Nextcloud, protecció contra força bruta integrada i política de contrasenyes d'AD. |
| Interceptació de les comunicacions | HTTPS per a l'accés web i LDAPS entre Nextcloud i AD. |
| Accés no autoritzat a l'administració | SSH només amb claus i sense accés de `root`, i només des de la xarxa interna o per VPN WireGuard. |
| Vulnerabilitats en el programari | Actualitzacions de seguretat automàtiques en Ubuntu (`unattended-upgrades`) i actualització controlada de les imatges de Docker. |
| Accés a informació d'altres usuaris | Espais personals i compartició controlada per l'usuari. Compte de servei LDAP de només lectura. |
| Filtració de contrasenyes del sistema | Fitxer `.env` amb permisos restringits i exclòs del repositori. |
| Compartició insegura amb l'exterior | Enllaços externs només amb contrasenya i data de caducitat. |
| Ransomware o esborrat de dades | Còpies xifrades en un servidor separat i en un disc extern (apartat 4.6). |

Els registres (*logs*) de Nextcloud, Caddy i del sistema es conservaran en cada servidor per poder analitzar qualsevol incidència.

## 4.6 Còpies de seguretat i recuperació

La política de còpies segueix la **regla 3-2-1**: tres còpies de les dades, en dos suports diferents i una fora de l'oficina.

| Aspecte | Decisió |
|---|---|
| Què es copia | Dades dels usuaris, configuració de Nextcloud, fitxers de Docker i exportació de la base de dades. En DC01, l'estat del sistema amb *Windows Server Backup*. |
| Freqüència | Cada nit a les 02:00 (`cron`). |
| Destinació | Còpia 1: RAID 1 de NUVOL01. Còpia 2: BACKUP01. Còpia 3: disc extern guardat fora de l'oficina. |
| Retenció | 7 còpies diàries, 4 setmanals i 6 mensuals. |
| Protecció | Repositori Borg xifrat. NUVOL01 envia les còpies per SSH amb clau, a un usuari de BACKUP01 que només pot executar Borg. |
| Eina | BorgBackup: còpies incrementals, comprimides i xifrades. |

**Procediment de còpia** (script `scripts/backup/`):

1. Posar Nextcloud en mode manteniment.
2. Exportar la base de dades (`mariadb-dump`).
3. Copiar amb Borg les dades, la configuració i l'exportació a BACKUP01.
4. Traure Nextcloud del mode manteniment.
5. Esborrar les còpies antigues segons la retenció i avisar si alguna cosa ha fallat.

**Procediment de restauració** (script `scripts/restore/`):

- **Un fitxer:** si encara és a la paperera o en les versions de Nextcloud, el recupera el mateix usuari. Si no, es recupera des de la còpia de Borg i es torna a escanejar amb `occ files:scan`.
- **Servei complet:** instal·lar un servidor nou amb el manual d'instal·lació, restaurar des de Borg les dades i la configuració, importar la base de dades i arrancar els contenidors.

Es farà una prova de restauració de fitxers periòdica i, almenys una vegada durant el projecte, una restauració completa en una màquina virtual nova.

## 4.7 Monitorització i manteniment

**Monitorització.** Uptime Kuma s'executa en BACKUP01 perquè, si NUVOL01 deixa de funcionar, el sistema de monitorització continue actiu i puga avisar.

| Element | Comprovació | Alerta |
|---|---|---|
| Nextcloud | La web respon per HTTPS | Correu si no respon |
| Certificat | Dies que falten perquè caduque | Correu abans que caduque |
| NUVOL01 | Ping i port SSH | Correu si no respon |
| DC01 | Ping i port LDAPS (636) | Correu si no respon |
| RAID 1 | `mdadm --monitor` | Correu si falla un disc |
| Còpies | Resultat de l'script de còpia | Correu si la còpia falla |

**Manteniment preventiu:**

| Tasca | Freqüència |
|---|---|
| Revisar els avisos i l'estat de les còpies | Setmanal |
| Actualitzar les imatges de Docker (Nextcloud, MariaDB, Redis, Caddy) | Mensual |
| Prova de restauració d'un fitxer | Mensual |
| Revisar l'espai lliure i les quotes | Mensual |
| Rotar el disc extern de còpies | Mensual |
| Restauració completa de prova | Anual |

Les actualitzacions de seguretat del sistema operatiu s'apliquen automàticament.

**Manteniment correctiu:** davant d'una incidència (disc avariat, servei caigut, pèrdua de dades) se seguirà el manual de recuperació de l'apartat 11.4 i es registrarà en el registre d'incidències (apartat 7.2).

## 4.8 Fases del projecte

| Fase | Objectiu | Tasques principals | Recursos | Resultat | Hores estimades |
|---|---|---|---|---|---|
| F1. Anàlisi i disseny | Definir què es farà i com | Anàlisi, requisits, alternatives, disseny | Documentació oficial, GitHub | Apartats 1–4 de la memòria | 15 h |
| F2. Planificació | Organitzar el treball | Riscos, tasques, Gantt, pressupost, aspectes legals | Full de càlcul, eina de Gantt | Apartat 5 | 8 h |
| F3. Preparació de l'entorn | Muntar el laboratori | Crear les VM, xarxa, instal·lar Windows Server, AD, DNS i AD CS | VirtualBox, ISOs | Laboratori funcionant | 12 h |
| F4. Desplegament de Nextcloud | Tindre la cloud funcionant | Ubuntu, RAID 1 + LVM, Docker Compose, Caddy, HTTPS | NUVOL01 | Nextcloud accessible per HTTPS | 14 h |
| F5. Integració i seguretat | Usuaris d'AD i accés segur | LDAPS, grups, MFA, UFW, SSH, WireGuard, accés extern | DC01, NUVOL01 | Inici de sessió amb AD i MFA | 14 h |
| F6. Còpies i monitorització | Protegir i vigilar el servei | BACKUP01, Borg, scripts, cron, Uptime Kuma, avisos | BACKUP01 | Còpies i avisos automàtics | 14 h |
| F7. Migració i proves | Comprovar que tot compleix els requisits | Migració de prova, pla de proves, fallada de disc, restauració | Totes les VM | Proves documentades i PMV | 12 h |
| F8. Documentació i defensa | Tancar el projecte | Manuals, memòria, presentació i assaig de la demo | — | Memòria final i presentació | 20 h |
| **Total** | | | | | **≈ 110 h** |

Les hores són una estimació i inclouen un marge per a la resolució de problemes, que se seguirà en l'apartat 7.

---

# 5. PLANIFICACIÓ

## 5.1 Anàlisi de riscos

> ⏳ **Pendent de desenvolupament**

## 5.2 Tasques i distribució temporal

> ⏳ **Pendent de desenvolupament**

## 5.3 Allotjament

> ⏳ **Pendent de desenvolupament**

## 5.4 Recursos i pressupost

> ⏳ **Pendent de desenvolupament**

## 5.5 Monetització/retorn

> ⏳ **Pendent de desenvolupament**

## 5.6 Requisits legals, normatius i ètics

> ⏳ **Pendent de desenvolupament**

## 5.7 Gestió documental

> ⏳ **Pendent de desenvolupament**

---

# 6. IMPLANTACIÓ I PROVES

## 6.1 Preparació de l'entorn

> ⏳ **Pendent de desenvolupament**

## 6.2 Instal·lació i configuració

> ⏳ **Pendent de desenvolupament**

## 6.3 Automatització

> ⏳ **Pendent de desenvolupament**

## 6.4 Pla de proves

> ⏳ **Pendent de desenvolupament**

### 6.4.1 Resultats de les proves

> ⏳ **Pendent de desenvolupament**

### 6.4.2 Producte mínim viable (PMV)

> ⏳ **Pendent de desenvolupament**

---

# 7. SEGUIMENT I CONTROL

## 7.1 Evolució del projecte

> ⏳ **Pendent de desenvolupament**

## 7.2 Registre d'incidències

> ⏳ **Pendent de desenvolupament**

## 7.3 Canvis realitzats

> ⏳ **Pendent de desenvolupament**

---

# 8. RESULTATS I CONCLUSIONS

## 8.1 Resultats obtinguts

> ⏳ **Pendent de desenvolupament**

## 8.2 Grau de consecució dels objectius

> ⏳ **Pendent de desenvolupament**

## 8.3 Incidències i dificultats

> ⏳ **Pendent de desenvolupament**

## 8.4 Nivell de satisfacció

> ⏳ **Pendent de desenvolupament**

## 8.5 Propostes de millora

> ⏳ **Pendent de desenvolupament**

---

# 9. BIBLIOGRAFIA/WEBGRAFIA I FONTS

Tots els recursos en línia s'han consultat l'octubre de 2026. La llista s'ampliarà durant la implantació.

| Font | Tipus | URL / Referència | Informació utilitzada |
|---|---|---|---|
| Nextcloud – *Server Administration Manual* | Documentació oficial | https://docs.nextcloud.com/server/stable/admin_manual/ | Funcionament general i administració de Nextcloud |
| Nextcloud – *System requirements* | Documentació oficial | https://docs.nextcloud.com/server/stable/admin_manual/installation/system_requirements.html | Bases de dades compatibles i requisits del servidor |
| Nextcloud – *User authentication with LDAP* | Documentació oficial | https://docs.nextcloud.com/server/stable/admin_manual/configuration_user/user_auth_ldap.html | Integració amb Active Directory |
| Nextcloud – *Two-factor authentication* | Documentació oficial | https://docs.nextcloud.com/server/stable/admin_manual/configuration_user/two_factor-auth.html | Autenticació multifactor i com fer-la obligatòria |
| Nextcloud – *Backup* | Documentació oficial | https://docs.nextcloud.com/server/stable/admin_manual/maintenance/backup.html | Què s'ha de copiar i ús del mode manteniment |
| Nextcloud – Imatge oficial de Docker | Repositori oficial | https://github.com/nextcloud/docker | Desplegament de Nextcloud amb Docker Compose |
| ownCloud – *Infinite Scale Admin Documentation* | Documentació oficial | https://doc.owncloud.com/ocis/next/admin/index.html | Característiques per a la comparativa |
| Seafile – *Admin Manual* | Documentació oficial | https://manual.seafile.com/ | Característiques per a la comparativa |
| Canonical – *Ubuntu 26.04 LTS release notes* | Documentació oficial | https://documentation.ubuntu.com/release-notes/26.04/ | Versió i suport del sistema operatiu |
| Canonical – *Ubuntu Server documentation* | Documentació oficial | https://documentation.ubuntu.com/server/ | Administració d'Ubuntu Server |
| Microsoft – *Selecting the Forest Root Domain* | Documentació oficial | https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/selecting-the-forest-root-domain | Nom del domini d'Active Directory |
| Microsoft – *Evaluation Center* | Fabricant | https://www.microsoft.com/en-us/evalcenter/ | Versió d'avaluació de Windows Server |
| Docker – *Docker Docs* i *Docker Compose* | Documentació oficial | https://docs.docker.com/ | Contenidors i fitxers Compose |
| Docker Hub – Imatges oficials de MariaDB i Redis | Repositori oficial | https://hub.docker.com/_/mariadb · https://hub.docker.com/_/redis | Configuració de la base de dades i la memòria cau |
| Caddy – *Documentation* | Documentació oficial | https://caddyserver.com/docs/ | Proxy invers i HTTPS automàtic (`tls internal`) |
| Let's Encrypt – *Documentation* | Documentació oficial | https://letsencrypt.org/docs/ | Certificats en un entorn real |
| Linux man-pages – *mdadm(8)* i *lvm(8)* | Documentació oficial | https://man7.org/linux/man-pages/man8/mdadm.8.html · https://man7.org/linux/man-pages/man8/lvm.8.html | RAID per programari i volums lògics |
| BorgBackup – *Documentation* | Documentació oficial | https://borgbackup.readthedocs.io/ | Còpies incrementals, xifrat i retenció |
| INCIBE – *Copias de seguridad: una guía de aproximación para el empresario* | Organisme públic | https://www.incibe.es/empresas/blog/copias-seguridad-guia-aproximacion-el-empresario | Política de còpies en l'empresa |
| Uptime Kuma – Repositori oficial | Repositori oficial | https://github.com/louislam/uptime-kuma | Monitorització de serveis |
| WireGuard | Documentació oficial | https://www.wireguard.com/ | VPN d'administració |
| Ubuntu Community – *UFW* | Documentació de la comunitat | https://help.ubuntu.com/community/UFW | Tallafocs dels servidors |
| Oracle – *VirtualBox User Manual* | Documentació oficial | https://www.virtualbox.org/manual/ | Màquines virtuals i Xarxa NAT |
| eesel AI – *Precios de Google Workspace 2026* | Article | https://www.eesel.ai/es/blog/google-workspace-pricing | Cost de Google Workspace per a la comparativa |
| Nordflux – *Aumento de precios Microsoft 365 2026* | Article | https://nordflux.de/es/conocimiento/microsoft-365-subida-de-precios-2026 | Cost de Microsoft 365 per a la comparativa |
| Git – *Documentation* | Documentació oficial | https://git-scm.com/doc | Control de versions |
| GitHub – *GitHub Docs* | Documentació oficial | https://docs.github.com/ | Repositori remot |

---

# 10. ANNEXOS

> ⏳ **Pendent de desenvolupament**

---

# 11. DOCUMENTACIÓ TÈCNICA

## 11.1 Manual d'instal·lació

> ⏳ **Pendent de desenvolupament**

## 11.2 Manual d'administració

> ⏳ **Pendent de desenvolupament**

## 11.3 Manual d'usuari

> ⏳ **Pendent de desenvolupament**

## 11.4 Manual de recuperació

> ⏳ **Pendent de desenvolupament**

---

# 12. PRESENTACIÓ I DEFENSA

## 12.1 Estructura de la presentació

> ⏳ **Pendent de desenvolupament**

## 12.2 Demostració

> ⏳ **Pendent de desenvolupament**

## 12.3 Preguntes previsibles

> ⏳ **Pendent de desenvolupament**
