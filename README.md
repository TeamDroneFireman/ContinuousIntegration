# Continuous Integration

## Docker

##### Build
Pour construire les images que nous allons utiliser :
```
docker build -t <nom_de_l_image> /lien/vers/le/repertoire/.
```

## Jenkins

[![Jenkins on Quay](https://quay.io/repository/teamdronefireman/jenkins/status "Docker Repository on Quay")](https://quay.io/repository/teamdronefireman/jenkins)

[![Data Volume on Quay](https://quay.io/repository/teamdronefireman/jenkins-volume/status "Docker Repository on Quay")](https://quay.io/repository/teamdronefireman/jenkins-volume)

### Persistence des données
Afin de conserver les données, on va utiliser un *data-volume container* pour y écrire les données du jenkins, ce volume sera conservé entre deux démarrage du  conteneur de jenkins.

##### Lancer le conteneur
###### vide
```
docker create --name=jenkins-data thuchede/sit-volume-jenkins
```

###### exporter les données
Après avoir éteint/perdu le conteneur de notre jenkins
```
docker run --rm --volumes-from=jenkins-data -v $(pwd):/backup busybox \
tar cvf /backup/save.tar /var/log/jenkins/jenkins.log /var/jenkins_home
```
###### charger les données
Après avoir recréer le conteneur de données :
```
docker run --rm --volumes-from=jenkins-data -v $(pwd):/backup busybox \ tar xvf /backup/save.tar var/log/jenkins/jenkins.log var/jenkins_home
```


### Serveur


Pour lancer le serveur:
```
docker run -p 8083:8080 -p 50000:50000 --name=jenkins-master \
--volumes-from=jenkins-data -d <nom_de_l_image>
```

##### Accès
Pour des raisons de sécurité, l'url ne sera disponible que via notre wiki, merci de s'y réferer.

##### Se connecter

Une inscription est nécessaire pour pouvoir se connecter. Les inscriptions seront bientôt ouvertes. Par la suite, l'accès au Jenkins se fera via l'identifiant et le mot de passe que vous aurez choisis.

##### Configurer/Lancer un job

Après connexion, sélectionner un job.
Sur la partie droite il suffit respectivement de cliquer sur *Configurer* ou *Lancer un build* pour effectuer la tache en question.

##### Lire les logs du jenkins

Afficher les logs en temps réel
```
docker exec jenkins-master tail -f /var/log/jenkins/jenkins.log
```
Afficher les logs à un instant donnée
```
docker exec jenkins-master cat /var/log/jenkins/jenkins.log
```

## Docktor

SOON™

### Liens:

 * Une liste de commandes utiles sous docker est disponible [ici](http://http://www.relatably.com/m/img/still-waiting-memes/66259065.jpg)
 * Plus d'info [là-bas](http://lmgtfy.com/?q=docker+doc)
