# On se base sur l'image postgres:14.1-alpine
FROM postgres:14.1-alpine

# On définit les variables d'environnement de notre base de données
ENV POSTGRES_DB=db \
   POSTGRES_USER=usr \
   POSTGRES_PASSWORD=pwd

# On copie les fichiers SQL dans le répertoire d'initialisation de la base de données
# Permet de remplir la base de données au lancement du container
COPY 01-CreateScheme.sql /docker-entrypoint-initdb.d
COPY 02-InsertData.sql /docker-entrypoint-initdb.d
