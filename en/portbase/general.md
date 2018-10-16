# General stuff


## Database

Database lokaal draait op poort 1522. (in productie op de normale 1521);
Wanneer je de lokale database wil updaten met een liquibase script, dan kun je de pom.xml aanpassen naar poort 1522.

Je kunt ook de database kopieren en laten draaien op poort 1521, om daarmee je wijzigingen in de lokale database te testen.
Voor unit testen hebben we een database nodig maar dan draaiend op poort 1521. 
In de build.sh van de WPCS repository wordt dit gedaan. Wat daar gebeurt kunnen we ook zelf doen.
Stop de database:
docker stop database
docker commit database rdbms:naomi-test

docker run -d -p 1521:1521 --name rdbms rdbms:naomi-test && docker exec -i rdbms /ensure-rdbms-ready.sh


## Benaderen portbase corporate and developer shares


`alias pubcorp='sudo mount "//ad.portbase.com/fileshare/corporate/publicCorp" -t cifs -o uid=1000,gid=1000,username=r.van.wingerden,domain=AD /mnt/pubcorp/'
`
`alias pubdev='sudo mount "//ad.portbase.com/fileshare/developers/publicDev" -t cifs -o uid=1000,gid=1000,username=r.van.wingerden,domain=AD /mnt/pubdev'
`


## Inloggen op de Showcase

- tc@SO1    Test!2345
- pi.administrator@PORTINFOLINK Test!234

