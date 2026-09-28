# Núvol Lliure

## Cloud privada per a una organització

**Projecte Intermodular – 2n ASIR**

> 🚧 **Document en desenvolupament**
>
> Esta memòria s'anirà completant i actualitzant durant el desenvolupament del projecte.

---

# 1. PRESENTACIÓ DEL PROJECTE

**Núvol Lliure** és un projecte que consisteix en el disseny i implantació d'una infraestructura de cloud privada per a una consultoria d'aproximadament 50 treballadors.

L'organització disposa actualment d'un servidor Windows amb Active Directory que també s'utilitza com a servidor de fitxers. El creixement de l'empresa i la necessitat d'accedir a la informació des de fora de l'oficina fan necessari estudiar una evolució d'esta infraestructura.

El projecte planteja mantindre Active Directory com a sistema de gestió d'identitats i incorporar un nou servidor dedicat basat en Linux per proporcionar els serveis de cloud privada.

La nova infraestructura haurà de permetre centralitzar, sincronitzar i compartir la informació de manera segura, proporcionar accés des de la xarxa interna i des de l'exterior i incorporar mecanismes de protecció, redundància, còpies de seguretat, monitorització i recuperació.

Sempre que siga possible, s'utilitzarà programari lliure i de codi obert i es buscarà una solució que puga créixer juntament amb les necessitats de l'organització.

---

# 2. INTRODUCCIÓ

## 2.1 Títol del projecte

**Núvol Lliure – Cloud privada per a una organització**

## 2.2 Resum

Núvol Lliure és un projecte orientat al disseny i implantació d'una infraestructura de cloud privada per a una consultoria d'aproximadament 50 treballadors.

Actualment, l'organització utilitza un servidor Windows amb Active Directory que també actua com a servidor de fitxers. La informació s'emmagatzema en carpetes compartides i l'accés es controla mitjançant grups i permisos NTFS. El creixement de l'empresa ha augmentat la complexitat de la seua administració i ha generat la necessitat de facilitar l'accés segur a la informació des de fora de l'oficina.

La solució proposada consisteix a incorporar un servidor dedicat basat en Linux per allotjar una plataforma de cloud privada, mantenint Active Directory com a sistema de gestió dels comptes corporatius i estudiant la seua integració amb la nova plataforma.

Inicialment es preveu utilitzar tecnologies com Linux, Docker, Docker Compose i Nextcloud, prioritzant les solucions de programari lliure.

El resultat esperat és una infraestructura segura i administrable que permeta centralitzar, sincronitzar i compartir la informació, amb accés intern i remot, control d'accés, autenticació multifactor, redundància d'emmagatzematge, còpies de seguretat, monitorització i procediments de recuperació.

## 2.3 Justificació

El projecte naix de la necessitat d'adaptar la infraestructura de l'organització al seu creixement i a una forma de treball en la qual els usuaris no sempre accedeixen a la informació des de l'oficina.

El sistema actual de carpetes compartides ha permés centralitzar els documents de l'empresa, però la seua administració s'ha tornat més complexa a mesura que han augmentat els usuaris, grups, directoris i permisos. Al mateix temps, determinats treballadors necessiten consultar o modificar documentació corporativa quan es troben fora de les instal·lacions.

Una cloud privada permet abordar estes necessitats mantenint el control de la infraestructura i de les dades dins de l'organització, al mateix temps que proporciona mecanismes més adaptats a l'accés, sincronització i compartició de fitxers.

Des del punt de vista formatiu, el projecte permet aplicar de manera conjunta diferents competències treballades durant el cicle d'ASIR: administració de sistemes Linux i Windows, Active Directory, xarxes, emmagatzematge, contenidors, seguretat, serveis de xarxa, còpies de seguretat, monitorització, automatització i documentació tècnica.

A més, s'utilitzaran Git i GitHub per controlar les versions de la documentació, configuracions i scripts desenvolupats durant el projecte, aplicant una metodologia de treball pròxima a la utilitzada en un entorn professional.

## 2.4 Objectius

### Objectiu general

Dissenyar i implantar una infraestructura de **cloud privada segura, centralitzada i accessible remotament** per a una organització d'aproximadament 50 usuaris, que permeta emmagatzemar, sincronitzar i compartir la informació corporativa.

### Objectius específics

