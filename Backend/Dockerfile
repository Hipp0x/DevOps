# Partie du build de l'application
# On se base sur l'image maven:3.8.6-amazoncorretto-17
FROM maven:3.8.6-amazoncorretto-17 AS myapp-build

# On définit les variables d'environnements
ENV MYAPP_HOME /opt/myapp

# On définit le répertoire de travail
WORKDIR $MYAPP_HOME
COPY pom.xml .
COPY src ./src

# On compile le code
RUN mvn package -DskipTests


# Partie du run de l'application
# On se base sur l'image amazoncorretto:17
FROM amazoncorretto:17

# On définit les variables d'environnements
ENV MYAPP_HOME /opt/myapp

# On définit le répertoire de travail
WORKDIR $MYAPP_HOME
COPY --from=myapp-build $MYAPP_HOME/target/*.jar $MYAPP_HOME/myapp.jar

ENTRYPOINT java -jar myapp.jar
