# Elasticsearch playground

## Cheatsheet
> https://elasticsearch-cheatsheet.jolicode.com/

`docker network create es-net --driver=bridge`

`docker run -d --name elasticsearch --net es-net -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.13.2`

`docker run --name kibana --net es-net -p 5601:5601 -e "ELASTICSEARCH_HOSTS=http://elasticsearch:9200" docker.elastic.co/kibana/kibana:7.13.2`

---

# Creating a index (like datasource) and a type (like table) on ES with a specific ID:
POST http://localhost:9200/index-study/people/1
```json
{
    "name": "Márcio",
    "likes": [ "tibia", "beber", "esporte", "futebol"],
    "city": "Araxá",
    "state": "SP",
    "degree": "Engenharia de Produção",
    "contry": "Brazil"
}
```

# Creating a index (like datasource) and a type (like table) on ES:

POST http://localhost:9200/index-study/people
```json
{
    "name": "Tadeu",
    "likes": [ "exercicios", "academia", "esporte", "tecnologia"],
    "city": "Araxá",
    "state": "MG",
    "degree": "Ciência da Computação",
    "contry": "Brazil"
}
```
```json
{
    "name": "José",
    "likes": [ "exercicios", "academia", "esporte", "tecnologia"],
    "city": "São José do rio preto",
    "state": "SP",
    "degree": "Ciência da Computação",
    "contry": "Brazil"
}
```

```json

{
    "name": "Branco",
    "likes": [ "exercicios", "academia", "esporte", "tecnologia"],
    "city": "São José do rio preto",
    "state": "SP",
    "degree": "Ciência da Computação",
    "contry": "Brazil"
}
```
# Updating the replicas to 0, since we're only using 1 instance

PUT http://localhost:9200/index-study/_settings
```json
{
    "index": {
        "number_of_replicas": 0
    }
}
```

# Getting count of documents

GET http://localhost:9200/index-study/people/_count

Response
```json
{
    "count": 3,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    }
}
```

# Getting documents and filtering

GET http://localhost:9200/index-study/people/_search?q=Tadeu

Response
```json
{
    "took": 18,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 1,
            "relation": "eq"
        },
        "max_score": 1.2039728,
        "hits": [
            {
                "_index": "index-study",
                "_type": "people",
                "_id": "TSG9aHoB_wW61CPyiUqm",
                "_score": 1.2039728,
                "_source": {
                    "name": "Tadeu",
                    "likes": [
                        "exercicios",
                        "academia",
                        "esporte",
                        "tecnologia"
                    ],
                    "city": "Araxá",
                    "state": "MG",
                    "degree": "Ciência da Computação",
                    "contry": "Brazil"
                }
            }
        ]
    }
}
```

# Updating a single document with partial props
POST http://localhost:9200/index-study/people/TSG9aHoB_wW61CPyiUqm

Body
```json
{
    "doc": {
        "nome": "Tadeu Tupinambá"
    }
}
```

Response
```json
{
    "_index": "index-study",
    "_type": "people",
    "_id": "TSG9aHoB_wW61CPyiUqm",
    "_version": 2,
    "result": "updated",
    "_shards": {
        "total": 1,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 4,
    "_primary_term": 1
}
```