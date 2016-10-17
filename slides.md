layout: true
name: title
class: ifb, middle

.footer.center[
Galaxy4Bioinformatics V5
]

---

layout: true
name: content
class: ifb

.footer.center[
Galaxy4Bioinformatics V5
]

---

template: title

# Installation

---

template: content

# Récupération des sources

La façon la plus courante d’installer Galaxy est de télécharger le code source du projet en utilisant l’outil Git

**Récupérer les sources de Galaxy (en utilisant Git)**

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

Lancez le service Galaxy
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