- Implantar una plataforma de cloud privada sobre un servidor dedicat basat en Linux.
- Centralitzar en la nova plataforma les dades que actualment es troben en les carpetes compartides del servidor Windows.
- Mantindre Active Directory com a sistema de gestió dels usuaris i grups corporatius.
- Estudiar i provar la integració dels usuaris i grups d'Active Directory amb la plataforma de cloud privada.
- Permetre l'accés als documents des de la xarxa interna i des de l'exterior.
- Proporcionar accés segur mitjançant HTTPS i autenticació multifactor.
- Establir permisos d'accés diferenciats segons els usuaris i grups de l'organització.
- Implementar un sistema d'emmagatzematge redundant i comprovar el seu funcionament davant de la fallada d'un disc.
- Implementar un sistema de còpies de seguretat independent de la redundància de l'emmagatzematge.
- Realitzar proves de restauració que permeten verificar la recuperació de les dades.
- Monitoritzar l'estat del servidor i dels principals serveis desplegats.
- Automatitzar almenys una tasca d'administració o manteniment.
- Documentar la instal·lació, configuració, administració, manteniment i recuperació de la infraestructura.

---

# 3. ANÀLISI DEL PROJECTE

## 3.1 Situació actual

L'organització sobre la qual es planteja el projecte és una consultoria d'aproximadament **50 treballadors**, distribuïts en cinc departaments:

- Recursos Humans.
- Administració.
- Sistemes.
- Equip Comercial.
- Direcció.

La infraestructura actual disposa d'un **servidor Windows** que proporciona Active Directory per a la gestió centralitzada dels comptes d'usuari i grups de l'organització.

El mateix servidor també actua com a servidor de fitxers. La informació corporativa està organitzada en carpetes compartides i els accessos es controlen mitjançant grups i permisos NTFS.

Cada departament treballa amb informació pròpia, encara que també existeixen documents que han de ser accessibles per diversos departaments. A mesura que l'organització ha crescut, també ho han fet el nombre d'usuaris, grups, directoris i permisos que el departament de Sistemes ha de gestionar.

L'accés als fitxers està pensat principalment per a equips connectats a la xarxa corporativa. Esta situació limita els treballadors que necessiten consultar informació quan es troben fora de les instal·lacions, especialment l'equip comercial.

La nova infraestructura haurà de tindre també en compte la protecció i disponibilitat de la informació. Per això serà necessari estudiar mecanismes de redundància de l'emmagatzematge i disposar d'un sistema independent de còpies de seguretat i recuperació.

Les principals limitacions detectades en la situació actual són:

- Augment de la complexitat en la gestió de carpetes compartides i permisos.
- Major càrrega d'administració a mesura que creix l'organització.
- Accés als fitxers orientat principalment a la xarxa interna.
- Dificultat per proporcionar un accés remot còmode i segur.
- Necessitat de millorar la protecció davant de fallades de l'emmagatzematge.
- Necessitat de disposar d'una estratègia definida de còpies de seguretat i recuperació.

## 3.2 Necessitats

A partir de la situació actual s'han identificat necessitats funcionals, tècniques, de seguretat, disponibilitat, escalabilitat i econòmiques.

### Necessitats funcionals

La plataforma haurà de centralitzar la informació corporativa i permetre als usuaris emmagatzemar, consultar, sincronitzar i compartir documents.

Els usuaris autoritzats hauran de poder accedir als seus documents tant des de la xarxa interna com des de l'exterior.

També serà necessari establir diferents nivells d'accés segons els usuaris i grups de l'organització i permetre compartir informació entre departaments quan siga necessari.

### Necessitats tècniques

La infraestructura de cloud privada s'allotjarà en un servidor dedicat basat en Linux, separat del servidor Windows actual.

Active Directory continuarà utilitzant-se per gestionar les identitats corporatives i s'estudiarà la seua integració amb la nova plataforma.

També serà necessari definir un procediment per migrar les dades actuals des del servidor Windows i seleccionar les tecnologies necessàries per desplegar i administrar la nova infraestructura.

### Necessitats de seguretat

Cada usuari haurà d'identificar-se individualment i únicament podrà accedir a la informació per a la qual dispose d'autorització.

Les comunicacions realitzades des de l'exterior hauran d'estar xifrades i la plataforma haurà de permetre utilitzar autenticació multifactor.

Els serveis exposats a Internet hauran de limitar-se als necessaris i tant el sistema operatiu com les aplicacions hauran de mantindre's actualitzats.

### Necessitats de disponibilitat

La informació corporativa és necessària per al funcionament diari de l'organització, per la qual cosa serà necessari reduir l'impacte que podria provocar la fallada d'un dispositiu d'emmagatzematge.

S'estudiaran diferents alternatives de redundància per determinar quina s'adapta millor a les necessitats i recursos disponibles.

Independentment del sistema de redundància seleccionat, serà necessari disposar de còpies de seguretat i comprovar periòdicament que les dades poden restaurar-se.

### Necessitats d'escalabilitat

