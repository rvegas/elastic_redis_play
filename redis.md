# Elastic Search

## Instalar

```
```

## Configurar
```
```
Cambiar:

## Manipular Documentos
- Crear un índice:
```
```
- Insertar un Documento:
```
```
- Ver un Documento:
 - <http://localhost:9200/shakespeare/line/AV1kDezKznRrr8dRARwx?pretty=true>
- Modificar Parcialmente un Documento:
```
```
- Modificar Completamente un Documento:
```
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
