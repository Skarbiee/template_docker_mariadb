# Template Docker MariaDB
Ce projet est un **template Docker** simple pour déployer une base de données **MariaD**B avec une configuration de démarrage rapide, permettant une gestion aisée de vos bases de données dans des environnements Docker. Il est conçu pour être un modèle réutilisable pour différents projets nécessitant une instance MariaDB avec initialisation via un script SQL.

## Structure du projet

```
./
├── docker-compose.yml        # Fichier de configuration Docker Compose
├── README.md                 # Ce fichier README
└── mariadb/                  # Dossier contenant les configurations de MariaDB
    └── init.sql              # Script SQL pour initialiser la base de données
```
---

## 1. Prérequis

Avant de commencer, assurez-vous que vous avez les éléments suivants installés :

- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/)

## 2. Installation du projet

### Étape 1 : Cloner le projet

Si ce projet n'est pas déjà sur votre machine, vous pouvez le cloner depuis le dépôt Git (ou simplement télécharger les fichiers si vous avez un ZIP).

```bash
git clone https://github.com/Skarbiee/template_docker_mariadb.git
```

### Étape 2 : Lancer le projet avec Docker Compose

Une fois que vous avez cloné le projet, vous pouvez démarrer le conteneur MariaDB avec la commande suivante :

```bash
docker-compose up -d
```

Cela va :
- Télécharger l'image `mariadb:latest` si elle n'est pas déjà présente sur votre machine.
- Créer un conteneur nommé `mariadb_container`.
- Configurer le mot de passe root, créer la base de données `tp-docker_db`, et configurer l'utilisateur `user` avec le mot de passe `password`.

## 3. Accéder à MariaDB depuis le conteneur

Une fois que le conteneur est lancé, vous pouvez vous connecter à MariaDB en utilisant la commande suivante pour accéder au shell MySQL dans le conteneur :

```bash
docker exec -it mariadb_container mysql -u root -p
```

Entrez le mot de passe root (`rootpassword`) lorsque vous y êtes invité.

Si vous préférez utiliser l'utilisateur `user` (mot de passe `password`), vous pouvez vous connecter avec cette commande :

```bash
docker exec -it mariadb_container mysql -u user -p
```

## 4. Vérifier la base de données et son contenu

Une fois connecté à MariaDB, vous pouvez vérifier la base de données et son contenu en exécutant les commandes suivantes :

### Afficher les bases de données disponibles :

```sql
SHOW DATABASES;
```

### Utiliser la base de données `tp-docker_db` :

```sql
USE tp-docker_db;
```

### Afficher les tables de la base de données :

```sql
SHOW TABLES;
```

## 5. Personnalisation de la base de données initiale

Le fichier `mariadb/init.sql` est utilisé pour initialiser la base de données lorsque le conteneur est lancé pour la première fois. Vous pouvez y ajouter vos propres instructions SQL (création de tables, insertion de données, etc.).

### Exemple de contenu pour `init.sql` :

```sql
CREATE DATABASE IF NOT EXISTS tp-docker_db;
USE tp-docker_db;

CREATE TABLE IF NOT EXISTS users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE
);

INSERT INTO users (name, email) VALUES ('Alice', 'alice@example.com');
INSERT INTO users (name, email) VALUES ('Bob', 'bob@example.com');
```

## 6. Persistance des données

Les données de la base de données sont persistées dans un volume Docker (`mariadb-data`), ce qui signifie que même si le conteneur est supprimé, les données restent intactes. Les volumes Docker sont stockés dans le système local, en dehors du conteneur, garantissant la persistance des données entre les redémarrages du conteneur.

## 7. Arrêter et nettoyer les conteneurs

Pour arrêter les conteneurs et nettoyer les ressources associées, vous pouvez utiliser la commande suivante :

```bash
docker-compose down
```

Cette commande arrêtera les conteneurs mais laissera les volumes de données intacts. Si vous souhaitez supprimer les volumes de données (ce qui effacera toutes les données persistantes), utilisez :

```bash
docker-compose down -v
```

## 8. Problèmes courants

- **Le conteneur ne démarre pas** : Vérifiez les logs du conteneur en utilisant la commande suivante pour voir si des erreurs apparaissent lors du démarrage :
  
  ```bash
  docker logs mariadb_container
  ```

- **Base de données non initialisée** : Si vous avez modifié `init.sql`, assurez-vous que les changements sont corrects et que le conteneur a été redémarré correctement. Vous pouvez redémarrer le conteneur avec :

  ```bash
  docker-compose restart
  ```

## 9. Ressources supplémentaires

- [Documentation officielle de MariaDB](https://mariadb.org/documentation/)
- [Documentation Docker Compose](https://docs.docker.com/compose/)

---