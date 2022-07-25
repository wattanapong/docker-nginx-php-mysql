# run http website
1. copy docker-compose.yml from
   1.1. docker-compose.mint.yml when your os is Linux
   1.2. docker-compose.windows.yml when your os is Windows

2. build docker-compose
```
docker-compose up -d
```

# restart nginx service
docker exec -it nginx nginx -s reload

# Generate nginx SSL on docker


1. test csystem.ict.up.ac.th on browser

2. whether step 2 worked well, you can then generate certificate as following command:
```
docker-compose run --rm  certbot certonly --webroot --webroot-path /var/www/html/csystem/certbot/ --dry-run -d csystem.ict.up.ac.th
```
3. rerun without --dry-run
```
docker-compose run --rm  certbot certonly --webroot --webroot-path /var/www/html/csystem/certbot/ -d csystem.ict.up.ac.th
```
4. uncomment 443 port on nginx/default.template.conf (server { } )

*** no need to generate ssl certificate by docker run *** jacoelho/generate-certificate ***

# Renew let's encrypt ssl
docker-compose run --rm  certbot certonly --webroot --webroot-path /var/www/html/csystem/certbot/ -d csystem.ict.up.ac.th
docker-compose run --rm  certbot certonly --webroot --webroot-path /var/www/html/wattanapong/certbot/ -d wattanapong.com
docker-compose run --rm  certbot certonly --webroot --webroot-path /var/www/html/training/certbot/ -d training.wattanapong.com

# check expire date
## based docker
docker-compose run --rm  certbot certificates

## based client access
echo | openssl s_client -connect localhost:443 -servername csystem.ict.up.ac.th 2>/dev/null | openssl x509 -noout -dates

# composer install & update
docker run --rm -v /home/wattanapongsu/php_www/training:/app composer update


# Connect database (MySQL)
1. connect 
```
docker exec -it mysql bash
mysql -u"$MYSQL_ROOT_USER" -p"$MYSQL_ROOT_PASSWORD"
```

2. restore
```
source .env && docker exec -i $(docker-compose ps -q mysqldb) mysql -u"$MYSQL_ROOT_USER" -p"$MYSQL_ROOT_PASSWORD" < "/home/wattanapongsu/mysql.sql"
```

3. backup (dump)
```
mkdir -p data/db/dumps
source .env && docker exec $(docker-compose ps -q mysqldb) mysqldump --all-databases -u"$MYSQL_ROOT_USER" -p"$MYSQL_ROOT_PASSWORD" > "data/db/dumps/db.sql"
```
*** In case can't access mysql, try to delete folder data