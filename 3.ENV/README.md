# Shell Suite

Fichier .bashrc (profile)

:one: Créer votre fichier `.bashrc` sur votre serveur et mettez son contenu dans le fichier :id:`.rc`

* Pemissions

https://www.tutorialspoint.com/unix/unix-file-permission.htm

* Variables d'environnement

https://www.tutorialspoint.com/unix/unix-environment.htm

## Docker Machine

* voir les machines virtuelles disponibles

```
$ docker-machine ls
NAME   ACTIVE   DRIVER   STATE   URL   SWARM   DOCKER   ERRORS
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

