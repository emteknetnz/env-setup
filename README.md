This is a quick start to get silverstripe setup on docker

It's recommended that once it's setup you delete the .git folder and git init a new project with a different name and git commit that to your own github account

## Setup

```bash
git clone git@github.com:emteknetz/env-setup mycms
```

If you wish to change the name of the installation folder, you'll also need to update the following in `docker-compose.yml`

```yml
    volumes:
      - ../mycms/:/var/www
```

Add the following to `/etc/hosts` as a sudo user

```
10.0.50.50 mycms.test
```

## Install new silverstripe project

Note, the following step will give a "cp: '_project/..' and '.' are the same file" warning, which you can safely ignore

```bash
composer create-project --no-install silverstripe/installer:^4 _project
rm _project/README.md
cp -r _project/{.,}* .
rm -rf _project
composer install
```

Copy the following into `.env`

```
SS_ENVIRONMENT_TYPE="dev"
SS_DATABASE_CLASS="MySQLDatabase"
SS_DATABASE_SERVER="mariadb"
SS_DATABASE_USERNAME="root"
SS_DATABASE_PASSWORD="root"
SS_DATABASE_NAME="SS_mysite"
SS_DEFAULT_ADMIN_USERNAME="admin"
SS_DEFAULT_ADMIN_PASSWORD="password"
DEBUGBAR_DISABLE=true
SS_TRUSTED_PROXY_IPS="*"
SS_HOST="http://jessie.vagrantup.com"
SS_MFA_SECRET_KEY="92c3090584175b99966561e1efe237e4"
MAILER_DSN="sendmail://default?command=/home/www-data/go/bin/mhsendmail%20-t"
```

## Using the website

`http://mycms.test`

The first time you visit it it will redirect to `/dev/build` which will do an initial build of the database

The use the CMS, navigate to `/admin` and use the credentials `user`, `password`

## Commands

**spin-up docker container**
docker-compose up --build

**spin-up docker container as detached process**
docker-compose up --build -d

**spin-down docker container**
docker-compose down

**ssh in as root**
docker exec -it mywebserver /bin/bash

**ssh in as www-data**
docker exec -it mywebserver sh -c "cd /var/www && su -s /bin/bash www-data"

**mysql**
mysql -uroot -proot -h0.0.0.0 -P3350 -DSS_mysite
