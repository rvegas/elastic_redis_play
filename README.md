# elastic_setup

## Instalar

```
sudo apt-get update
sudo apt-get install openjdk-7-jre
wget https://download.elastic.co/elasticsearch/elasticsearch/elasticsearch-1.7.2.deb
sudo dpkg -i elasticsearch-1.7.2.deb
```

## Configurar
```

```

## Insertar Datos
```
curl -XPUT http://localhost:9200/shakespeare -d '
{
 "mappings" : {
  "_default_" : {
   "properties" : {
    "speaker" : {"type": "string", "index" : "not_analyzed" },
    "play_name" : {"type": "string", "index" : "not_analyzed" },
    "line_id" : { "type" : "integer" },
    "speech_number" : { "type" : "integer" }
   }
  }
 }
}
';
wget https://download.elastic.co/demos/kibana/gettingstarted/shakespeare.json
curl -H 'Content-Type: application/x-ndjson' -XPOST 'localhost:9200/shakespeare/_bulk?pretty' --data-binary @shakespeare.json
```

## Ver Datos:

- Indices: http://localhost:9200/_cat/indices?v
- Index: http://localhost:9200/shakespeare?pretty=true
- Settings: http://localhost:9200/shakespeare/_settings?pretty=true
- Mappings: http://localhost:9200/shakespeare/_mapping?pretty=true

## Search
- Buscar en el campo por defecto `_all`:
  - http://localhost:9200/shakespeare/_search?pretty=true&q=sword
- Busqueda compleja con operador, frase exacta y boost
  - http://localhost:9200/shakespeare/_search?pretty=true&q=text_entry:battle OR speaker:"EDMUND"^2
  - http://localhost:9200/shakespeare/_search?pretty=true&q=text_entry:king AND play_name:("King Lear" OR "Pericle")
- Comodines:
  - http://localhost:9200/shakespeare/_search?pretty=true&q=text_entry:??f?mous
- Fuzziness:
  - http://localhost:9200/shakespeare/_search?pretty=true&q=play_name:kong lea~3
  
## Queries
