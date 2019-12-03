# Shell Suite

Fichier .bashrc (profile)

:one: Créer votre fichier `.bashrc` sur votre serveur et mettez son contenu dans le fichier .:id:`.rc`

:two: Modifier votre prompt pour afficher `[moi@ip-10-13-237-16 /home/moimeme]$`

* Pemissions

https://www.tutorialspoint.com/unix/unix-file-permission.htm

* Variables d'environnement

https://www.tutorialspoint.com/unix/unix-environment.htm

## Docker Machine

* voir les machines virtuelles disponibles

```
$ docker-machine ls
NAME           ACTIVE   DRIVER    STATE     URL                     SWARM   DOCKER     ERRORS
fianarantsoa   -        generic   Running   tcp://10.13.5.21:2376           v19.03.5   
```

* remettre à zéro

```
$ docker-machine env --unset
unset DOCKER_TLS_VERIFY
unset DOCKER_HOST
unset DOCKER_CERT_PATH
unset DOCKER_MACHINE_NAME
# Run this command to configure your shell: 
# eval $(docker-machine env --unset)
```

<image src="images/docker-engine.png" width="980" height="540"></image>
