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
sudo vim /etc/elasticsearch/elasticsearch.yml
```
Cambiar:
 - node.name: "My First Node"
 - cluster.name: mycluster1
 - node.master: false
 - node.data: false
 - index.number_of_shards: 1
 - index.number_of_replicas: 0

## Manipular Documentos
- Crear un índice:
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
```
- Insertar un Documento:
```
curl -XPOST http://localhost:9200/shakespeare/line --header "Content-Type:application/json" --header "Accept: application/json" -d '
{
    "line_id" : 0,
    "play_name" : "Master Class",
    "speech_number" : 16,
    "line_number" : "1.1.1",
    "speaker" : "RICARDO",
    "text_entry" : "Insertemos un documento"
}
';
```
- Ver un Documento:
 - <http://localhost:9200/shakespeare/line/AV1kDezKznRrr8dRARwx?pretty=true>
- Modificar Parcialmente un Documento:
```
curl -XPATCH http://localhost:9200/shakespeare/line/AV1kDezKznRrr8dRARwx --header "Content-Type:application/json" --header "Accept: application/json" -d '
{
    "text_entry" : "Insertemos muchos documentos"
}
';
```
- Modificar Completamente un Documento:
```
curl -XPUT http://localhost:9200/shakespeare/line/AV1kDezKznRrr8dRARwx --header "Content-Type:application/json" --header "Accept: application/json" -d '
{
    "line_id" : 0,
    "play_name" : "Welcome to the Class",
    "speech_number" : 0,
    "line_number" : "1.1.1",
    "speaker" : "KEEPCODING",
    "text_entry" : "Elastic mola!"
}
';
```

## Insertar Datos en Batch
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
curl -XGET http://localhost:9200/shakespeare/_search?pretty=true --header "Content-Type:application/json" --header "Accept: application/json" -d '
{
  "query": {
    "match": {
      "play_name": "Henry IV"
    }
  }
}
';
```
- Eliminar resultados con cierto contenido:
```
curl -XGET http://localhost:9200/shakespeare/_search?pretty=true --header "Content-Type:application/json" --header "Accept: application/json" -d '
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "play_name": "King Lear"
          }
        }
      ],
      "must_not": [
        {
          "match_phrase": {
            "text_entry": "die"
          }
        }
      ]
    }
  }
}
';
```

## Extra
- https://www.elastic.co/guide/en/kibana/current/windows.html
```
sudo apt-get install apt-transport-https
echo "deb https://artifacts.elastic.co/packages/5.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-5.x.list
sudo apt-get update && sudo apt-get install kibana
kibana
```
