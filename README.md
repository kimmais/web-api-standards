# Guideline para criação de APIs Rest

### Conteúdo
1. [Guidelines](#guidelines)
2. [RESTful URLs](#restful-urls)
3. [Definir operações em termos de métodos HTTP](#definir-operações-em-termos-de-métodos-http)
4. [Response](#responses)


## Guidelines
Esse documento provê guideline e exemplos para as **HTTP API** Rest da KIM+.

Esse documento tem como base e referencias:
- [Architectural Styles and
the Design of Network-based Software Architectures](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm), by Roy Fielding
- [Api Facade Pattern](https://pages.apigee.com/rs/apigee/images/api-facade-pattern-ebook-2012-06.pdf), by Brian Mulloy
- [API Design](https://docs.microsoft.com/pt-br/azure/architecture/best-practices/api-design), by MicrosoftDocs

## RESTful URLs
### Diretrizes gerais para URLs RESTful
- A URL identifica um recurso.
- As URLs devem incluir substantivos, não verbos.
- Use substantivos no plural apenas para consistência(sem substantivos no singular).
- Use verbos HTTP (GET, POST, PUT, DELETE) para operar as coleções e elementos.
- Você não deve ir além de recurso/identificador/ recurso.
- Os formatos devem estar no formato  resource/{id}

### Exemplos de URLs
- Lista acomodações
  - GET http://api.example.com/customers
- Consulta com filtros
  - GET http://api.example.com/customers?namme=PAULO&sort=desc
  - GET http://api.example.com/customers?destination=paulo&company=tacom
- Consultando os reviews de um determinado hotal
  - GET http://api.example.com/customers/2d38df3c-8b37-4a6c-ac28-594806e30dc2/addresses
- Adicionando um novo review em um determinado hotel ou acomodação
  - POST http://api.example.com/customers/2d38df3c-8b37-4a6c-ac28-594806e30dc2/addresses

## Definir operações em termos de métodos HTTP

O protocolo HTTP define vários métodos que atribuem significado semântico a uma solicitação. Os métodos HTTP comuns usados pelas APIs da Web mais RESTful são:
 - GET, que recupera uma representação do recurso no URI especificado. O corpo da mensagem de resposta contém os detalhes do recurso solicitado.
- POST, que cria um novo recurso no URI especificado. O corpo da mensagem de solicitação fornece os detalhes do novo recurso. Observe que POST também pode ser usado para disparar operações que, na verdade, não criam recursos.
- PUT, que cria ou substitui o recurso no URI especificado. O corpo da mensagem de solicitação especifica o recurso a ser criado ou atualizado.
- PATCH, que realiza uma atualização parcial de um recurso. O corpo da solicitação especifica o conjunto de alterações a ser aplicado ao recurso.
- DELETE, que remove o recurso do URI especificado.

O efeito de uma solicitação específica deve depender de o recurso ser uma coleção ou um item individual. A tabela a seguir resume as convenções comuns adotadas pela maioria das implementações RESTful usando o exemplo de comércio eletrônico. Nem todas essas solicitações podem ser implementadas — depende do cenário específico.

|Recurso	|POST	|GET	|PUT	|DELETE|
|--------|------|-----|-----|------|
|/customers	|Criar um novo cliente	|Obter todos os clientes	|Atualização em massa de clientes	|Remover todos os clientes|
|/customers/1	|Erro|	Obter os detalhes do cliente 1	|Atualizar os detalhes do cliente 1 se ele existir	|Remover cliente 1|
|/customers/1/pedidos	|Criar um novo pedido para o cliente 1	|Obter todos os pedidos do cliente 1	|Atualização em massa de pedidos do cliente 1	|Remover todos os pedidos do cliente 1|


## Responses
- Sem valores nas chaves
- Sem nomes específicos internos (por exemplo, "nó" e "termo de taxonomia")
- Os metadados devem conter apenas propriedades diretas do conjunto de resposta, não propriedades dos membros do conjunto de resposta

### Exemplo
GET http://api.example.com/customers
```json
{
  "metadata":{
    "type":"list",
    "count":1,
    "offset":10,
    "limit": 10
  },
  
  "results":[
    {
     "id":"2d38df3c-8b37-4a6c-ac28-594806e30dc2",
     "name":"Blue Tree Premium Alphaville",
     "address":"Alameda Madeira, 398 - Alphaville, Barueri - SP, 06454-010",
     "reviews":[{
        "id":"e38def1b-3646-43ee-aa78-9de43c1fd61a",
        "subscriber_id":"b5f3b669-5b07-4c00-a357-c5b657ea753a",
        "subscriber_name":"b5f3b669-5b07-4c00-a357-c5b657ea753a",
        "ranking":3.5,
        "comments":"Nice"
     }],
     "photos":[{
        "id":"cef59c9b-461f-4342-bc2d-f7bbd6299d06",
        "title":"Blue Tree Premium Alphaville - Quarto",
        "description":"Quarto de Casal",
        "url":"https://api.example.comm/images/cef59c9b-461f-4342-bc2d-f7bbd6299d06"
      }]
    }
  ]
}
```

GET http://api.example.com/customers/2d38df3c-8b37-4a6c-ac28-594806e30dc2
```json
{
  "metadata":{
    "type":"object"
  },
  
  "results":{
     "id":"2d38df3c-8b37-4a6c-ac28-594806e30dc2",
     "name":"Blue Tree Premium Alphaville",
     "address":"Alameda Madeira, 398 - Alphaville, Barueri - SP, 06454-010",
     "reviews":[{
        "id":"e38def1b-3646-43ee-aa78-9de43c1fd61a",
        "subscriber_id":"b5f3b669-5b07-4c00-a357-c5b657ea753a",
        "subscriber_name":"b5f3b669-5b07-4c00-a357-c5b657ea753a",
        "ranking":3.5,
        "comments":"Nice"
     }],
     "photos":[{
        "id":"cef59c9b-461f-4342-bc2d-f7bbd6299d06",
        "title":"Blue Tree Premium Alphaville - Quarto",
        "description":"Quarto de Casal",
        "url":"https://api.example.comm/images/cef59c9b-461f-4342-bc2d-f7bbd6299d06"
      }]
    }
}
```
### Manipulação de erros
As respostas de erro devem incluir um código de status HTTP comum, mensagem para o desenvolvedor, ou mensagem para o usuário final (quando apropriado), código de erro interno (correspondente a algum ID determinado internamente).

Use três códigos de resposta simples e comuns indicando (1) sucesso, (2) falha devido a problema do lado do cliente, (3) falha devido a problema do lado do servidor:

| Status | Descrição                  | Retorno                                             |
|--------|----------------------------|-----------------------------------------------------|
|200     | OK                         | Retorna o conteudo solicitado                       |
|400     | Pedido inválido            | Retorna somente o status ou mensagem do usuário     |
|500     | Erro interno do servidor   | Retorna somente os status ou mensagem do usuário    |

### Mock-up Responses
É sugerido que cada recurso aceite um parâmetro 'sandbox' no servidor de teste. A passagem desse parâmetro deve retornar uma resposta de dados simulada (ignorando o back-end).
A implementação desse recurso no início do desenvolvimento garante que a API exibirá um comportamento consistente, suportando uma metodologia de desenvolvimento orientada a testes.

Observação: se o parâmetro simulado for incluído em uma solicitação para o ambiente de produção, um erro deve ser gerado.
