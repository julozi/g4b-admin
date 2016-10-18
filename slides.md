class: ifb, center, middle

# Galaxy4Bioinformatics V5

## Installation et paramétrage d'une instance

Christophe Caron | Alexis Dereeper | Stéphanie Le Gras | Julien Seiler

.footer[
https://julozi.github.io/g4b-admin
]

---

layout: true
name: title
class: ifb, middle

.footer[
Galaxy4Bioinformatics V5
]

---

layout: true
name: content
class: ifb

.footer[
Galaxy4Bioinformatics V5
]

---

template: title

# Installation

---

template: content

# Récupération des sources

La façon la plus courante d’installer Galaxy est de télécharger le code source du projet en utilisant l’outil Git

### Récupérer les sources de Galaxy (en utilisant Git)

```shell
[~]$ git clone https://github.com/galaxyproject/galaxy/
[~]$ cd galaxy
[galaxy]$ git checkout -b master origin/master
```

.callout.callout-danger[
#### Attention
Les sources de Galaxy sont déjà installé sur votre VM dans le dossier /usr/local/galaxy/galaxy
]

---

# Premier démarrage de Galaxy

Vous êtes prêt à lancer votre instance Galaxy !

### Lancez le service Galaxy

```shell
[galaxy]$ sh run.sh
```

Le fichier `run.sh` réalise plusieurs opérations permettant d’initialiser votre environnement Galaxy avec des paramètres par défaut :

1. Il crée et source votre environnement virtuel python (par défault .venv)
1. Il récupère les packages python nécessaires au fonctionnement de Galaxy
1. Il initialise les fichiers de configurations par défaut
1. Il initialise la base de données (par défaut SQLlite)

Après une ou deux minutes d’initialisation, vous pouvez tester l’application via l’URL : http://localhost:8080/

---

# Lancer un job

Chargez dans votre historique le fichier `/usr/local/galaxy/galaxy/test-data/1.tabular`

1. Cliquez sur l’outil **Get Data / Upload File**
1. Cliquez sur **Choose local file**
1. Sélectionnez le fichier `/usr/local/galaxy/galaxy/test-data/1.tabular`
1. Cliquez sur **Start**
1. Lorsque le chargement est terminé, cliquez sur **Close**

Utilisez un outil de manipulation de texte pour changer la casse de la troisième colonne de ce dataset

1. Cliquez sur l’outil **Text Manipulation / Change case**
1. Vérifier que la champ **From** pointe vers le dataset que vous venez de charger
1. Indiquez la valeur `c3` pour le champ **Change case of columns**
1. Cliquez sur **Execute**

Un nouveau dataset a été créé dans votre historique

---

template: title

# Configuration

### Organisation générale des fichiers

---

template: content

# Organisation générale des répertoires

- **config**: Contient les différents fichiers de configurations

- **tools**: Contient les outils et wrappers de base de Galaxy

- **tool-data**: Contient les descripteurs de données (fichier .loc (“location”)).
La liste des génomes disponibles est à renseigner dans `shared/ucsc/builds.txt`

- **static**: Contient le style et les pages HTML.
Pour personnaliser la page d’accueil, on peut modifier le script `static/welcome.html`

- **database**: Contient les données des utilisateurs

- **logs**: Contient les fichiers de logs de l’instance.

---

# Les fichiers de configuration

Les principaux fichiers de configuration se trouvent dans le répertoire `config`.
Ils sont créés automatiquement par Galaxy lors du premier lancement.

- **galaxy.ini**: configuration générale de l’instance

- **tool_conf.xml**: configuration des outils disponibles

- **tool_sheds_conf.xml**: configuration des toolshed accessibles depuis votre instance

- **shed_tool_conf.xml**: configuration des outils déployés sur l’instance via un toolshed

- **datatypes_conf.xml**: déclaration des différents types de données

- **job_conf.xml**: déclaration des ressources de calcul (queue, mémoire…)

---

# Les fichiers de configuration

Le fichier `galaxy.ini` est le fichier de configuration principal de Galaxy. C’est lui qui indique la localisation relative ou absolu de l’ensemble des autres fichiers ou dossiers de Galaxy.

Il est possible de spécifier un fichier `galaxy.ini` spécifique lors du lancement de Galaxy en renseignant la variable d’environnement `GALAXY_CONFIG_FILE`.

```shell
[galaxy]$ GALAXY_CONFIG_FILE=config/galaxy.ini run.sh
```

.callout.callout-success[
Afin de laisser inchangées les sources de Galaxy et ainsi faciliter vos futures mise à jour, nous vous recommandons de stocker l’ensemble de vos fichiers de configuration et dossier spécifiques à Galaxy en dehors du dossier des sources.

Vous pouvez par exemple créer un dossier `config` contenant l’ensemble de vos fichiers de configuration à côté de votre dossier `galaxy`.
]

