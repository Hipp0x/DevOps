# DevOps 2024

## TP 1

### Network

    Pour créer un network :
        docker network create app-network

    Le network va permettre d'avoir un environnement dans lequel différents containers tourne pour qu'ils puissent intéragir ensemble

### Database

#### Build de l'image :   
    1. se placer dans le dossier DB
    2. lancer la commande suivante :
        docker build -t database .

#### Run du container de l'image :
    !! RAJOUTER --env-file .env (afin de récupérer les variables pour l'initialisation de la bd) !!

    - sans persistance de la base de données :
        docker run -p 8888:5432 --network app-network --name devops-database database
    - avec persistance de la base de données :
        docker run -p 8888:5432 --network app-network -v /Users/pauline_adam/DevHome/Fac/Master/M2/DevOPS/TP/DB/data:/var/lib/postgresql/data --name devops-database database

#### Connexion à la DB 
    - via le terminal :   
        psql -p 8888 -U usr -d db -h localhost
    - via le web :
        1. lancer adminer :
            docker run -p "8090:8080" --net=app-network  --name=adminer  -d adminer
            (redirection du port donc on peut avoir accès à localhost:8090)
            (si déjà créer mais juste arrêter, relancer le container)
        2. se connecter à localhost:8090
        3. renseigner les informations suivantes :
            serveur : devops-database (nom du container de notre database)
            utilisateur : usr
            mot de passe : pwd
            base de données : db
            
            (on met devops-database et non localhost car les containers sont dans un réseau privés)

### Backend

    On utilise un build multistage afin de pouvoir séparer la partie du build et la partie du run de l'application. Cela nous évite d'avoir 2 Dockerfile.

    Ne pas oublier de modifier "simple-api/src/main/resources/application.yml" :
    jdbc:postgresql://NOM-DU-CONTAINER:PORT-DU-CONTAINER/NOM-DE-LA-DB
    jdbc:postgresql://devops-database:5432/db

#### Build de l'image :   
    1. se placer dans le dossier API
    2. lancer la commande suivante :
        docker build -t api .

#### Run du container de l'image :    
    docker run -p 8080:8080 --network app-network --name devops-api api 

    (on se rattache au network)
    (on redirige la sortie du port afin d'y avoir accès : le port 8080 correspondrait sinon à celui du container et non à celui en local)

### Serveur

#### Build de l'image :   
    docker build -t server .

#### Run du container de l'image :    
    docker run --network app-network -p 8082:80 --name devops-serveur serveur 
    -> se rattacher au network créé précédement
    -> redirigier la sortie du port pour y avoir accès en local (sinon le 80 de base cest dans le container et pas en localhost)

#### Récupération de la config initiale :    
    docker exec serveur cat /usr/local/apache2/conf/httpd.conf

#### Reverse Proxy
    On utilise un reverse proxy car cela nous permet de rediriger nos urls web vers notre backend

### Docker Compose

    - Se déplacer à la racine du dossier 
    - Lancer la commande suivante :
        docker compose up
        docker compose up --build # pour forcer à build les images
        docker compose down # pour shut down les containers
    - Se connecter à http://localhost:8082/


### Publier les images

    - Pour voir toutes les images disponibles :
        docker images
    
    - Publication de l'image de la database
        docker build -t database .
        docker tag database hipp0x/devops-database:latest   
        docker push hipp0x/devops-database:latest

    - Publication de l'image du backend
        docker build -t api .
        docker tag api hipp0x/devops-api:latest    
        docker push hipp0x/devops-api:latest

    - Publication de l'image du serveur
        docker build -t server .
        docker tag server hipp0x/devops-server:latest   
        docker push hipp0x/devops-server:latest  


## TP2

### Tescontainers: 

    - facilite la création, gestion et suppression de conteneur Docker pour les tests
    - permet d'avoir un environnement de tests isolé proche de la production
    - facilite les tests d'intégration et d'acceptance

### Github Actions

    2 Workflows :

        1. Lancement des tests du backend
        2. Publication des images sur Docker Hub

### Sonar

    - New Code:

        - New Issues:

        - Accepted Issues:

        - Coverage:

        - Duplications:

        - Security Hostpots:

    
    - Overall Code: 

        - Security:

        - Reliability:

        - Maintenability:

        - Accepted Issues:

        - Coverage:

        - Duplications:

        - Security Hostpots:

    
## TP3 

### Introduction

    Nom de serveur: pauline92.takima.cloud

    Se connecter à Centos:
        ssh -i id_rsa centos@pauline92.takima.cloud


    Se déplacer dans le dossier ansible pour lancer les commandes

    Tester le ping de Centos:
        ansible all -i inventories/setup.yml -m ping

    Demander l'OS de Centos:
        ansible all -i inventories/setup.yml -m setup -a "filter=ansible_distribution*"

    Supprimer Apache de Centos:
        ansible all -i inventories/setup.yml -m yum -a "name=httpd state=absent" --become

### PlayBook

    Exectuer le playbook:
        ansible-playbook -i inventories/setup.yml playbook.yml

    Créer un rôle nommé "docker":
        ansible-galaxy init roles/docker

    Pour se connecter au serveur:
        http://pauline92.takima.cloud/
        http://pauline92.takima.cloud/departments/IRC/students

### Front

    Se déplacer dans le dossier Front

    - Build et Publication de l'image du front
        docker build -t frontend .
        docker tag frontend hipp0x/devops-frontend:latest   
        docker push hipp0x/devops-frontend:latest 


    La connexion avec le Front n'est faite que pour la machine centos.

    