La infraestructura es dimensionarà inicialment per a aproximadament 50 usuaris, però haurà de permetre augmentar la capacitat d'emmagatzematge i assumir un possible creixement del nombre d'usuaris.

### Necessitats econòmiques

Sempre que siga possible s'utilitzaran solucions de **programari lliure i de codi obert**, reduint els costos associats a llicències i evitant una dependència innecessària de proveïdors.

No obstant això, s'hauran de considerar els costos del maquinari, emmagatzematge, sistema de còpies de seguretat, consum elèctric i manteniment de la infraestructura.

## 3.3 Requisits

A partir de les necessitats detectades s'estableixen els següents requisits inicials:

| ID | Tipus | Requisit | Prioritat | Com es verificarà |
|---|---|---|---|---|
| RF-01 | Funcional | El sistema haurà de permetre emmagatzemar, consultar i gestionar fitxers de manera centralitzada. | Alta | Crear, modificar, descarregar i eliminar fitxers de prova. |
| RF-02 | Funcional | Els usuaris hauran de poder accedir als seus documents des de la xarxa interna. | Alta | Realitzar una prova d'accés des d'un equip de la xarxa corporativa. |
| RF-03 | Funcional | Els usuaris autoritzats hauran de poder accedir als seus documents des d'una xarxa externa. | Alta | Realitzar una prova d'accés des d'una connexió externa. |
| RF-04 | Funcional | El sistema haurà de permetre compartir fitxers i carpetes entre usuaris o grups autoritzats. | Alta | Compartir una carpeta de prova i comprovar-ne l'accés. |
| RF-05 | Funcional | El sistema haurà de permetre aplicar permisos diferents segons els usuaris o grups. | Alta | Comprovar l'accés utilitzant comptes amb permisos diferents. |
| RF-06 | Funcional | Les dades del servidor Windows hauran de poder migrar-se a la nova plataforma. | Alta | Realitzar una migració de prova i verificar els fitxers resultants. |
| RT-01 | Tècnic | La cloud privada s'executarà sobre un servidor dedicat basat en Linux. | Alta | Comprovar i documentar el servidor desplegat. |
| RT-02 | Tècnic | Active Directory continuarà proporcionant la gestió centralitzada dels comptes corporatius. | Alta | Comprovar el funcionament dels comptes i grups després de la implantació. |
| RT-03 | Tècnic | S'haurà de provar la integració de la plataforma amb Active Directory. | Alta | Autenticar un usuari de prova procedent d'Active Directory. |
| RS-01 | Seguretat | L'accés a la informació privada requerirà autenticació. | Alta | Intentar accedir sense iniciar sessió. |
| RS-02 | Seguretat | La plataforma haurà de permetre utilitzar autenticació multifactor. | Alta | Activar MFA en un compte i realitzar una autenticació de prova. |
| RS-03 | Seguretat | L'accés web haurà d'estar protegit mitjançant HTTPS. | Alta | Comprovar el certificat i la connexió HTTPS des d'un navegador. |
| RS-04 | Seguretat | Un usuari no podrà accedir a informació per a la qual no dispose de permisos. | Alta | Intentar accedir amb un compte no autoritzat a un recurs restringit. |
| RD-01 | Disponibilitat | L'emmagatzematge haurà de disposar d'un mecanisme de redundància. | Alta | Simular de manera controlada la fallada d'un disc i comprovar la disponibilitat de les dades. |
| RD-02 | Disponibilitat | Les dades hauran de disposar de còpies de seguretat independents del sistema de redundància. | Alta | Executar una còpia i comprovar que finalitza correctament. |
| RD-03 | Disponibilitat | Les còpies de seguretat hauran de permetre recuperar informació. | Alta | Eliminar un fitxer de prova i restaurar-lo des d'una còpia de seguretat. |
| RM-01 | Monitorització | S'haurà de supervisar l'estat del servidor i dels principals serveis. | Mitjana | Detindre de manera controlada un servei i comprovar que la incidència és detectada. |
| RE-01 | Escalabilitat | La infraestructura haurà de permetre ampliar la capacitat d'emmagatzematge. | Mitjana | Documentar el procediment i les possibilitats d'ampliació. |
| RA-01 | Automatització | S'automatitzarà almenys una tasca d'administració o manteniment. | Mitjana | Executar la tasca automatitzada i documentar-ne el resultat. |
| RDOC-01 | Documentació | El projecte disposarà de documentació suficient per instal·lar, administrar i recuperar la infraestructura. | Alta | Seguir els procediments documentats durant les proves finals. |

Els requisits podran revisar-se durant les fases de disseny i implantació si apareixen noves necessitats o limitacions tècniques.

## 3.4 Anàlisi de solucions existents/competència

