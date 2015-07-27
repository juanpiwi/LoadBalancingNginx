## Métodos de balanceo

+ **Round-Robin**: La petición a los servidores se distribuyen al estilo round-robin
+ **Least-connected**: La siguiente petición se asigna al servidor con menos conexiones activas
+ **ip-hash**: Se usa una función *hash* para determinar el servidor al que debería enviarse la siguiente petición, basandose en la ip del cliente

## Configuración por defecto

```sh
http {
    upstream myapp1 {
        server srv1.example.com;
        server srv2.example.com;
        server srv3.example.com;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://myapp1;
        }
    }
}
```

En este ejemplo, hay 3 instancias de la misma aplicación. ejecutándose en srv1-srv3. Si no se especifíca ningún método de balanceo, se aplica **round-robin**. Todas las peticiones se redirigen al grupo de servidores myapp1 y nginx se encarga de realizar el balanceo Http mediante round-robin

## Balanceo Least Connected

En este método de balanceo, se permite controlar la carga que reciben los servidores. Nginx intentará no sobrecargar un servidor ocupado con demasiadas peticiones, enviándolas a los servidores menos ocupados.

Para activar este método, hay que añadir la directiva **least_conn**:

```sh
upstream myapp1 {
        least_conn;
        server srv1.example.com;
        server srv2.example.com;
        server srv3.example.com;
}
```