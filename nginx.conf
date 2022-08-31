events {
  worker_connections  1024;
}

http {
  
  proxy_send_timeout 120;
  proxy_read_timeout 300;
  proxy_buffering    off;
  keepalive_timeout  5 5;
  tcp_nodelay        on;
  # disable any limits to avoid HTTP 413 for large image uploads
  client_max_body_size 0;


  server {
      listen   *:80;

      location ~ ^/(v1|v2)/[^/]+/?[^/]+/blobs/ {
          if ($request_method ~* (POST|PUT|DELETE|PATCH|HEAD) ) {
              rewrite ^/(.*)$ /repository/docker-hosted/$1 last;
          }
          rewrite ^/(.*)$ /repository/docker-group/$1 last;
      }

      location ~ ^/(v1|v2)/ {
          if ($request_method ~* (POST|PUT|DELETE|PATCH) ) {
              rewrite ^/(.*)$ /repository/docker-hosted/$1 last;
          }
          rewrite ^/(.*)$ /repository/docker-group/$1 last;
      }

      location / {
          proxy_pass http://nexus.example.com:8081/;
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto "http";
      }
  }
} 
