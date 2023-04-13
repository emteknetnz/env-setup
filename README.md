This is a quick start to get silverstripe setup on docker

It's recommended that once it's setup you delete the .git folder and git init a new project with a different name and git commit that to your own github account

# Setup docker

`git clone git@github.com:emteknetz/env-setup mycms`

If you wish to change the name of the installation folder, you'll also need to update the following in docker-compose.yml

```yml
    volumes:
      - ../mycms/:/var/www
```

Add the following to `/etc/hosts` as a sudo user
```
10.0.50.50 mycms.test
```

# Install new silverstripe project

Note, the following step will give a "same file" warning, which you can safetly ignore

```
composer create-project --no-install silverstripe/installer:^4 _project
cp -r _project/{.,}* .
rm -rf _project
composer install
```

# Commands

## spin-up docker container
docker-compose up --build

## spin-up docker container as detached process
docker-compose up --build -d

## spin-down docker container
docker-compose down

## ssh in as root
docker exec -it mywebserver /bin/bash

## ssh in as www-data
docker exec -it mywebserver sh -c "cd /var/www && su -s /bin/bash www-data"

## mysql
mysql -uroot -proot -h0.0.0.0 -P336 -DSS_mysite
