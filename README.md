Gérez vos conteneurs avec le Docker Compose

Docker Compose est un outil permettant de définir le comportement de vos conteneurs et d’exécuter des applications Docker à conteneurs multiples. La config se fait à partir d'un fichier YAML, et ensuite, avec une seule commande, vous créez et démarrez tous vos conteneurs de votre configuration.

docker-compose --version // Pour visualiser la version de docker-compose

Exemple:
Pour lancer une appli web et une bd via docker-compose

Création du docker-compose
Contenu du docker-compose

version: '3.7'

services: //Les services ne sont en réalité que des conteneurs. Dans notre cas nous aurons besoin d'un service pour notre base de données et un autre pour notre application web.
    db:
        image: mysql:5.7
        container_name: mysql_c
        restart: always
        volumes:
            - db-volume:/var/lib/mysql
            - ./articles.sql:/docker-entrypoint-initdb.d/articles.sql
        environment:
            MYSQL_ROOT_PASSWORD: test
            MYSQL_DATABASE: test
            MYSQL_USER: test
            MYSQL_PASSWORD: test

    app:
        image: myapp
        container_name: myapp_c
        restart: always
        volumes:
            - ./app:/var/www/html
        ports:
            - 8080:80
        depends_on:
            - db

volumes:
    db-volume:   //Enfin, je demande au moteur Docker de me créer un volume nommé db-volume, c'est le volume pour stocker les données de notre base de donné

Placez vous au niveau du dossier qui contient le fichier docker-compose.yml. Ensuite lancez la commande suivante pour exécuter les services du docker-compose.yml :;
docker-compose up -d


