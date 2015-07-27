## Métodos de balanceo

+ **Round-Robin**: La petición a los servidores se distribuyen al estilo round-robin
+ **Least-connected**: La siguiente petición se asigna al servidor con menos conexiones activas
+ **ip-hash**: Se usa una función *hash* para determinar el servidor al que debería enviarse la siguiente petición, basandose en la ip del cliente

