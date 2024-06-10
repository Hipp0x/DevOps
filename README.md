# DevOps 2024

## TP 1

### Database

#### Build de l'image :   
    1. se placer dans le dossier DB
    2. lancer la commande suivante :
        docker build -t hipp0x/database .

#### Run du container de l'image :
    - sans persistance de la base de données :
        docker run -p 8888:5432 --network app-network --name database hipp0x/database
    - avec persistance de la base de données :
        docker run -p 8888:5432 --network app-network -v /Users/pauline_adam/DevHome/Fac/Master/M2/DevOPS/TP/DB/data:/var/lib/postgresql/data --name database hipp0x/database

#### Connexion à la DB 
    - via le terminal :   
        psql -p 8888 -U usr -d db -h localhost
    - via le web :
        1. lancer adminer :
            docker run -p "8090:8080" --net=app-network  --name=adminer  -d adminer
            (redirection du port donc on peut avoir accès à localhost:8090)
        2. se connecter à localhost:8090
        3. renseigner les informations suivantes :
            serveur : database
            utilisateur : usr
            mot de passe : pwd
            base de données : db
            
            (on met database et non localhost car les containers sont dans un réseau privés)

### Backend

#### Build de l'image :   
    1. se placer dans le dossier DB
    2. lancer la commande suivante :
        docker build -t hipp0x/backend .

#### Run du container de l'image :    
    docker run -p 8080:8080 --network app-network --name backend hipp0x/backend 

    (on se rattache au network)
    (on redirige la sortie du port afin d'y avoir accès : le port 8080 correspondrait sinon à celui du container et non à celui en local)

### Serveur

#### Build de l'image :   
    docker build -t hipp0x/serveur .

#### Run du container de l'image :    
    docker run --network app-network -p 8082:80 --name serveur hipp0x/serveur 
    -> se rattacher au network créé précédement
    -> redirigier la sortie du port pour y avoir accès en local (sinon le 80 de base cest dans le container et pas en localhost)

#### Récupération de la config initiale :    
    docker exec serveur cat /usr/local/apache2/conf/httpd.conf

### Docker Compose

    - Se déplacer à la racine du dossier 
    - Lancer la commande suivante :
        docker compose up
    - Se connecter à http://localhost:8082/

### Publier les images

    - Pour voir toutes les images disponibles :
        docker images
    
    - Publication de l'image de la database
        docker tag hipp0x/database hipp0x/database:1.0   
        docker push hipp0x/database:1.0

    - Publication de l'image du backend
        docker tag hipp0x/backend hipp0x/backend:1.0    
        docker push hipp0x/backend:1.0

    - Publication de l'image du serveur
        docker tag hipp0x/serveur hipp0x/serveur:1.0    
        docker push hipp0x/serveur:1.0  


## TP2

    tescontainers: 
        - facilite la création, gestion et suppression de conteneur Docker pour les tests
        - permet d'avoir un environnement de tests isolé proche de la production
        - facilite les tests d'intégration et d'acceptance

     