<!-- https://giovannireisnunes.wordpress.com/2016/10/28/json-em-shell-script/ -->

# Usando o `jq`

O `jq` funciona a partir de filtros, ou seja, você envia um arquivo JSON e ele extrai/processa aquilo de acordo com o(s) filtro(s) indicado(s) e exibindo o resultado em STDOUT. O filtro mais simples é o “.” :

```bash
$ echo $( cat mongodb.json | tr "[:space:]*" "\000" ) | jq '.'
{
  "service": {
    "name": "mongodb",
    "tags": [
      "database",
      "db",
      "nosql"
    ],
    "port": 28017,
    "check": {
      "tcp": "localhost:28017",
      "interval": "20s"
    }
  }
}
```

Exibindo o json em uma única linha:

```bash
$ echo $( cat mongodb.json | tr "[:space:]*" "\000" ) | jq '.'
{"service":{"name":"mongodb","tags":["database","db","nosql"],"port":28017,"check":{"tcp":"localhost:28017","interval":"20s"}}}
```

Para recuperar o valor da chave:

```bash
$ cat mongodb.json | jq '.service.name'
"mongodb"
```

```bash
$ cat resultado.json | jq '.metrics.iterations.count'
1351
```

ou

```bash
$ cat resultado.json | jq '.metrics.iterations.rate'
6.723486782130118
```

Para listar todos os elementos dentro da chave “tags”, que é um array

```bash
$ cat mongodb.json | jq --compact-output '.service.tags'
["database","db","nosql"]
```

```bash
$ cat resultado.json | jq --compact-output '.metrics.iterations'
{"count":1351,"rate":6.723486782130118}
```

Desejando apenas saber quantos elementos o array possui:

```bash
$ cat mongodb.json | jq '.service.tags | length'
3
```

E para recuperar elementos específicos, no caso o primeiro e o terceiro, dentro do array:

```bash
$ cat mongodb.json | jq '.service.tags[0,2]'
"database"
"nosql"
```

Que tal somar 10 ao valor 28017 da chave “port”? Isto pode ser feito assim:

```bash
$ cat mongodb.json | jq '.service.port + 10'
28027
```

Para listar as chaves presentes dentro do arquivo JSON, faça:

```bash
$ cat mongodb.json | jq -c '.service | keys'
["check","name","port","tags"]
```

```bash
$ cat resultado.json | jq -c '.metrics.iterations | keys'
["count","rate"]
```