---

# `galaxy.ini`

Le fichier de configuration `galaxy.ini`  est organisé en sections permettant de paramétrer les différents composants de Galaxy :

--

## Les gestionnaires HTTP ou handlers
- Un gestionnaire HTTP est le processus capable de traduire une requête HTTP (par exemple envoyé par le navigateur d’un utilisateur) en un appel de l’application Galaxy
- Par défaut, un seul gestionnaire HTTP `[server:main]` est défini.

---

# `galaxy.ini`

## Les filtres
- Les filtres sont situés entre les serveurs HTTP et l’application Galaxy
- Ils permettent par exemple de définir un algorithme de compression des données transférés ou de définir un prefix pour son serveur Galaxy (`/galaxy` par exemple)
- Par défaut, aucun filtre n’est utilisé.

--

## La configuration de l’application Galaxy
- Cette section commence par le marqueur `[app:main]`
- Cette section contient l’ensemble des paramètres de fonctionnement de Galaxy (chemin, répertoires, bases de données, authentification des utilisateurs, sécurité, etc.)
- Elle définit également la localisation des fichiers de configuration annexes

---

template: title

# Configuration

### Les handlers

---

template: content
layout: true

# Les handlers

---

Galaxy est une application Web qui utilise des gestionnaires ou handlers pour effectuer des actions.

Il existe deux grands types d'actions qui sont réalisées par les handlers :
- Répondre aux requêtes des utilisateurs ; ces actions sont effectuées par des web handlers
- Gérer l’exécution des outils ; ces actions sont effectuées par les job handlers.

Par défaut, Galaxy est configuré pour exécuter un seul gestionnaire qui gère les requêtes utilisateurs et les jobs.

Selon le nombre d'utilisateurs accédant à votre instance Galaxy ou le nombre de jobs que vous devez gérer vous pourriez avoir besoin de lancer des web handlers ou des jobs handlers supplémentaires.

---

Pour une instance Galaxy classique on déclare au moins un handler dédié à l’execution des jobs. Ceci nous permettra notamment de séparer les informations de log spécifiques à l’exécution des outils, des informations concernant les actions de l’utilisateur sur l’interface web de Galaxy.

Si votre instance Galaxy tourne toujours, arrêtez-la avec la séquence de touche `Ctrl+c`

---

### Déclarez un nouveau handler qui écoutera sur le port `8090`

Dans votre votre fichier `config/galaxy.ini`, ajouter les lignes suivantes à la fin de la section HTTP server (ligne 50)

```ini
[server:job]
use = egg:Paste#http
port = 9080
use_threadpool = True
threadpool_kill_thread_limit = 10800
```

Cette section définit un nouveau handler qui sera lancé par le script `run.sh`

---

.pure-table.pure-table-bordered.smaller-font[
Paramètre | Description
--- | ---
use = egg:Paste#http | Le type de moteur utilisé pour exécuter ce handler
port = 9080 | Le handler écoutera sur le port 8090
use_threadpool = True | Utilise un nombre limité de fils d’exécution (10 par défaut) pour ce handler. Chaque fil gère une requête à la fois.
threadpool_kill_thread_limit = 10800 | Le nombre de secondes pendant lesquelles un fil peut s’exécuter avant d’être tué automatiquement (3 heures)
]

---

### Re-lancez votre instance en mode daemon

```shell
[galaxy]$ GALAXY_RUN_ALL=1 ./run.sh --daemon
```

Votre instance Galaxy fonctionne à présent avec deux handlers : un sur le port `8080` et un sur le port `9080`

--

### Vérifiez vos logs

```shell
[galaxy]$ tail -f main.log
[galaxy]$ tail -f job.log
```

Deux serveurs Galaxy ont été lancés et vous pouvez accéder à chacun d’entre eux en utilisant le port correspondant. Les deux serveurs ont la même vue de vos données ou de vos jobs.

---

### Créer un fichier de configuration des jobs en utilisant le modèle basic de Galaxy

```shell
[galaxy]$ cp config/job_conf.xml.sample_basic config/job_conf.xml
```

---

### Déclarez un nouveau handler en tant que handler de jobs en vous basant sur le lanceur de job local

Editez votre fichier job_conf.xml comme suit :

```xml
<?xml version="1.0"?>
<!-- A sample job config that explicitly configures job running the way it is configured by default (if there is no explicit config). -->
<job_conf>
    <plugins>
        <plugin id="local" type="runner" load="galaxy.jobs.runners.local:LocalJobRunner" workers="1"/>
    </plugins>
    <handlers>
        <handler id="job"/>
    </handlers>
    <destinations>
        <destination id="local" runner="local"/>
    </destinations>
</job_conf>
```

