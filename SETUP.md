1. copy docker-compose.yml from
   1.1. docker-compose.mint.yml when your os is Linux
   1.2. docker-compose.windows.yml when your os is Windows


# Generate nginx SSL on docker
1. build docker-compose
```
docker-compose up -d
```

2. test csystem.ict.up.ac.th on browser

3. whether step 2 worked well, you can then generate certificate as following command:
```
docker-compose run --rm  certbot certonly --webroot --webroot-path /var/www/html/csystem/certbot/ --dry-run -d csystem.ict.up.ac.th
```
4. rerun without --dry-run
```
docker-compose run --rm  certbot certonly --webroot --webroot-path /var/www/html/csystem/certbot/ -d csystem.ict.up.ac.th
```
5. uncomment 443 port on nginx/default.template.conf (server { } )


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