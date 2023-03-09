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
5.  Nous avons supprimé le conteneur précédent et sommes arrivés au même résultat en utilisant ces commandes `docker run --name my_siteweb -d -p 8081:80 httpd` pour creé le conteneur server web
`docker cp $PWD/html my_siteweb:/usr/local/apache2/htdocs`
pour copier le fichier index.html dans le conteneur.
`docker restart my_siteweb`
pour relancer le conteneur avec le nouveau fichier html
![Screenshot](ScreenShots/screenShotBash5E.png)
page html accessible sur http://localhost:8081.
![Screenshot](ScreenShots/screenShotBash5DD.png)