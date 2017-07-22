# Elastic Search

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
- Crear y manipular listas:
```
LPUSH users ricardo (users es 'ricardo')
LPUSH users luisa (users es 'luisa','ricardo')
LPUSH users pedro (users es 'pedro','luisa','ricardo')
```

## Insertar Datos en Batch
```
```

## Ver Datos:

- Indices: http://localhost:9200/_cat/indices?v
- Index: http://localhost:9200/shakespeare?pretty=true
- Settings: http://localhost:9200/shakespeare/_settings?pretty=true
- Mappings: http://localhost:9200/shakespeare/_mapping?pretty=true

## Search
- Buscar en el campo por defecto `_all`:
  - <http://localhost:9200/shakespeare/_search?pretty=true&q=sword>
- Busqueda compleja con operador, frase exacta y boost
  - <http://localhost:9200/shakespeare/_search?pretty=true&q=text_entry:battle OR speaker:"EDMUND"^2>
  - <http://localhost:9200/shakespeare/_search?pretty=true&q=text_entry:king AND play_name:("King Lear" OR "Pericle")>
- Comodines:
  - <http://localhost:9200/shakespeare/_search?pretty=true&q=text_entry:??f?mous>
- Fuzziness:
  - <http://localhost:9200/shakespeare/_search?pretty=true&q=play_name:kong lea~3>
  
## Queries
- Buscar por titulo exacto:
```
```
- Eliminar resultados con cierto contenido:
```
```

## Extra
- https://www.elastic.co/guide/en/kibana/current/windows.html
```
```
