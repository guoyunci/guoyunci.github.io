1. 拉取镜像
```shell script
docker pull star7th/showdoc
```
2. 创建文件夹
```shell script
mkdir -p /home/doc/html
```

3. 运行docker命令
```shell script
docker run -d \
  --name=doc \
  --user=root \
  --privileged=true \
  -p 4999:80 \
  -v /home/doc/html:/var/www/html/ \
  star7th/showdoc
```

4. nginx配置

新增/usr/local/nginx/conf/vhost/doc4999.conf
```shell script
server {
        listen       80;
        listen       443 ssl;
        server_name demo.com;
        ssl_certificate   *.pem;
        ssl_certificate_key  *.key;
        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
        ssl_prefer_server_ciphers on;
        ssl_ciphers "TLS13-AES-256-GCM-SHA384:TLS13-CHACHA20-POLY1305-SHA256:TLS13-AES-128-GCM-SHA256:TLS13-AES-128-CCM-8-SHA256:TLS13-AES-128-CCM-SHA256:EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+AES128:RSA+AES128:EECDH+AES256:RSA+AES256:EECDH+3DES:RSA+3DES:!MD5";
        ssl_session_cache builtin:1000 shared:SSL:10m;

        location / {
                proxy_pass http://127.0.0.1:4999;
        }
}
```



