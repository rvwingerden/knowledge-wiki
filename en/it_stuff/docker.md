# All about Docker

## Theorie

abc

## handige Commando's

- `docker ps` : Show all running containers (gebruik -a om ook de containers in rust te zien);
- `docker exec -ti frontend /bin/bash` : start een interactieve sessie naar de frontend container en gebruik bash;
- `docker ps -qa | xargs -r docker rm -f` : docker remove all containers;
- `docker images -q -f "dangling=true" | xargs -r docker rmi` : remove docker images

