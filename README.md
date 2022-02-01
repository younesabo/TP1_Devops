# TP1 Docker

## Database

1 -  On commence par build notre dockerfile qui se trouve dans le dossier courant 
```$ docker build -t younes/pgsql .```

2 -  On run l'image que l'on vient de build
```$ docker run --rm --name pgsql -e POSTGRES_PASSWORD=pwd -v /my/own/datadir:/var/lib/postgresql/data younes/pgsql```
Il n'y a pas besoin de mettre de port, on veut que la base soit ouverte à tous (5432 est le port par défault de postgres).
On donne un nom au conteneur (ici pgsql).
L'option -v  signifie qu'on lie le dossier de notre système aux données du conteneur pour les sauvegarder.
On indique quelle image il faut utiliser (ici younes/pgsql).

3 - Pour vérifier que notre base fonctionne correctement on utilise Adminer, on crée un conteneur Adminer en utilisant --link pour le lier au conteneur pgsql que l'on vient de créer.
```$ docker run --link pgsql -p 8080:8080 adminer```

Identification Adminer :

serveur : pgsql
utilisateur : usr
mdp : 123
base de données : bd

## Backend API

Il nous faut un multistage build pour pouvoir build et run notre application.

1 - On commence par build notre dockerfile 
```$ docker build -t younes/java .```

2 - On run le conteneur en utilisant --link pour lier au conteneur de la base de données
```$ docker run -it --rm --link pgsql --name java younes/java```

3 - Pour tester notre api, on a juste à rajouter l'option ```-p 8080:8080```
```$ docker run -p 8080:8080 -it --rm --link pgsql --name java younes/java```

Lien :

http://localhost:8080/departments/IRC/students

Résultat : 

[{"id":1,"firstname":"Eli","lastname":"Copter","department":{"id":1,"name":"IRC"}}]

## HTTP Server

1 - On commence par build notre dockerfile
```$ docker build -t younes/httpd .```

2 - On run le conteneur en utilisant --link pour lier au conteneur de l'api au port 80
```$ docker run -dit --rm --link java --name httpd -p 80:80 younes/httpd```

3 - On peut tester le serveur : http://localhost

4 - On copie le httpd.conf
```$ docker run --rm httpd:2.4 cat /usr/local/apache2/conf/httpd.conf > my-httpd.conf```

5 - Dans notre httpd.conf on décommente les lignes suivantes :
    LoadModule proxy_module modules/mod_proxy.so
    LoadModule proxy_http_module modules/mod_proxy_http.so


## Docker compose

Docker compose est une commande assez importante, elle nous permet de build, run, etc, l'ensemble de nos images.
```$ docker-compose up```
```$ docker-compose down```

## Publish

Pour publier les images sur Docker Hub :
```$ docker login```
```$ docker tag tp1_backendapi younesabo/tp1_backendapi:latest```
```$ docker tag tp1_database younesabo/tp1_database:latest```
```$ docker tag tp1_httpserver younesabo/tp1_httpserver:latest```
```$ docker push younesabo/tp1_backendapi```
```$ docker push younesabo/tp1_database```
```$ docker push younesabo/tp1_httpserver```


# Images Docker :

* https://hub.docker.com/repository/docker/younesabo/tp1_database
* https://hub.docker.com/repository/docker/younesabo/tp1_httpserver
* https://hub.docker.com/repository/docker/younesabo/tp1_backendapi

# GitHub : 
* https://github.com/younesabo/TP1_Devops.git