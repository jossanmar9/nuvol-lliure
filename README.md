<p align="center">
  <img src="docs/imatges/logo.png" alt="Logo Núvol Lliure" width="300">
</p>

<h1 align="center">☁️ Núvol Lliure</h1>

<p align="center">
  <strong>Cloud privada per a una organització</strong>
</p>

<p align="center">
  Projecte Intermodular de <strong>2n curs del Cicle Formatiu de Grau Superior
  d'Administració de Sistemes Informàtics en Xarxa (ASIR)</strong>.
</p>

---

## 📋 Descripció

**Núvol Lliure** és un projecte que consisteix en el disseny i implantació d'una infraestructura de cloud privada per a una consultoria d'aproximadament **50 treballadors**.

Actualment, l'organització disposa d'un servidor Windows amb **Active Directory** que també s'utilitza com a servidor de fitxers mitjançant carpetes compartides i permisos NTFS.

El projecte planteja mantindre Active Directory com a sistema de gestió d'usuaris i grups i incorporar un nou servidor dedicat basat en **Linux** per proporcionar els serveis de cloud privada.

La nova infraestructura permetrà centralitzar, sincronitzar i compartir la informació de manera segura, proporcionant accés tant des de la xarxa interna com des de l'exterior.

Sempre que siga possible s'utilitzarà **programari lliure i de codi obert**.

## 🎯 Objectius

- Implantar una plataforma de cloud privada sobre un servidor dedicat basat en Linux.
- Centralitzar l'emmagatzematge de la informació de l'organització.
- Permetre la sincronització i compartició de fitxers.
- Mantindre Active Directory com a sistema de gestió dels usuaris i grups corporatius.
- Estudiar i provar la integració de la cloud privada amb Active Directory.
- Gestionar usuaris, grups i permisos.
- Proporcionar accés segur des de la xarxa interna i des de l'exterior.
- Implementar autenticació multifactor.
- Proporcionar redundància a l'emmagatzematge.
- Implementar una política de còpies de seguretat i recuperació.
- Monitoritzar els principals serveis de la infraestructura.
- Automatitzar tasques d'administració i manteniment quan resulte útil.
- Documentar la instal·lació, administració, manteniment i recuperació del sistema.

## 🏗️ Arquitectura inicial

La infraestructura partirà de dos servidors amb funcions diferenciades:

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

El servidor Windows continuarà gestionant les identitats corporatives mitjançant Active Directory, mentre que el nou servidor assumirà els serveis relacionats amb la cloud privada i l'emmagatzematge de la informació.

L'arquitectura definitiva s'establirà durant la fase de disseny del projecte.

## 🛠️ Tecnologies

### Previstes inicialment

- **Linux / Ubuntu Server**
- **Docker**
- **Docker Compose**
- **Nextcloud** *(candidata principal com a plataforma de cloud; la selecció definitiva es justificarà en la fase de disseny)*
- **Active Directory**
- **Git**
- **GitHub**

### Pendents d'estudi i selecció

Durant el desenvolupament del projecte s'estudiaran diferents alternatives per a:

- Plataforma de cloud privada (Nextcloud, ownCloud Infinite Scale o Seafile).
- Base de dades.
- Emmagatzematge i redundància.
- Integració amb Active Directory.
- Proxy invers.
- HTTPS i certificats.
- Accés remot.
- Còpies de seguretat i recuperació.
- Monitorització.
- Automatització.
- Seguretat.

Les decisions tècniques es documentaran i justificaran segons els requisits i les necessitats de la infraestructura.

## 📁 Estructura del repositori

```text
cloud-privada-asir/
├── ansible/                  # Automatització de la infraestructura
├── config/                   # Fitxers de configuració
├── docker/                   # Configuració dels contenidors
├── docs/
│   ├── arquitectura/         # Diagrames i disseny de la infraestructura
│   ├── imatges/              # Imatges utilitzades en la documentació
│   ├── installacio/          # Documentació d'instal·lació
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

L'estructura podrà evolucionar durant el desenvolupament del projecte segons les necessitats que vagen apareixent.

## 📖 Documentació

La memòria principal del projecte es troba en:

[`docs/memoria/memoria.md`](docs/memoria/memoria.md)

Actualment estan documentades les primeres fases del projecte:

- Presentació del projecte.
- Introducció.
- Anàlisi de la situació actual.
- Necessitats.
- Requisits.
- Anàlisi de solucions existents.
- Anàlisi DAFO.

La documentació s'anirà ampliant a mesura que avance el disseny i la implantació de la infraestructura.

## 🔄 Control de versions

El projecte utilitza **Git** com a sistema de control de versions i **GitHub** com a repositori remot.

L'objectiu és:

- Mantindre un historial dels canvis realitzats durant el desenvolupament.
- Facilitar el seguiment de l'evolució del projecte.
- Mantindre una còpia remota de les configuracions, scripts i documentació.
- Aplicar una metodologia de treball similar a la utilitzada en un entorn professional.

> [!IMPORTANT]
> Les credencials, contrasenyes, claus privades, tokens, certificats i altres dades sensibles **no s'emmagatzemaran al repositori**.

Els fitxers que necessiten variables sensibles utilitzaran fitxers d'exemple, com `.env.example`, sense incloure credencials reals.

## 🚧 Estat del projecte

> 🟡 **En desenvolupament**

Actualment el projecte es troba en la fase d'**anàlisi i disseny de la solució**.

### Progrés

- [x] Definició inicial del projecte
- [x] Anàlisi de la situació actual
- [x] Identificació de necessitats
- [x] Definició inicial de requisits
- [x] Anàlisi inicial de solucions
- [x] Anàlisi DAFO
- [ ] Disseny de l'arquitectura definitiva
- [ ] Planificació de la implantació
- [ ] Preparació del servidor
- [ ] Desplegament de la cloud privada
- [ ] Integració amb Active Directory
- [ ] Configuració de seguretat
- [ ] Configuració de còpies de seguretat
- [ ] Monitorització
- [ ] Automatització
- [ ] Pla de proves
- [ ] Documentació tècnica
- [ ] Finalització de la memòria

---

## 📌 Estat

**Núvol Lliure es troba actualment en desenvolupament.**

Les tecnologies, configuracions i arquitectura descrites en este repositori poden canviar a mesura que avance l'anàlisi, el disseny i les proves del projecte.
