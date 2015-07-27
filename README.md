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

## Session persistence (ip-hash)

Se debe tener en cuenta que con *round-robbin* o *least-connected*, la solicitud de cada cliente subsiguiente puede ser distribuido un servidor diferente. No existe garantía que el mismo cliente se dirija al mismo servidor

Si existe la necesidad de atar a un cliente a un servidor de aplicaciones en particular, se puede utilizar el método **ip-hash** para persistir siempre en un servidor determinado

Con *ip-hash*, la dirección IP del cliente se utiliza como hash para determinar que servidor en un grupo de servidores se debe seleccionar para la solicitudes del cliente. Este método garantiza que las solicitudes de un mismo cliente siempre serán dirigidos al mismo servidor, excepto cuando este servidor no está disponible

Para configura el **ip-hash** load balancing, solo se tiene que agregar la directiva *ip_hash* a la configuración 


```sh
upstream myapp1 {
    ip_hash;
    server srv1.example.com;
    server srv2.example.com;
    server srv3.example.com;
}
```

## Balanceo ponderado

Si se quiere dar más ponderación, es posible usar el parámetro weight en los servidores, para asignarles más “peso” a la hora de tomar decisiones de balanceo:

```sh
 upstream myapp1 {
        server srv1.example.com weight=3;
        server srv2.example.com;
        server srv3.example.com;
    }
```

Con esta configuración, 5 peticiones nuevas se distribuirán de la siguiente forma: 3 a srv1, 1 a srv2 y 1 a srv3.
