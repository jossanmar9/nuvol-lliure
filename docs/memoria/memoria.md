# Núvol Lliure

## Cloud privada per a una organització

**Projecte Intermodular – 2n ASIR**

> 🚧 **Document en desenvolupament**
>
> Esta memòria s'anirà completant i actualitzant durant el desenvolupament del projecte.

---

## 1. Presentació del projecte

### 1.1 Títol

**Núvol Lliure – Cloud privada per a una organització**

### 1.2 Resum del projecte

Núvol Lliure és un projecte que té com a objectiu dissenyar i implantar una infraestructura de cloud privada per a una consultoria d'aproximadament 50 treballadors.

Actualment, l'empresa disposa d'un servidor Windows que s'utilitza tant per a la gestió d'usuaris mitjançant Active Directory com per a l'emmagatzematge de la informació. Els documents es comparteixen a través de carpetes de xarxa i l'accés es controla mitjançant permisos NTFS.

A mesura que l'empresa ha crescut i ha augmentat el nombre de treballadors, la gestió de les carpetes compartides i dels seus permisos s'ha tornat cada vegada més complexa. A més, existeix la necessitat de facilitar l'accés a la informació des de fora de l'oficina, especialment per als treballadors que desenvolupen part de la seua activitat fora de les instal·lacions, com l'equip comercial.

El projecte planteja mantindre el servidor Windows per a la gestió d'Active Directory i implantar un nou servidor dedicat basat en Linux que proporcione els serveis de cloud privada.

Les dades actuals es migraran a la nova plataforma, que haurà de proporcionar accés segur, gestió d'usuaris i permisos, redundància de l'emmagatzematge, còpies de seguretat i mecanismes de recuperació davant de fallades.

---

## 2. Introducció

### 2.1 Context i justificació

La consultoria compta amb aproximadament **50 treballadors**, distribuïts entre els següents departaments:

- Recursos Humans.
- Administració.
- Sistemes.
- Equip Comercial.
- Direcció.

Actualment, l'empresa utilitza un servidor Windows per centralitzar la informació. Els diferents directoris es comparteixen a través de la xarxa i els accessos es gestionen mitjançant usuaris, grups i permisos NTFS.

Este sistema ha funcionat correctament mentre l'organització tenia un nombre menor de treballadors, però el seu creixement ha provocat que la gestió de les carpetes i permisos siga cada vegada més complexa. El departament de Sistemes ha de gestionar un nombre creixent d'usuaris, grups, carpetes compartides i permisos.

Al mateix temps, la manera de treballar de l'empresa ha evolucionat. No tots els treballadors necessiten accedir a la informació exclusivament des de l'oficina. Un exemple és l'equip comercial, que pot necessitar consultar o modificar documentació mentre es troba visitant un client o treballant fora de les instal·lacions.

Per este motiu, es planteja la implantació d'una cloud privada que permeta mantindre la informació centralitzada i facilite l'accés als documents des de diferents ubicacions.

La nova infraestructura haurà de garantir també la seguretat de la informació. L'accés estarà protegit mitjançant comptes d'usuari, contrasenyes i autenticació multifactor (MFA), a més d'aplicar diferents permisos segons els grups i necessitats de cada departament.

També es tindrà en compte la disponibilitat de les dades. L'emmagatzematge haurà de disposar de redundància mitjançant RAID i existirà, de manera independent, un sistema de còpies de seguretat i recuperació.

### 2.2 Objectiu general

Dissenyar i implantar una infraestructura de **cloud privada segura, centralitzada i accessible remotament** que permeta a la consultoria gestionar i compartir la seua informació d'una manera més senzilla i adaptada al creixement de l'organització.

### 2.3 Objectius específics

Els objectius inicials del projecte són:

- Implantar una plataforma de cloud privada en un servidor dedicat basat en Linux.
- Centralitzar l'emmagatzematge de la informació de l'organització.
- Migrar les dades emmagatzemades actualment en les carpetes compartides del servidor Windows.
- Permetre l'accés als documents tant des de la xarxa interna com des de l'exterior.
- Gestionar usuaris, grups i permisos segons les necessitats de cada departament.
- Mantindre Active Directory com a sistema de gestió dels comptes corporatius.
- Estudiar la integració de la plataforma de cloud privada amb els usuaris i grups existents en Active Directory.
- Implementar mecanismes d'autenticació segura, incloent autenticació multifactor.
- Proporcionar redundància a l'emmagatzematge per reduir l'impacte de la fallada d'un disc.
- Implantar un sistema de còpies de seguretat independent de la redundància de l'emmagatzematge.
- Disposar d'un procediment que permeta recuperar les dades i els serveis davant d'una fallada.
- Monitoritzar l'estat del servidor i dels principals serveis.
- Automatitzar tasques d'administració quan resulte útil.
- Documentar la instal·lació, configuració, administració i recuperació de la infraestructura.

---

## 3. Anàlisi del projecte

### 3.1 Situació actual

La infraestructura actual de l'organització està basada principalment en un **servidor Windows**.

Este servidor proporciona el servei d'**Active Directory**, que permet gestionar de manera centralitzada els comptes d'usuari i grups de l'organització.

A més, el mateix servidor s'utilitza actualment per centralitzar els documents de l'empresa. La informació s'organitza en diferents carpetes compartides accessibles des de la xarxa i els permisos d'accés es controlen mitjançant permisos NTFS.

