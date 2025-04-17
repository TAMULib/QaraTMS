### Original README

https://github.com/a13xh7/QaraTMS

### TAMU Deployment Steps

Build image:
```
docker compose build app --no-cache
```


There is no need to build a brand new nginx image, create a configMap with [this content](https://github.com/TAMULib/QaraTMS/blob/tamu/docker/nginx/conf.d/default.conf) and mount it as a file to an nginx deployment at `/etc/nginx/conf.d/default.conf`.


Upon first startup, start the container as `root` or user `0` and shell into the `app` deployment and run the following commands to initialize the app:
```
cp -a /opt/www/. /var/www/
chown -R www-data:www-data /var/www
chmod -R 755 /var/www/storage
```

Restart the container with user `33` or `www-data` and then run the following commands:
```
php artisan migrate
php artisan db:seed --class=AdminSeeder
```

**Note**: Configure ingress, app url, and database accordingly, and mount persistent storage to both `app` and `nginx` deployments at `/var/www` , and set the `APP_URL` and `FORCE_HTTPS` environment variables accordingly.


