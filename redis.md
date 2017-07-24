# Redis

## Instalar

```
sudo apt-get install redis-server
redis-cli monitor
redis-cli
```

## Configurar
```
sudo vim /etc/redis/redis.conf
```

## Manipular Datos
- Crear/Modificar un key:value
```
SET key1 value
```

- Obtener el valor de un key
```
GET key1
```

- Otros comandos utiles para key:value
```
DEL key
SETNX (set if not exists) key
SET key2 10
INCR key2 (atomic)
INCR key2
INCR key2
GET key2
```

- Nota sobre INCR y otras operaciones atómicas:
```
x = GET users (13)
y = GET users (13)

x = x + 1 (x es 14)
y = y + 1 (y es 14)

SET users x (users es 14)
SET users y (users es 14)
```

## Listas:
Ordenadas por como se llenan
- Crear y manipular listas:
```
LPUSH users ricardo (users es 'ricardo')
LPUSH users luisa (users es 'luisa','ricardo')
LPUSH users pedro (users es 'pedro','luisa','ricardo')
```
- Ver elementos de listas:
```
LRANGE users 0 1
LRANGE users 0 -1
```

## Sets
Compuestas por elementos únicos, sin orden
- Crear y manipular sets:
```
SADD pages a #Anadir
SADD pages b
SADD pages foo
SADD pages bar
SCARD pages => 4 #Cardinalidad
SMEMBERS myset #bar,a,foo,b #miembros
```
- Interseccion de sets:
```
SADD mynewset b
SADD mynewset foo
SADD mynewset hello
SINTER pages mynewset #foo,b
```
- Verificar pertenencia de elementos:
```
SISMEMBER pages foo #1
SISMEMBER pages xxxxxxxxx #0
```

## Sets ordenados
Deben llevar como valor un float.
- Manipular sets ordenados:
```
ZADD ranks 10 home
ZADD ranks 5 hello-world
ZADD ranks 12.55 trending
ZRANGE ranks 0 -1 #b,a,c
```
- Ver pertenencia de elementos:
```
ZSCORE ranks home #10
ZSCORE ranks sitemap #NULL
```

## Hashes
Simplemente hashes
- Manipular hashes:
```
HMSET user_1 name Ricardo surname Vegas country Spain
HGET user_1 surname #Vegas
```

## Ejemplo rapido de crear usuarios
- Queremos tener un counter de usuarios:
```
SET next_user_id 1000
INCR next_user_id 
```
- Creamos un usuario:
```
HMSET user:1001 username rvegas password 1234 name ricardo last_name
```
- Creamos un hash de usuarios -> id, asi podemos obtener el id de un usuario a traves de su ID:
```
HSET users rvegas 1001 # HSET es como HMSET pero con un solo campo
```
- Paginas creadas por un usuario:
```
SADD pages:1001 bio
SADD pages:1001 stories
SMEMBERS pages:1001
SCARD pages:1001
```
- Followers de un usuario:
```
ZRANGE followers:1001 # ids de followers en orden
ZADD followers:1001 1500911298 1005 # Añadir follower 1005 en timestamp 1500911298
ZREM followers:1001 1016
ZSCORE followers:1001 1005 # 1500911298
ZSCORE followers:1016 # null
```
- Autenticacion:
```
HSET user:1001 auth fea5e81ac8ca77622bed1c2132a021f9 # El token negociado se guarda. ojo que el set existe, solo anado un campo
```
- Mapeo/storage de tokens:
```
HSET auths fea5e81ac8ca77622bed1c2132a021f9 1001
```
- Procedimiento de login:
```
Obtener username y password
Existe el username en `users`?
Obtener el hash de `user:XXXX` donde XXXX es el valor que guardamos para el campo username (rvegas).
Verificar si los passwords coinciden.
Generar token
Ok logged in! Guardar token en user:1001 (como un campo del hash)
Entregar cookie con el token.
```

## Extra
- http://docs.grafana.org/features/datasources/influxdb
