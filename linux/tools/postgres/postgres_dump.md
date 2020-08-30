# ras DB dump-restore手順
## dump手順

* pg_dump -Fc (db名) -U (db user名) > tmp.dump
  * -c: 圧縮形式でdump。なしだとsql形式でdumpされるが、サイズが20倍程度になる

## restore手順

docker run --name db -e POSTGRES_PASSWORD=(pass) -e POSTGRES_USER=(user) -e POSTGRES_DB=(db名) -d postgres
docker cp tmp.dump db:/
docker exec -it db bash
psql -U (user) postgres
#CREATE ROLE root LOGIN CREATEDB CREATEROLE PASSWORD 'pass';
#localedef -i ja_JP -c -f UTF-8 -A /usr/share/locale/locale.alias ja_JP.UTF-8
exit
pg_restore -U (user) -d (db名) tmp.dump