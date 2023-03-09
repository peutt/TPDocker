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
1. On récupères les images docker de mysql et phpmyadmin grace à la commande docker pull ![Screenshot](ScreenShots/screenShotBash7A.png)
2. On lance les conteneurs mysql et phpmyadmin en renseignant :
- les variables d'environements mysql (nom de la BDD et mot de passe) `docker run -d --name mysql_container -e MYSQL_ROOT_PASSWORD=Passw0rd -e MYSQL_DATABASE=tpdocker mysql`
- la liaison entre phpmyadmin et mysql `docker run -d --name phpmyadmin --link mysql_container:db -p 8080:80 phpmyadmin`
![Screenshot](ScreenShots/screenShotBash7B.png)
Ensuite, on se connecte sur phpmyadmin sur http://localhost:8080 grace au mot de passe renseigné prècédement et à l'identifiant 'root' pour ajouter un table et une donnée exemple dans la base 'tpdocker mysql'.
![Screenshot](ScreenShots/screenShotPhpmyadmin7B.png)

8\. Utilisation de docker-compose
---------------------------------
1. Nous avons utilisé un fichier docker-compose.yml pour créer les services et les réseaux.
![Screenshot](ScreenShots/screenShotBash8.png)
Le fichier docker-compose.yml permet de définir et de lancer plusieurs conteneurs avec leurs dépendances en une seule commande (`docker-compose up -d` à l'emplacement du fichier docker-compose.yml). En outre, il permet de spécifier les volumes, les ports et les réseaux nécessaires pour chaque conteneur, simplifiant ainsi la gestion des conteneurs.
2. Nous pouvons déclarer les mots de passe et autres dans environement pour le conteneur mysql dans le fichier docker-compose.yml

9\.Observation de l’isolation réseau entre 3 conteneurs
---------------------------------
1. On modifie le docker-compose.yml en rajoutant les services web, app et db et en les connectant à leurs bon network:
Puis : 
	docker-compose up -d
![Screenshot](ScreenShots/screenShotBash9.png)
2. A l'aide de ces commandes :
	
	docker inspect {containerId or name}
Retour pour web :
```
[
    {
        "Id": "ef30eb641e0790e36ea7e12c118d7a8f3923875b0c566793863e4af5f114f92d",
        "Created": "2023-03-08T14:24:25.4046045Z",
        
        {...}
        
            "Networks": {
                "docker_webserver-db_backend": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": [
                        "web",
                        "web",
                        "ef30eb641e07"
                    ],
                    "NetworkID": "101e107e7d1616e5ad1f1a34d8bb0d4d8f95819f1be1b4815b481b37306e9e83",
                    "EndpointID": "5194af06f60edc5d65767c243acfe67aa2a40459ec0fe4ef3692da797af4c293",
                    "Gateway": "172.23.0.1",
                    "IPAddress": "172.23.0.3",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:17:00:03",
                    "DriverOpts": null
                },
                "docker_webserver-db_frontend": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": [
                        "web",
                        "web",
                        "ef30eb641e07"
                    ],
                    "NetworkID": "ff6e5255d44cb5995ead33c4f0b6cf677c3f667170c91a5c58fa40a05dbe5c64",
                    "EndpointID": "170266cbf054e56b2cbf2947c61eefc3217661adf484315e7a084bf35566a1c8",
                    "Gateway": "172.22.0.1",
                    "IPAddress": "172.22.0.3",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:16:00:03",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

On regarde la ou les lignes Gateway ou bien IpAddress, ou plus directement on peut faire :

	docker inspect -f '{{range .NetworkSettings.Networks}}{{.Gateway}}, {{end}}' db

Retour : `172.22.0.1,`

	docker inspect -f '{{range .NetworkSettings.Networks}}{{.Gateway}}, {{end}}' app

Retour : `172.23.0.1,`

	docker inspect -f '{{range .NetworkSettings.Networks}}{{.Gateway}}, {{end}}' web

Retour : `172.23.0.1, 172.22.0.1,`

  

On voit que db n'est pas sur le même reseau que app, et que web est sur les deux.

3. 
Cela permet de limiter la connexion entre les différents services, ce qui permet de renforcer la sécurité et d'isoler les services. Isoler les services permets d'en maintenir debout lorsque une partie de l'infra tombe.