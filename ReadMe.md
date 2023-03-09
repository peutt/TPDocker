TP Conteneurs Docker
====================

5\. Exécuter un serveur web dans un conteneur Docker
----------------------------------------------------

Pour exécuter un serveur web (apache, nginx, ...) dans un conteneur Docker, nous avons suivi les étapes suivantes :

1.  Nous avons récupéré l'image sur le Docker Hub à l'aide de la commande `docker pull httpd`.
![Screenshot](ScreenShots/screenShotBash5A.png)
2.  Nous avons vérifié la presence de l'image httpd en local à l'aide de la commande `docker image ls`.
![Screenshot](ScreenShots/screenShotBash5B.png)
3.  Nous avons créé un fichier index.html simple dans le dossier html.
4.  Nous avons démarré un conteneur et servi la page HTML créée précédemment à l'aide d'un volume en utilisant la commande `docker run -d -p 8081:80 -v C:/Users/aymer/Desktop/docker/html/index.html:/usr/local/apache2/htdocs/index.html httpd` page html accessible sur http://localhost:8081.
![Screenshot](ScreenShots/screenShotBash5D.png)
![Screenshot](ScreenShots/screenShotBash5DD.png)
5.  Nous avons supprimé le conteneur précédent et sommes arrivés au même résultat en utilisant ces commandes :
- `docker run --name my_siteweb -d -p 8081:80 httpd` pour creé le conteneur server web
- `docker cp $PWD/html my_siteweb:/usr/local/apache2/htdocs` pour copier le fichier index.html dans le conteneur.
- `docker restart my_siteweb` pour relancer le conteneur avec le nouveau fichier html
![Screenshot](ScreenShots/screenShotBash5E.png)
page html accessible sur http://localhost:8081.
![Screenshot](ScreenShots/screenShotBash5DD.png)

6\. Builder une image
----------------------------------------------------
1. Après avoir remplis le dockerfile où l'on indique l'utilisation de la dernière version d'httpd et la commande pour copier le fichier html, on effectue la commande suivante à la racine du projet pour build l'image : `docker build -t my_siteweb -f dockerfile .`
![Screenshot](ScreenShots/screenShotBash6A.png)
2. On effectue la commande docker run sur l'image crée sur le port 8081:80 : `docker run --name my_siteweb -d -p 8081:8
0 my_siteweb`
![Screenshot](ScreenShots/screenShotBash6B.png)
page html accessible sur http://localhost:8081.
![Screenshot](ScreenShots/screenShotBash5DD.png)
3. Les différences entre les procédures 5 et 6 sont que la procédure 5 utilise une image existante, tandis que la procédure 6 crée une nouvelle image à partir d'un Dockerfile. La procédure 6 offre plus de flexibilité et de contrôle sur l'image créée, alors que la procédure 5 est plus stable car cest une image fournis par docker hub.

7\. Utiliser une base de données dans un conteneur docker
----------------------------------------------------
1. ![Screenshot](ScreenShots/screenShotBash7A.png)