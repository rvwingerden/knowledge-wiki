# Sexy webserver NGINX

NGINX is open source software for web serving, reverse proxying, caching, load balancing, media streaming, and more

## Used as reversed proxy server

**_Introduction_**

Proxying is typically used to distribute the load among several servers, seamlessly show content from different websites, or pass requests for processing to application servers over protocols other than HTTP.



**_Passing a Request to a Proxied Server_**

When NGINX proxies a request, it sends the request to a specified proxied server, 
fetches the response, and sends it back to the client. 
It is possible to proxy requests to an HTTP server (another NGINX server or any other server) 
or a non-HTTP server using a specified protocol. 
Supported protocols include FastCGI, uwsgi, SCGI, and memcached.

To pass a request to an HTTP proxied server, 
the proxy_pass directive is specified inside a location. For example:

```json
location /some/path/ {
    proxy_pass http://www.example.com/link/;
}
```

This example configuration results in passing all requests processed in this location to the proxied server at the specified address. This address can be specified as a domain name or an IP address. The address may also include a port:

```json
location ~ \.php {
    proxy_pass http://127.0.0.1:8000;
}```

Note that in the first example above, the address of the proxied server is followed by a URI, /link/. 
If the URI is specified along with the address, it replaces the part of the request URI that matches the location parameter. 
For example, here the request with the `/some/path/page.html` URI will be proxied to `http://www.example.com/link/page.html`. 
If the address is specified without a URI, or it is not possible to determine the part of URI to be replaced, the full request URI is passed (possibly, modified).

To pass a request to a non-HTTP proxied server, the appropriate `**_pass` directive should be used:

- fastcgi_pass passes a request to a FastCGI server
- uwsgi_pass passes a request to a uwsgi server
- scgi_pass passes a request to an SCGI server
- memcached_pass passes a request to a memcached server
Note that in these cases, the rules for specifying addresses may be different. You may also need to pass additional parameters to the server (see the reference documentation for more detail).

The proxy_pass directive can also point to a named group of servers. In this case, requests are distributed among the servers in the group according to the specified method.




**_Passing Request Headers_**
By default, NGINX redefines two header fields in proxied requests, “Host” and “Connection”, and eliminates the header fields whose values are empty strings. “Host” is set to the $proxy_host variable, and “Connection” is set to close.

To change these setting, as well as modify other header fields, use the proxy_set_header directive. This directive can be specified in a location or higher. It can also be specified in a particular server context or in the http block. For example:

```json
location /some/path/ {
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_pass http://localhost:8000;
}```
In this configuration the “Host” field is set to the $host variable.

To prevent a header field from being passed to the proxied server, set it to an empty string as follows:

```json
location /some/path/ {
    proxy_set_header Accept-Encoding "";
    proxy_pass http://localhost:8000;
}```


_**Choosing an Outgoing IP Address**_
If your proxy server has several network interfaces, sometimes you might need to choose a particular source IP address for connecting to a proxied server or an upstream. This may be useful if a proxied server behind NGINX is configured to accept connections from particular IP networks or IP address ranges.

Specify the proxy_bind directive and the IP address of the necessary network interface:

```json
location /app1/ {
    proxy_bind 127.0.0.1;
    proxy_pass http://example.com/app1/;
}```

```json
location /app2/ {
    proxy_bind 127.0.0.2;
    proxy_pass http://example.com/app2/;
}```

The IP address can be also specified with a variable. For example, the $server_addr variable passes the IP address of the network interface that accepted the request:

```json
location /app3/ {
    proxy_bind $server_addr;
    proxy_pass http://example.com/app3/;
}```
