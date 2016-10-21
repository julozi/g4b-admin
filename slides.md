class: center, middle

![Logo IFB](images/ifb-logo.png)

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

### Se donner les droits administrateur

---

layout: true
template: content

# Se donner les droits administrateur

---

### Création d'un compte utilisateur

Depuis l’interface web de Galaxy, créez-vous un compte en cliquant sur le menu **User** puis **Register**.

.callout.callout-info[
#### A noter
Galaxy attend une adresse mail comme login.
]

.center[![Erreur d'exécution](images/user-menu.png)]

---

Les administrateurs d’une instance Galaxy sont déclarés dans le fichier `galaxy.ini`

```ini
admin_users = prenom.nom@domaine.fr
```

.callout.callout-info[
- Il s’agit du mail Galaxy
- Il est possible d’ajouter plusieurs administrateurs en séparant les mails par des virgules.
- Il est nécessaire de relancer Galaxy pour que ces modifications soient prises en compte.
]

---

template: content
# Interface d'administration

.row[
.col-md-4[
- Visualisation des data types et data tables
- Gestion des jobs en cours

- Installation d’outil via toolshed
- Relance d’un outil individuel après modification du wrapper XML d’un outil

- Gestion des utilisateurs, groupes et rôles

- Gestion des données et des librairies partagées

- Gestion des quotas d’espace disque
]
.col-md-8[
.image[![Interface d'administration](images/administration.png)]
]
]

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

Voici la configuration à laquelle nous souhaitons arriver :

.center[![Configuration cible](images/handlers.png)]

---

### Déclarez un nouveau handler qui écoutera sur le port `8090`

Dans votre votre fichier `config/galaxy.ini`, ajouter les lignes suivantes à la fin de la section HTTP server (ligne 50)

```ini
[server:job]
use = egg:Paste#http
port = 8090
use_threadpool = True
threadpool_kill_thread_limit = 10800
```

Cette section définit un nouveau handler qui sera lancé par le script `run.sh`

---

.pure-table.pure-table-bordered.smaller-font[
Paramètre | Description
--- | ---
use = egg:Paste#http | Le type de moteur utilisé pour exécuter ce handler
port = 8090 | Le handler écoutera sur le port 8090
use_threadpool = True | Utilise un nombre limité de fils d’exécution (10 par défaut) pour ce handler. Chaque fil gère une requête à la fois.
threadpool_kill_thread_limit = 10800 | Le nombre de secondes pendant lesquelles un fil peut s’exécuter avant d’être tué automatiquement (3 heures)
]

---

### Re-lancez votre instance en mode daemon

```shell
[galaxy]$ GALAXY_RUN_ALL=1 ./run.sh --daemon
```

Votre instance Galaxy fonctionne à présent avec deux handlers : un sur le port `8080` et un sur le port `8090`

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

### Aller plus loin avec les jobs handlers

Le fichier `job_conf.xml` permet de déléguer l’exécution des outils à des systèmes tiers (serveurs distant, clusters, etc.).

.callout.callout-danger[
Si vous souhaitez utiliser un système tier pour l'exécution de vos outils vous devez impérativement veiller à ce que l’accès aux binaires ainsi qu’aux datasets soit identique sur le serveur Galaxy (les handlers) et sur les serveurs réalisant l’exécution des jobs (les runners)
]

.center[![Système de fichiers partagé](images/shared-fs.png)]

---

### Les concepts clés pour configurer l’exécution des jobs :

Le fichier job_conf.xml compte 5 sections principales
- Les plugins <plugins>
- Les handlers <handlers>
- Les destinations <destinations>
- Les outils <tools>
- Les limitations <limits>

---

### Les plugins

La section <plugins> permet de déclarer les différents types de runner que l’on souhaite utiliser.

- **CLI** : Lancement d’une ligne de commande sur un shell (ssh)
- **Condor - Torque/PBS - SLURM** : Support direct de certains gestionnaires de fil
- **DRMA API** : Distributed Resource Management Application API caseompatible avec Condor, Slurm, Grid Engine, Torque/PBS...
- **Local** : Exécution des outils en local sur le serveur executant le job handler

---

### Les plugins

- **Pulsar** (anciennement LWR) : Exécution sur une machine distante (y compris Windows) sans nécessiter de système de fichiers partagé


.center[![Logo de Pulsar](images/pulsar-logo.png)]

---

### Les plugins

Exemple :

```xml
<plugins>
    <plugin id="local" type="runner" load="galaxy.jobs.runners.local:LocalJobRunner"/>
    <plugin id="drmaa" type="runner" load="galaxy.jobs.runners.drmaa:DRMAAJobRunner">
        <param id="drmaa_library_path">/sge/lib/libdrmaa.so</param>
    </plugin>
</plugins>
```

---

### Les handlers

La section `<handlers>` permet de déclarer les handlers Galaxy qui seront en charge de transmettre les jobs aux `runners`.

Chaque `<handler>` est défini par :
- **id** : un identifiant correspondant au nom déclaré dans le fichier `galaxy.ini` pour ce handler (`[server:id]`).<br/>
- **tag** : cet attribut optionnel permet de regrouper un ensemble de handlers similaires.

Exemple :

```xml
<handlers>
  <handler id="job0" tags="simple"/>
  <handler id="job1" tags="simple"/>
  <handler id="job1" tags="advanced"/>
</handlers>
```

---

### Les handlers

Par défaut, chaque handler charge l’ensemble des plugins déclarés dans la section `<plugins>`.

Il est possible de limiter les plugins chargés pour un handler grace à la balise `<plugin>`.

Exemple :

```xml
<handler id="job3" tags="advanced">
	<plugin id="slurm">
</handler>
```
---

### Les destinations

Les destinations permettent de définir différents paramétrages pour les runners que l’on souhaite utiliser.
Chaque destination correspond à un paramétrage pour un runner donné.

Une <destination> est défini par :
- **id** : l’identifiant unique de la destination
- **runner** : l’identifiant du plugin sur lequel elle s’appuie

Elle contient les paramètres spécifiques au runner utilisé.

Il est possible de regrouper un ensemble de destination par l’intermédiaire de l’attribut **tags**.

---

### Les destinations

Exemple :

```xml
<destinations>
  <destination id="sge" runner="drmaa">
    <param id="nativeSpecification">-q galaxy</param>
  </destination>
  <destination id="smallmem" runner="slurm">
    <param id="nativeSpecification">--mem-per-cpu=512</param>
    <resubmit condition="memory_limit_reached" destination="bigmem" />
  </destination>
</destinations>
```

---

### Les tools

Les handlers et les destinations sont définis par défaut pour l’ensemble des outils.

La section `<tools>` permet de définir des choix spécifiques de destinations et de handlers pour certains outils.

Un `<tool>` est défini par  :
- **id** : l’identifiant de l’outil auquel il fait référence
- **handler** : l’identifiant ou le tag des handlers à utiliser
- **destination** : l’identifiant ou le tag de la destination  

Exemple :

```xml
<tool id="baz" handler="special_handlers" destination="bigmem"/>
```

---

### Les limits

La section `<limits>` permet de définir des limitations concernant l’exécutions des jobs afin de mieux gérer la disponibilité des ressources.

Chaque `<limit>` s’applique pour un **id** destination donné ou un ensemble de destinations portant un tag donné.

Elle cible un `type` de limitation :
- `registered_user_concurrent_jobs`
- `unregistered_user_concurrent_jobs`
- `job_walltime`
- `output_size`
- `concurrent_jobs`

---

### Les limits

Exemples :

```xml
<limits>
  <limit type="registered_user_concurrent_jobs">2</limit>
  <limit type="anonymous_user_concurrent_jobs">1</limit>
  <limit type="destination_total_concurrent_jobs" id="local">16</limit>
  <limit type="output_size">10GB</limit>
</limits>
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

template: title

# Configuration

### Gestion des données

---

layout: true
template: content

# Gestion des données

---

### Objects store

Galaxy intègre des options de configuration avancée permettant la gestion de plusieurs solutions de stockage parallèles pour vos données. Ces options sont définies dans la configuration des **objects store**.

.callout.callout-info[
Nous allons créer une configuration objects store pour permettre à votre instance de stocker les datasets dans deux dossiers différents.
]

---

### Créer un fichier `config/object_store_conf.xml` comme suit :

```xml
<?xml version="1.0"?>
<object_store type="distributed">
    <backends>
        <backend id="files1" type="disk" weight="1">
            <files_dir path="database/files"/>
            <extra_dir type="temp" path="database/tmp"/>
            <extra_dir type="job_work" path="database/job_working_directory"/>
        </backend>
        <backend id="files2" type="disk" weight="1">
            <files_dir path="database/files2"/>
            <extra_dir type="temp" path="database/tmp"/>
            <extra_dir type="job_work" path="database/job_working_directory"/>
        </backend>
    </backends>
</object_store>
```

---

Cette configuration définit en object store de type `distributed` avec deux points de stockage de type `disk`.
Un objects store de type `distributed` écrit les datasets de façon aléatoire dans l’un des points de stockage disponible en fonction de leur poid (`weight`). Les datasets sont retrouvés automatiquement par Galaxy sur le point de stockage sur lequel il se trouve.

L’un des points de stockage utilisé existe déjà, il s’agit du point de stockage par défaut des datasets. Vous devez créer le second point de stockage manuellement pour que Galaxy puisse l’utiliser.

### Créer le dossier correspondant au second point de stockage

```shell
[galaxy]$ mkdir database/files2
```

### Activez votre configuration objects store avancée dans votre fichier `galaxy.ini`

```ini
object_store_config_file = config/object_store_conf.xml
```

### Relancez votre instance Galaxy et vérifiez dans quel point de stockage les données sont écrites.

---

### Les types d’object store :

- **Distributed** : sélection aléatoire du backend avec poids
- **Hierarchical** : écriture dans le premier store uniquement

### Les types de backend :

- Disk
- S3
- Swift
- iRods

---

### Une configuration "type" objects store en production

.center[![Configuration objects store type](images/objects-store.png)]

---

### Suppression et purge des données

L'interface de Galaxy offre plusieurs niveau de suppression de données :
- Les données peuvent être marquées comme supprimés. On considère dans ce cas qu'elles sont récupérable.
- Les données peuvent être marquées comme purgés. Dans ce cas elles peuvent être supprimés physiquement des espaces de stockage.

.callout.callout-warning[
#### Attention
Les références des données ne sont **jamais** réellement supprimés de la base de données.
]

Il est fréquent que les utilisateurs ne suppriment pas leurs données en profondeurs.

---

### Entretien des espaces de stockage

Galaxy intègre des scripts permettant d'assurer un bon entretien des espaces de stockage de données. Il est notamment possible de purger des données supprimés afin de libérer physiquement de l'espace sur les volumes de stockage.

Le script principale de nettoyage des données est `scripts/cleanup_datasets/cleanup_datasets.py`.

Il accepte de nombreux paramètres :

- `--days` : âge en nombre de jours de la dernière mise à jour des datasets, historiques ou librairies à suppirmer ou à purger
- `--remove_from_disk` : supprime réellement les datasets des espace de stockage lorsqu'ils sont purgés
- `delete_userless_histories` : marque les datasets et les historiques n'étant plus associé à un utilisateur comme supprimés
- `--purge_histories` : marque les historiques supprimés comme purges et marques l'ensemble des datasets de ces historiques comme supprimés
- `--purge_datasets` : marque les datasets supprimés comme purgés
- `--purge_librairies` : marque les librairies supprimés comme purgés
- `--purge_folders` : marque les dossiers de librairies supprimés comme purgés
- `--delete_datasets` : marque les datasets comme supprimés et purge les données associés.

---

### Entretien des espaces de stockage

Exemple d'utilisation :

```shell
[galaxy]$ ./scripts/cleanup_datasets/cleanup_datasets.py --days=60 --remove_from_disk --purge_datasets
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

### Mise en place d'un proxy Apache

---

layout: true
template: content

# Mise en place d'un proxy Apache

---

### Architecture cible

.center.image[![Architecture cible](images/apache-proxy.png)]

---

### Arrêtez votre instance Galaxy

```shell
[galaxy]$ GALAXY_RUN_ALL=1 ./run.sh --stop-daemon
```

### Commentez le handler web par défaut

Editez votre fichier `galaxy.ini` et commentez la section correspondant au `[server:main]`

```ini
# [server:main]
# use = egg:Paste#http
# use_threadpool = True
# threadpool_kill_thread_limit = 10800
```

---

### Déclarez deux handlers web

Editez votre fichier `galaxy.ini` pour déclarez deux nouvelles sections handler web

```ini
[server:web1]
use = egg:Paste#http
port = 8081
host = 127.0.0.1
use_threadpool = true
threadpool_kill_thread_limit = 10800

[server:web2]
use = egg:Paste#http
port = 8082
host = 127.0.0.1
use_threadpool = true
threadpool_kill_thread_limit = 10800
```

---

### Relancez votre instance

```shell
[galaxy]$ GALAXY_RUN_ALL=1 ./run.sh --daemon
Handling web1 with log file web1.log...
Entering daemon mode
Handling web2 with log file web2.log...
Entering daemon mode
```

### Vérifiez

```shell
[galaxy]$ tail -2 web2.log
Starting server in PID 5343.
serving on http://127.0.0.1:8082
```

---

Pour utiliser Apache comme répartiteur de charge, nous allons utiliser le module [mod_balancer](http://httpd.apache.org/docs/2.2/mod/mod_proxy_balancer.html) et créer **virtualhost** Apache qui fera office de frontal web et redirigera les requêtes vers les web handlers Galaxy (port 8081 et 8082) :

### Editez le fichier `/etc/httpd/conf/httpd.conf`

```apache
<VirtualHost *:80>
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/html
	ServerName localhost
	ErrorLog logs/galaxy-error_log
	CustomLog logs/galaxy-access_log common
	RewriteEngine on
	<Proxy balancer://galaxy>
   		 BalancerMember http://localhost:8081
   	   BalancerMember http://localhost:8082
	</Proxy>
	RewriteRule ^(.*) balancer://galaxy$1 [P]
</VirtualHost>
```

---

### Testez la configuration Apache

```shell
[galaxy]$ sudo service httpd configtest
```

### Relancez le service Apache

```shell
[galaxy]$ sudo service httpd restart
```

### Validez en ouvrant `http://localhost/`

.callout.callout-success[
Vous pouvez aussi consultez les logs d'Apache : `/var/log/httpd/galaxy*log`
]

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

- HTTP : Le serveur HTTP de Galaxy n'est pas suffisamment performant pour permettre le téléversement ou le téléchargement de gros volume de données. Il est cependant possible de déléguer ces tâches à un serveur proxy (Apache ou NGiNX).


Pour plus d'information consultez la documentation officielle [pour Apache](https://wiki.galaxyproject.org/Admin/Config/ApacheProxy?action=show&redirect=Admin%2FConfig%2FApache+Proxy) ou [NGiNX](https://wiki.galaxyproject.org/Admin/Config/nginxProxy).

---

template: title

# Configuration

### Gestion des groupes et des quotas

---

template: content

# Gestion des groupes

Galaxy permet de rassembler un ensemble d’utilisateurs dans un ou plusieurs groupes.<br/>
L’utilisation de groupes permet de faciliter l’affectation des quotas et des rôles à un ensemble d’utilisateurs.

Vous pouvez gérer les groupes de votre instance depuis le menu **Security/Groups** de l’interface d’administration

### Exercice

Créer le groupe Administrators rassemblant les administrateurs de votre instance.

--

.center[![Ajout d'un groupe](images/add-group.png)]

---

layout: true
template: content

# Gestion des quotas

---

Galaxy intègre un mécanisme permettant de limiter l’espace de stockage disponible pour chaque utilisateur. Il s’agit des quotas.

Un quota se caractérise par une quantité d’espace de stockage et un mode d’affectation.<br/>
Il est possible d’affecter un quota à un utilisateur ou un groupe d’utilisateur.<br/>
Un utilisateur ou un groupe peuvent recevoir plusieurs quotas.

La quantité d’espace de stockage est un nombre suivi d’une unité. Par exemple : `1000MB` ou `2GB`
Il est possible de créer un quota “illimité” en utilisant la valeur `unlimited`

Un quota peut-être affecté :
- de façon exact : `=`
- en augmentation du quota actuel : `+`
- en diminution du quota actuel : `-`

La somme des affectations des quotas d’un utilisateur permet de déterminer son quota exact.

---

### Exemple

Les quotas suivants ont été défini sur une instance publique :

.pure-table.pure-table-bordered[
| Nom | Affectation | Quantité | Statut |
| --- | --- | --- | --- |
| Registered users | `=` | 50GB | Par défaut pour les utilisateurs disposant d'un compte |
| Anonymous | `=` | 100MB | Par défaut pour les utilisateurs non authentifiés |
| RNASeq users | `+` | 25GB | &nbsp; |
| Training | `-` | 49GB | &nbsp; |
]

Lors d’une formation, on va créer **un compte utilisateur pour chaque étudiant**.
Chaque compte se verra affecté le quota **Training**.

Le quota d’un étudiant sera donc :<br/>
50GB (par défaut) - 49GB (Training) = 1GB

---

### Activation du système de quotas

Par défaut, le système de quotas est désactivé.

Modifiez le fichier de configuration de votre instance pour activer le système de quotas :

```ini
enable_quotas = True
```

Redémarrez votre instance pour que la modification soit prise en compte

---

La gestion des quotas se fait depuis le menu Quotas de la section Data de l’interface d’administration

### Exercice 1 :
Créer un quota de 10MB pour tous les utilisateurs anonymes.

### Exercice 2 :
Créer un quota de 100MB pour tous les utilisateurs disposant d’un compte.

### Exercice 3 :
Créer un quota de 100MB supplémentaire pour les utilisateurs Administrateurs.
Vous pouvez utiliser le groupe des Administrateurs pour affecter ce quota.

---

template: title

# Configuration

### Reporting

---

layout: true
template: content

# Reporting

---

Galaxy intègre une application web permettant de consulter les statistiques d'utilisation de votre instance. Cette application dispose de sa propre configuration et de son propre serveur.

Copiez le fichier de configuration dédié au reporting

```shell
[galaxy]$ cp config/reports.ini.sample  config/reports.ini
```

Le serveur de rapport écoute sur le port 9001 par défaut

### Modifiez les paramètres de connexion à votre base de données (comme pour le galaxy.ini)

```ini
database_connection = postgresql://galaxy:azerty@localhost/galaxy
```

---

### Lancement du serveur de rapport

```shell
[galaxy]$ ./run_reports.sh --daemon
```

.callout.callout-info[
Le serveur de rapport dispose de son propre fichier de log : `reports_webapp.log`
]

### Arrêt du serveur de rapport

```shell
[galaxy]$ ./run_reports.sh --stop-daemon
```

---

### Les statistiques consultables

.row[
.col-md-4[

** Jobs **

- Today's jobs
- Jobs per day this month
- Jobs in error per day this month
- All unfinished jobs
- Jobs per month
- Jobs in error per month
- Jobs per user
- Jobs per tool
- Errors per tool
]
.col-md-4[

** Workflows **

- Workflows per month
- Workflows per user
]
.col-md-4[

** Users **

- Registered users
- Date of last login
- User disk usage

**System**

- Disk space maintenance
]
]
