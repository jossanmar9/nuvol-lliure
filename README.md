<p align="center">
  <img src="docs/imatges/logo.png" alt="Logo Núvol Lliure" width="300">
</p>

# ☁️ Núvol Lliure

Projecte Intermodular de **2n curs del Cicle Formatiu de Grau Superior d'Administració de Sistemes Informàtics en Xarxa (ASIR)**.

# ☁️ Cloud privada per a una organització

Projecte Intermodular de **2n curs del Cicle Formatiu de Grau Superior d'Administració de Sistemes Informàtics en Xarxa (ASIR)**.

## 📋 Descripció

El projecte consisteix en el disseny i implantació d'una infraestructura de **cloud privada** destinada a una organització.

La plataforma permetrà centralitzar l'emmagatzematge de fitxers i oferir serveis de sincronització, compartició i col·laboració entre els usuaris, mantenint el control de la infraestructura i de les dades.

Sempre que siga possible s'utilitzarà **programari lliure i de codi obert**.

## 🎯 Objectius inicials

- Implantar una plataforma de cloud privada.
- Gestionar usuaris, grups i permisos.
- Proporcionar emmagatzematge centralitzat.
- Permetre la sincronització i compartició de fitxers.
- Proporcionar accés segur mitjançant HTTPS.
- Implementar una política de còpies de seguretat i recuperació.
- Aplicar mesures de seguretat a la infraestructura.
- Monitoritzar els serveis principals.
- Automatitzar tasques d'administració quan siga possible.
- Documentar la instal·lació, administració, ús i recuperació del sistema.

## 🛠️ Tecnologies

Tecnologies definides inicialment:

- **Ubuntu Server**
- **Docker**
- **Docker Compose**
- **Nextcloud**
- **Git**
- **GitHub**

Durant el desenvolupament del projecte s'estudiaran i seleccionaran altres tecnologies per a:

- Base de dades.
- Proxy invers.
- Autenticació i gestió d'usuaris.
- Còpies de seguretat i recuperació.
- Monitorització.
- Automatització.
- Seguretat.

Les diferents alternatives seran estudiades i les decisions tècniques es justificaran segons les necessitats del projecte.

## 📁 Estructura del repositori

```text
cloud-privada-asir/
├── ansible/              # Automatització de la infraestructura
├── config/               # Fitxers de configuració
├── docker/               # Configuració dels contenidors
├── docs/
│   ├── arquitectura/     # Diagrames i disseny de la infraestructura
│   ├── instalacion/      # Documentació d'instal·lació
│   ├── pruebas/          # Plans i resultats de proves
│   └── seguridad/        # Documentació de seguretat
├── scripts/
│   ├── backup/           # Scripts de còpies de seguretat
│   ├── mantenimiento/    # Scripts de manteniment
│   └── restore/          # Scripts de restauració
├── .env.example
├── .gitignore
└── README.md
```

L'estructura podrà evolucionar durant el desenvolupament del projecte segons les necessitats que vagen apareixent.

## 🔄 Control de versions

El projecte utilitza **Git** com a sistema de control de versions i **GitHub** com a repositori remot.

L'objectiu és:

- Mantindre un historial dels canvis realitzats durant el desenvolupament.
- Facilitar el seguiment de l'evolució del projecte.
- Mantindre una còpia remota del codi, configuracions i documentació.
- Aplicar una metodologia de treball similar a la utilitzada en un entorn professional.

> [!IMPORTANT]
> Les credencials, contrasenyes, claus, tokens i altres dades sensibles **no s'emmagatzemaran al repositori**.

## 🚧 Estat del projecte

> 🟡 **En desenvolupament**

El projecte es troba actualment en la fase inicial d'**anàlisi i disseny de la solució**.