---

### Activez votre configuration des jobs dans votre fichier `galaxy.ini`

```ini
job_config_file = config/job_conf.xml
```

--

### Re-lancez votre instance Galaxy

```shell
[galaxy]$ GALAXY_RUN_ALL=1 ./run.sh --stop-daemon
[galaxy]$ GALAXY_RUN_ALL=1 ./run.sh --daemon
```

---

### Re-lancez l’outil **Change case** sur votre instance Galaxy et vérifiez quel handler exécute le job

.center[![Re-lancement de l'outil Change case](images/rerun-job.png)]

```shell
[galaxy]$ tail -f job.log
```

---

template: title

# Configuration

### Le cycle de vie des données

---

layout: true
template: content

# Le cycle de vie des données

---

Galaxy utilise une base de données pour stocker toutes les informations concernant les utilisateurs, les jobs, les workflows etc. Cependant, chaque fichier que vous chargez sur Galaxy (Upload) ou générez par un outil est stocké sur le système de fichier du serveur et non en base de données.
Ces fichiers sont appelés datasets.
Galaxy conserve une référence de chaque fichier en base de données afin d’y associer des méta-données comme le type de données ou les droits d’accès.

### La base de données de Galaxy
Par défaut, Galaxy utilise une base de données SQLite.
Dans le cadre du G4B, nous avons mis en place une base de données PostgreSQL pour galaxy.
Une base de données PostgreSQL est mieux adapté pour un environnement de développement complet ou un serveur de production.

Retrouvez le chemin de votre base de données SQLite dans votre fichier de configuration `galaxy.ini`

```ini
database_connection = postgresql://galaxy:azerty@localhost/galaxy
```

---

### Datasets et méta-données

Galaxy utilise un identifiant unique (UUID) pour chaque dataset. Vous pouvez retrouvez l’identifiant unique d’un dataset sur la vue **Details** d’un dataset depuis votre historique Galaxy.

### Retrouvez l’identifiant unique du second dataset de votre historique

.center[![La vue Details d'un dataset](images/uuid.png)]

---

### Datasets et méta-données

Par défaut, Galaxy stocke tous vos datasets dans le dossier `galaxy/database/files`.

Vous pouvez modifier cet emplacement en éditant la propriété `file_path` dans votre fichier `galaxy.ini`.

Si vous modifiez l’emplacement des datasets, n’oubliez pas de déplacer tous les datasets existants vers le nouvel emplacement.

Galaxy organise automatiquement vos datasets en sous-dossiers correspondant à l’identifiant du dataset en base de données. Chaque sous-dossiers peut contenir jusqu’à 1000 datasets.
Par exemple, le dataset avec l’identifiant de base de données `4567` sera enregistré dans le sous-dossier `004`.

La base de données de Galaxy permet de faire le lien entre un identifiant unique (UUID) d’un dataset et son identifiant de base de données.

---

.center[![UUI ID Dataset](images/uuid-id-dataset.png)]

---

### Retrouvez l’identifiant de base de données de votre dataset dans votre base de données PostgreSQL

Accédez votre base de données à l’aide de l’outil `psql` :

```shell
[galaxy]$ psql -U galaxy galaxy
```

Cherchez l’identifiant de base de données correspondant à l’UUID de votre dataset :

```sql
galaxy=> select id from dataset where uuid=’votreUUIDSansTiret’;
```

### Localisez dans votre dossier galaxy/databases/files le fichier correspondant à votre dataset.

---

.row[
.col-md-1[
.label.label-primary.bigger[1]
]
.col-md-11[
Un identifiant unique est généré par Galaxy pour l’exécution de l’outil ainsi que pour les fichiers de sortie.<br/>
Par exemple `3234` pour le job et `6567` pour le fichier de sortie
]
]

--

.row[
.col-md-1[
.label.label-primary.bigger[2]
]
.col-md-11[
Un dossier d’exécution est créé pour le job dans votre dossier `job_working_directory` (jwd)<br/>
*Par exemple jwd/003/3234*
]
]

--

.row[
.col-md-1[
.label.label-primary.bigger[3]
]
.col-md-11[
Un script est créé pour l’exécution de l’outil<br/>
*Par exemple jwd/003/3234/galaxy_3234.sh*
]
]

--

Galaxy indique automatiquement les chemins vers les fichiers d’entrée de l’outil (localisé dans un sous-dossier de votre dossier files) et les fichiers de sortie.
Les outils travaillent généralement directement à partir du fichier original et écrivent le ou les fichiers de sortie à leurs emplacements finaux.
En fonction de l’outil utilisé, certains fichiers intermédiaires peuvent être créés dans le dossier temporaire de votre instance ou dans le dossier d’exécution du job.

---

.row[
.col-md-1[
.label.label-primary.bigger[4]
]
.col-md-11[
La sortie standard et la sortie d’erreur du fichier sont stockées dans un fichier .o et .e dans le dossier d’exécution du job<br/>
*Par exemple `jwd/003/3234/galaxy_3234.o` et `jwd/003/3234/galaxy_3234.e`*<br/>
Le code d’erreur de l’exécution de l’outil est stocké dans un fichier .ec dans le dossier d’exécution du job<br/>
*Par exemple `jwd/003/3234/galaxy_3234.ec`*
]
]

--

.row[
.col-md-1[
.label.label-primary.bigger[5]
]
.col-md-11[
Galaxy inscrit les méta-données correspondant aux données générés par l’outil dans la base de données de l’instance.<br/>
Une trace des méta-données générés est conservée dans le dossier d’exécution du job.<br/>
*Par exemple `jwd/003/3234/metadata_in_HistoryDatasetAssociation_6567_xZfXu`*
]
]

--

.row[
.col-md-1[
.label.label-primary.bigger[6]
]
.col-md-11[
En fonction des choix de l’administrateur, Galaxy peut nettoyer le contenu du dossier d’exécution du job à la fin de son exécution.
]
]

--

.callout.callout-success[
Pour une instance Galaxy de production, il est conseillé de ne supprimer le dossier d’exécution des jobs que si le job a réussi.
]

---

### Conserver les traces d’exécution d’un job

Par défaut, Galaxy supprime toutes les traces d’exécution d’un job, que celui-ci ait réussi ou non.
Les informations consultable depuis l’interface web sont stockées en base de données, mais tous les fichiers intermédiaires générés par le job sont supprimés.

Pour conserver les fichiers créés par l’exécution du job dans votre dossier `job_working_directory` lorsqu’un job s’est terminé en erreur, vous pouvez modifier l’option `cleanup_job` dans votre fichier `galaxy.ini`.

```ini
# Clean up various bits of jobs left on the filesystem after completion. These
# bits include the job working directory, external metadata temporary files,
# and DRM stdout and stderr files (if using a DRM). Possible values are:
# always, onsuccess, never
cleanup_job = onsuccess
```

---

template: title

# Configuration

### Tracer les erreurs

---

layout: true
template: content

# Tracer les erreurs

---

### Visualiser le chemin exact de chaque dataset depuis l’interface web

Les administrateurs de la plate-forme peuvent visualiser le chemin exact d’un dataset directement depuis l’interface web de Galaxy

.center[![Chemin d'un dataset](images/dataset-path.png)]

---

### Consulter le debug d’un outil depuis l’interface web

Relancer l’outil **Change case** en indiquant une valeur incorrecte pour le champ **Change case of columns** (par exemple `0.5`)

.center[![Erreur d'exécution](images/tool-error.png)]

---

### Consulter le debug d’un outil depuis l’interface web

.center[![Erreur d'exécution](images/tool-debug.png)]

---

### Consulter les logs de l’instance

Un fichier de log est généré pour chaque handler de votre instance dans votre répertoire `galaxy`.

Vous pouvez suivre en temps réel les logs de votre instance grâce à la commande `tail -f`

```shell
[galaxy]$ tail -f main.log
```

La mise en place d’un handler dédié à l’exécution des outils permet de tracer plus facilement les erreurs liées à l’exécution d’un outil.

```shell
[galaxy]$ tail -f job.log
```

---

template: title

# Configuration

### Déléguer le chargement des données

---

layout: true
template: content

# Déléguer le chargement des données

---

Une instance Galaxy va recevoir et générer un grand volume de données.

Afin d'améliorer les performances et capacité de téléversement et téléchargement, Galaxy peut s'interface avec des outils triers :

- FTP : Galaxy s'interface bien avec le serveur `proFTPd` pour permettre aux utilisateurs de charger leurs données via une connexion FTP. Les données ainsi chargé sont accessible depuis l'outil d'**Upload** de données et pour être importée dans un historique.

.center[![Erreur d'exécution](images/ftp-upload.png)]

Pour plus d'information consultez [la documentation officielle](https://wiki.galaxyproject.org/Admin/Config/UploadviaFTP?action=show&redirect=Admin%2FConfig%2FUpload+via+FTP).

---

- HTTP : Le serveur HTTP de Galaxy n'est pas performant pour permettre le téléversement ou le téléchargement de gros volume de données. Il est cependant possible de déléguer ces tâches à votre serveur proxy (Apache ou NGiNX).


Pour plus d'information consultez la documentation officielle [pour Apache](https://wiki.galaxyproject.org/Admin/Config/ApacheProxy?action=show&redirect=Admin%2FConfig%2FApache+Proxy) ou [NGiNX](https://wiki.galaxyproject.org/Admin/Config/nginxProxy).
