# Sexy webserver NGINX

NGINX is open source software for web serving, reverse proxying, caching, load balancing, media streaming, and more

## Used as reversed proxy server

**_Introduction_**

Een reversed proxy in mijn denkwereld is niets anders dan meerdere website bedienen op dezelde poort.
Het onderscheidt kan dan worden gemaakt op basis van de url/headers en parameters.
Bijvoorbeeld: Wiki.js draait op poort 3001, om nu niet altijd de poort mee te geven kunnen we het volgende inregelen:
```json
server {
    listen 80;
    listen [::]:80;
    server_name  wiki.example.com;
​
    location / {
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_pass http://127.0.0.1:3001;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_next_upstream error timeout http_502 http_503 http_504;
    }
}```

Je kunt nu via http://wiki.example.com naar de wiki.js pagina navigeren, ipv localhost:3001

## links

[configuratie NGINX as reversed proxy](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/)