L'empresa està dividida en cinc departaments principals:

```text
Consultoria
│
├── Recursos Humans
├── Administració
├── Sistemes
├── Equip Comercial
└── Direcció
```

Cada departament necessita accedir a informació diferent, existint també documentació que pot ser compartida entre diversos departaments.

A mesura que l'empresa ha crescut fins a arribar aproximadament als 50 treballadors, l'administració del sistema de carpetes compartides s'ha tornat més complexa. La gestió dels permisos requereix cada vegada més treball i resulta menys còmoda a mesura que augmenten el nombre d'usuaris, grups i directoris.

Una altra limitació de la infraestructura actual és l'accés a la informació des de l'exterior. El sistema de carpetes compartides està orientat principalment a l'accés des de la xarxa corporativa, mentre que l'organització necessita facilitar el treball des d'altres ubicacions.

Esta necessitat afecta especialment l'equip comercial, els membres del qual poden necessitar accedir a documentació corporativa mentre es troben fora de l'oficina.

### 3.2 Necessitats detectades

A partir de les limitacions de la infraestructura actual s'han identificat diverses necessitats.

La primera és disposar d'un sistema que mantinga la informació **centralitzada**, però que facilite la seua gestió i compartició respecte al sistema actual de carpetes compartides.

La plataforma haurà de permetre també l'**accés remot** a la informació, de manera que els usuaris autoritzats puguen consultar i treballar amb els seus documents independentment de si es troben dins o fora de l'oficina.

La **seguretat** serà un altre aspecte fonamental. Cada usuari haurà d'identificar-se individualment i l'accés haurà d'estar protegit mitjançant contrasenya i autenticació multifactor.

També serà necessari establir una estructura de grups i permisos que permeta separar la informació dels diferents departaments i compartir determinats recursos únicament quan siga necessari.

La infraestructura haurà de disposar, a més, de mecanismes que permeten mantindre la disponibilitat de les dades davant de determinades fallades de maquinari. Per este motiu, es planteja utilitzar un sistema d'emmagatzematge amb **redundància RAID**.

La redundància proporcionada pel RAID no substituirà les còpies de seguretat. S'implantarà també un sistema de **còpies de seguretat independent**, juntament amb procediments que permeten comprovar que la informació pot ser restaurada correctament.

### 3.3 Proposta inicial

La proposta consisteix a separar la gestió del directori corporatiu de l'emmagatzematge principal de les dades.

El servidor Windows actual **continuarà en funcionament** i mantindrà el servei d'Active Directory. D'esta manera, no serà necessari substituir la infraestructura d'identitats existent.

S'incorporarà un **nou servidor físic dedicat basat en Linux**, que serà l'encarregat d'allotjar la infraestructura de Núvol Lliure.

De manera simplificada, la infraestructura inicial prevista serà:

```text
                         INTERNET
                             │
                      Router / Firewall
                             │
                    Xarxa corporativa
                             │
              ┌──────────────┴──────────────┐
              │                             │
      Servidor Windows              Servidor Núvol Lliure
              │                             │
      Active Directory                    Linux
              │                             │
       Usuaris i grups              Cloud privada
                                            │
                                      Emmagatzematge
                                        redundant
```

Les dades que actualment es troben emmagatzemades en les carpetes compartides del servidor Windows es migraran progressivament a Núvol Lliure.

Els permisos actuals hauran de ser analitzats abans de realitzar la migració. No es planteja traslladar directament l'estructura de permisos NTFS, sinó definir una nova estructura de grups i permisos adaptada al funcionament de la nova plataforma.

També s'estudiarà la possibilitat d'integrar la cloud privada amb **Active Directory**, de manera que els treballadors puguen utilitzar els seus comptes corporatius existents per accedir a la plataforma.

---

## 4. Disseny de la solució

> ⏳ **Pendent de desenvolupament**

En este apartat s'estudiaran les diferents alternatives disponibles i es definirà l'arquitectura definitiva de Núvol Lliure.

Entre altres aspectes, s'hauran d'estudiar:

- Dimensionament del servidor.
- Sistema operatiu.
- Plataforma de cloud privada.
- Emmagatzematge i nivell de RAID.
- Base de dades.
- Integració amb Active Directory.
- Accés des de l'exterior.
- Proxy invers i HTTPS.
- Autenticació multifactor.
- Còpies de seguretat i recuperació.
- Monitorització.
- Automatització.

Les decisions tècniques es prendran després de comparar diferents alternatives i justificar quina s'adapta millor a les necessitats del projecte.

---

## 5. Planificació

> ⏳ **Pendent de desenvolupament**

---

## 6. Implantació i proves

> ⏳ **Pendent de desenvolupament**

---

## 7. Seguiment i control

> ⏳ **Pendent de desenvolupament**

---

## 8. Resultats i conclusions

> ⏳ **Pendent de desenvolupament**

---

## 9. Bibliografia, webgrafia i fonts

> ⏳ **Pendent de desenvolupament**

---

## 10. Anexos

> ⏳ **Pendent de desenvolupament**

---

## 11. Documentació tècnica

Durant el desenvolupament del projecte es prepararà la documentació necessària per a la instal·lació, administració, ús i recuperació de Núvol Lliure.

> ⏳ **Pendent de desenvolupament**

---

## 12. Presentació i defensa

> ⏳ **Pendent de desenvolupament**