Abans de seleccionar la plataforma de cloud privada s'han considerat tres alternatives autohostejables: **Nextcloud, ownCloud Infinite Scale i Seafile**.

### Nextcloud

Nextcloud és una plataforma orientada a l'emmagatzematge, sincronització, compartició i col·laboració de fitxers.

Disposa de gestió d'usuaris i grups, possibilitats d'integració amb LDAP i Active Directory, autenticació multifactor i un sistema d'aplicacions que permet ampliar les funcionalitats de la plataforma.

La seua orientació com a plataforma de cloud i col·laboració fa que siga una de les alternatives que s'estudiaran per a Núvol Lliure.

### ownCloud Infinite Scale

ownCloud Infinite Scale és una plataforma destinada a l'emmagatzematge, sincronització i compartició de fitxers.

La seua arquitectura està orientada a proporcionar escalabilitat i permet integrar sistemes externs d'identitat. És una alternativa que haurà de valorar-se tenint en compte tant les seues possibilitats com la complexitat d'implantació i administració per a una organització d'esta grandària.

### Seafile

Seafile és una plataforma especialment orientada a l'emmagatzematge, sincronització i compartició eficient de fitxers.

Disposa d'una edició comunitària i permet el desplegament en contenidors i la integració amb serveis de directori.

És una alternativa a considerar si es prioritza principalment la gestió i sincronització de fitxers davant d'altres funcionalitats de col·laboració.

### Comparació inicial

| Característica | Nextcloud | ownCloud Infinite Scale | Seafile |
|---|---|---|---|
| Autohostatge | Sí | Sí | Sí |
| Edició de programari lliure/comunitària | Sí | Sí | Sí |
| Emmagatzematge de fitxers | Sí | Sí | Sí |
| Sincronització de fitxers | Sí | Sí | Sí |
| Compartició de fitxers | Sí | Sí | Sí |
| Integració amb serveis de directori | Sí | Sí | Sí |
| Accés web | Sí | Sí | Sí |
| Desplegament amb contenidors | Sí | Sí | Sí |
| Orientació principal | Cloud i col·laboració | Cloud i escalabilitat | Sincronització i fitxers |

Esta comparació és una primera aproximació. La selecció de la plataforma es realitzarà durant el disseny de la solució, després d'analitzar amb més detall els requisits del projecte i justificar l'alternativa escollida.

## 3.5 Anàlisi DAFO

L'anàlisi DAFO permet identificar els factors interns i externs que poden influir en el desenvolupament i implantació de Núvol Lliure.

| Fortaleses | Debilitats |
|---|---|
| Control de la infraestructura i de les dades per part de l'organització. | Necessitat d'administrar i mantindre una infraestructura pròpia. |
| Centralització de la informació en una plataforma de cloud privada. | Major responsabilitat del departament de Sistemes sobre la disponibilitat i seguretat del servei. |
| Possibilitat d'aprofitar els usuaris i grups d'Active Directory. | Necessitat de migrar les dades des del servidor actual. |
| Possibilitat d'utilitzar principalment programari lliure. | Necessitat de dedicar temps a la configuració, proves i manteniment. |
| Capacitat d'adaptar la infraestructura a les necessitats de l'organització. | Necessitat d'adaptació dels usuaris a la nova plataforma. |
| Possibilitat d'incorporar redundància, backups i mecanismes de recuperació. | Necessitat de disposar de personal amb coneixements per administrar la infraestructura. |

| Oportunitats | Amenaces |
|---|---|
| Facilitar el treball remot i l'accés a la informació des de diferents ubicacions. | Atacs contra els serveis accessibles des d'Internet. |
| Reduir la dependència de serveis cloud de tercers. | Aparició de vulnerabilitats en els components utilitzats. |
| Facilitar el creixement futur de la infraestructura. | Fallades de maquinari que provoquen interrupcions del servei. |
| Incorporar noves funcionalitats en el futur. | Pèrdua o corrupció de dades. |
| Modernitzar el sistema de compartició de fitxers de l'organització. | Dependència de la connexió a Internet per a l'accés remot. |
| Introduir processos d'automatització i monitorització. | Errors de configuració que puguen provocar problemes de seguretat o disponibilitat. |

L'anàlisi mostra que la implantació d'una cloud privada pot resoldre diverses de les limitacions detectades, però també trasllada a l'organització la responsabilitat de mantindre i protegir la infraestructura.

Per este motiu, el disseny haurà de prestar especial atenció a la seguretat, disponibilitat, còpies de seguretat, monitorització i recuperació davant de fallades.

---

## 4. Disseny de la solució

> ⏳ **Pendent de desenvolupament**

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